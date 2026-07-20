---
name: proton-pass-secrets
description: Read secrets (API tokens, passwords, TOTP codes) from Proton Pass using the pass-cli and secret references like pass://vault/item/field. Use whenever the user provides a pass:// reference, mentions a credential/token/API key stored in Proton Pass, asks to run a command or investigate an API using a secret from their vault, or wants secrets injected into environment variables, .env files, or config templates — even if they don't name pass-cli explicitly.
---

# Reading Secrets from Proton Pass

The Proton Pass CLI (`pass-cli`) resolves secret references so credentials never need to be pasted into a conversation, a script, or shell history. The guiding principle: **the plaintext secret should never appear in your output, the conversation context, or any committed file.** `pass-cli run` exists precisely for this — it injects secrets into a child process's environment and masks any secret values that leak to stdout/stderr.

## Secret reference format

```
pass://<vault>/<item>/<field>
```

- **vault** — vault name or share ID (e.g. `Personal`)
- **item** — item title or item ID (e.g. `hardcover.app`)
- **field** — `username`, `password`, `email`, `url`, `note`, `totp`, or a custom field name

Details that bite:
- All three components are required for a secret reference; trailing slashes are invalid.
- Common field names are case-sensitive; custom field names match case-insensitively.
- Fields inside sections use `SectionName.fieldname`.
- Names with spaces are allowed. If multiple items match a name, the first match wins — prefer IDs when ambiguous.
- TOTP: `?totp=code` (default) returns the computed 6-digit code; `?totp=uri` returns the raw `otpauth://` URI.

## Preflight

Before anything else:

```bash
pass-cli info
```

- **Command not found** → install with `curl -fsSL https://proton.me/download/pass-cli/install.sh | bash`
- **Not logged in** → login is interactive; the user must run `pass-cli login` themselves. In Claude Code, suggest they type `! pass-cli login` so it runs inside the session.
- **Session locked** → the user must run `pass-cli session unlock`.
- **Headless/CI** → authenticate with a personal access token instead: `export PROTON_PASS_PERSONAL_ACCESS_TOKEN=pst_xxx::TOKENKEY` then `pass-cli login`. Tokens are created with `pass-cli pat create --name NAME --expiration 3m` and granted vault access with `pass-cli pat access grant --pat-name NAME --vault-name VAULT --role viewer`.

## Using a secret in a command (preferred)

Put the *reference* in an environment variable, then let `pass-cli run` resolve it inside the child process:

```bash
export HARDCOVER_API_TOKEN='pass://Personal/hardcover.app/HARDCOVER_API_TOKEN'
pass-cli run -- sh -c 'curl -s https://api.hardcover.app/v1/graphql \
  -H "authorization: Bearer $HARDCOVER_API_TOKEN" \
  -H "content-type: application/json" \
  -d "{\"query\":\"{ me { username } }\"}"'
```

Why this exact shape:

- The parent shell's environment only ever holds the `pass://` URI, not the secret.
- `pass-cli run` scans every environment variable for `pass://` references and injects the resolved values into the child process.
- The inner command is **single-quoted** so `$HARDCOVER_API_TOKEN` expands in the child `sh` — *after* injection. With double quotes, the parent shell would expand it first, and the literal URI string would be sent as the token.
- Masking is on by default: if the secret value appears in the child's stdout/stderr, it is replaced with `<concealed by Proton Pass>`. Leave masking on — it's what keeps the secret out of agent context and logs. Don't reach for `--no-masking`.

References can also be embedded inside larger values, and `run` resolves them in place:

```bash
export DATABASE_URL='postgresql://app:pass://Personal/prod-db/password@db.example.com/app'
pass-cli run -- ./migrate
```

For repeated API calls (e.g. exploring a GraphQL API), don't re-export each time — write a small script that reads the env var and invoke it under `pass-cli run` per call, or run a longer-lived process (REPL, dev server) under a single `pass-cli run`.

### Hardening scripts that consume injected secrets

When you write a script the user will run under `pass-cli run`, two touches make it meaningfully safer:

**Guard against running outside `pass-cli run`.** If the script runs directly, the variable is either empty or still holds the literal `pass://` URI — and that URI would get sent to the API as the "token". Catch both:

```bash
case "${HARDCOVER_API_TOKEN:-}" in
  "")        echo "HARDCOVER_API_TOKEN not set — run via: pass-cli run -- ./query.sh" >&2; exit 1 ;;
  pass://*)  echo "Reference was not resolved — run via: pass-cli run -- ./query.sh" >&2; exit 1 ;;
esac
```

**Keep the secret out of the process argument list.** `-H "Authorization: Bearer $TOKEN"` puts the resolved secret into argv, visible to any local process via `ps` or `/proc` for the duration of the request. Env vars don't have that problem, so feed the header to curl on stdin instead of the command line:

```bash
curl -s "$ENDPOINT" \
  -H "content-type: application/json" \
  -d '{"query":"{ me { username } }"}' \
  --config - <<CURLCFG
header = "authorization: Bearer ${HARDCOVER_API_TOKEN}"
CURLCFG
```

The same principle applies to other tools: prefer ones that read credentials from the environment or stdin over passing them as flags.

## .env files with references

A `.env` file holding only references contains no secrets, so it is safe to commit:

```bash
# .env — references only, no plaintext secrets
HARDCOVER_API_TOKEN=pass://Personal/hardcover.app/HARDCOVER_API_TOKEN

pass-cli run --env-file .env -- npm run dev
```

Multiple `--env-file` flags are allowed; later files override earlier ones.

Two practical notes:

- Most repos gitignore `.env` by convention because it normally holds plaintext. If the project already does, name the reference file something like `.env.pass` and force-include it (`!.env.pass` in `.gitignore`) rather than fighting the convention — the point is that *this* file is committable precisely because it holds only references.
- To keep the plain `npm run dev` muscle memory working, wrap the underlying command in `package.json` instead of asking the user to type the `pass-cli run` prefix:

  ```json
  { "scripts": { "dev": "pass-cli run --env-file .env.pass -- node server.js" } }
  ```

## Config file templates

When a tool needs secrets in a config *file* rather than the environment, use `pass-cli inject`. It only substitutes references wrapped in double braces (bare `pass://` URIs are ignored, unlike `run`):

```yaml
# config.template.yaml
api_token: "{{ pass://Personal/hardcover.app/HARDCOVER_API_TOKEN }}"
```

```bash
pass-cli inject -i config.template.yaml -o config.yaml
```

The output file is written with `0600` permissions, but it contains plaintext secrets: don't `cat` it, don't commit it, and delete it when the task is done.

## Printing a value (last resort)

```bash
pass-cli item view "pass://Personal/hardcover.app/HARDCOVER_API_TOKEN"
```

This prints the plaintext secret to stdout. Only do this when the user explicitly asks to see the value — and note to them that the value will then live in the conversation and terminal history. Two related traps:

- `item view` on an item *without* a field (or with `--output json`) prints **every** field, including passwords. Don't use it as a discovery tool.
- Never echo a resolved secret into a file or command yourself; route it through `run` or `inject` instead.

## Finding the right reference

When the user gives a vague location ("my Hardcover token is in my Personal vault") rather than a full reference:

```bash
pass-cli vault list
pass-cli item list --vault-name "Personal" --output json
```

`item list` returns titles and IDs (not secret values), so it's safe for discovery. Build the reference from the vault name + item title + field. If you can't guess the field name (`password` and custom fields like `HARDCOVER_API_TOKEN` are common), ask the user rather than dumping the full item, since a full item view exposes every secret in it.

## Troubleshooting

| Symptom | Likely cause / fix |
|---|---|
| `run` passes the literal `pass://...` through | Reference must be the env var's value (or embedded in it) — `run` does not resolve URIs in command arguments. Also check all three components are present and there's no trailing slash. |
| Field not found | Common field names are case-sensitive — try `password`, `username`, `note`. Custom fields match case-insensitively. Section fields need `Section.field`. |
| Every API operation fails | Session is locked — user runs `pass-cli session unlock`. |
| Works locally, fails in CI | No interactive session — use a personal access token (see Preflight). |
| Wrong item resolved | Duplicate titles — first match wins. Use `--item-id`/share ID or a `pass://SHARE_ID/ITEM_ID/field` reference. |
