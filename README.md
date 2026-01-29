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

### Design

| Skill | Description |
|-------|-------------|
| [logo-ideation](./logo-ideation/) | Brand discovery and logo concept development. Use when brainstorming logo ideas, exploring visual directions, analyzing competitor logos, or developing logo concepts before creation. |
| [svg-logo-creator](./svg-logo-creator/) | Create professional SVG logos from concept briefs or descriptions. Use when generating SVG logo files, creating logo variations, or implementing logo designs. |

## Creating Skills

These skills are created following the best practices outlined in the [Agent Skills documentation](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview).
