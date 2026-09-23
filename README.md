# Skills

A collection of AI skills for Claude Code and other Claude agents.

## Installation

This repo is a Claude Code plugin marketplace. Add it once:

```bash
claude plugin marketplace add kbravh/skills
```

Then install a plugin. Pick the scope based on where you want it:

```bash
# Every project (user scope)
claude plugin install html-specs@kbravh-skills

# Only the current repo (writes to .claude/settings.json, so collaborators get it too)
claude plugin install tanstack-query@kbravh-skills --scope project
```

You can also browse and install from inside Claude Code with `/plugin`.

## Available Plugins

### Database

| Plugin | Description |
|-------|-------------|
| [prisma-expand-contract](./prisma-expand-contract/) | Safe database schema migrations using the expand-and-contract pattern with Prisma ORM. Use when renaming columns/tables, changing column types, adding non-nullable columns, or any schema change requiring zero-downtime deployment. |

### Design

| Plugin | Description |
|-------|-------------|
| [logo-design](./logo-design/) | Brand discovery and SVG logo creation. Two skills: `logo-ideation` develops concepts from brand research and competitor analysis, then `svg-logo-creator` turns the approved concept brief into SVG logos and variations. |

### Documentation

| Plugin | Description |
|-------|-------------|
| [html-specs](./html-specs/) | Create engaging, self-contained HTML slide decks for specs, implementation plans, ADRs, and brainstorm docs. Single-file, keyboard-navigable, dark slate + emerald design system with ready-made components (callouts, option grids, timelines, terminal blocks, diagrams). |

### Creative

| Plugin | Description |
|-------|-------------|
| [game-design-brainstorming](./game-design-brainstorming/) | A creative collaborator for video game design. Turns loose ideas, themes, and mechanics into gameplay loops, design pillars, and pitch documents. |

### Developer Tools

| Plugin | Description |
|-------|-------------|
| [proton-pass-secrets](./proton-pass-secrets/) | Read secrets from Proton Pass with pass-cli secret references (`pass://vault/item/field`). Use when running commands that need API tokens or passwords from a Proton Pass vault, injecting secrets into env vars or config templates, or resolving a `pass://` reference without exposing plaintext. |

### Data Fetching

| Plugin | Description |
|-------|-------------|
| [tanstack-query](./tanstack-query/) | TanStack Query (React Query) v5 best practices for data fetching, caching, and mutations. Use when fetching server data with useQuery, setting up mutations, configuring caching strategies, managing query keys, handling optimistic updates, or invalidating queries. |

## Creating Skills

These skills are created following the best practices outlined in the [Agent Skills documentation](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview).
