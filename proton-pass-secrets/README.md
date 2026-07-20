# Proton Pass Secrets

A Claude Code plugin for reading secrets from [Proton Pass](https://proton.me/pass) via the [Proton Pass CLI](https://protonpass.github.io/pass-cli/).

Give an agent a secret reference like:

```
pass://Personal/hardcover.app/HARDCOVER_API_TOKEN
```

and it will resolve the credential through `pass-cli run` — injecting the value into the child process environment with output masking on, so the plaintext secret never lands in the conversation, shell history, or committed files.

## What it covers

- Secret reference syntax (`pass://vault/item/field`, TOTP query params, section-qualified fields)
- Running commands with injected secrets (`pass-cli run`)
- Reference-only `.env` files that are safe to commit
- Config file templating (`pass-cli inject`)
- Discovering vault/item/field names safely (`vault list`, `item list`)
- Auth preflight: login, session locks, and personal access tokens for CI

## Requirements

- [`pass-cli`](https://protonpass.github.io/pass-cli/get-started/installation/) installed and logged in (`pass-cli login`)
