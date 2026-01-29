# Skills

A collection of AI skills for Claude Code and other Claude agents.

## Installation

Install skills from this repository using the `npx skills` command:

```bash
npx skills add kbravh/skills
```

This will prompt you to select which skills to install.

## Available Skills

### Database

| Skill | Description |
|-------|-------------|
| [prisma-expand-contract](./prisma-expand-contract/) | Safe database schema migrations using the expand-and-contract pattern with Prisma ORM. Use when renaming columns/tables, changing column types, adding non-nullable columns, or any schema change requiring zero-downtime deployment. |

## Creating Skills

These skills are created following the best practices outlined in the [Agent Skills documentation](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview).
