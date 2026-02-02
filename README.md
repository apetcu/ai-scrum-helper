# Scrum Master AI Helper

**Experimental.** A markdown-based, assistant-agnostic AI agent that uses MCP (Jira and Confluence) to automate sprint health checks, onboarding, and stakeholder summaries. No code—only natural language instructions and playbooks.

## What it does

- **Sprint health** (`/health`): Stale tickets (no movement in 3+ days), scope creep, unpointed work, and recommendations.
- **Sprint summary** (`/summary`): Stakeholder-ready review of completed work, grouped by theme, with business-value language.
- **Onboarding** (`/onboard`): Discovers Jira/Confluence config (project, sprint, custom fields, team norms) and writes `scrum-master/PROJECT_CONTEXT.md`.

## Quick start

1. **MCP**: Use an MCP server that can talk to Jira (and optionally Confluence). For example, the Atlassian HTTP MCP at `https://mcp.atlassian.com/v1/mcp`.
2. **Assistant**: Use Cursor or another AI assistant that can run MCP tools and follow the playbooks.
3. **Run** `/onboard` once to create `PROJECT_CONTEXT.md`, then use `/health` and `/summary` as needed.

## Commands

| Command     | Purpose |
|------------|---------|
| `/onboard` | Discover and save Jira/Confluence config to `scrum-master/PROJECT_CONTEXT.md`. Run first or when project/sprint changes. |
| `/health`  | Analyze active sprint: stale tickets, scope creep, unpointed work; output markdown report and recommendations. |
| `/summary` | Generate sprint review summary (Done work, themes, metrics) for stakeholders. |

## Layout

- **`scrum-master/playbooks/`** – Step-by-step instructions (onboard, sprint-health, review-summary) that the AI follows.
- **`scrum-master/AGENTS.md`** – Persona and behavior (proactive, business language, actionable).
- **`scrum-master/PROJECT_CONTEXT.md`** – Created by `/onboard`; gitignored. Source of truth for project key, sprint, custom fields, team norms.
- **`.claude/commands/`** and **`.cursor/rules/`** – Wire slash commands and rules to the playbooks.

## Configuration

- **Cursor**: Rules in `.cursor/rules/index.mdc` point to `PROJECT_CONTEXT.md` and the playbooks.
- **Claude Code**: Slash commands in `.claude/commands/` invoke the same playbooks.
- **Other assistants**: Use `CLAUDE.md`, `scrum-master/playbooks/`, and `scrum-master/AGENTS.md`; config lives in `PROJECT_CONTEXT.md`.

## Extending

- **New command**: Add a playbook under `scrum-master/playbooks/`, then add a command/rule that references it.
- **New tools**: Playbooks are written in semantic terms (“fetch sprint issues”, “search Confluence”); adapt them to whatever MCP or API your Jira/Confluence integration exposes.

## Troubleshooting

- **No active sprint / PROJECT_CONTEXT missing**: Run `/onboard`. Ensure an active sprint and board exist in Jira.
- **Confluence empty**: Optional. You can fill team norms manually in `PROJECT_CONTEXT.md`.
- **Custom fields**: Playbooks assume common fields (e.g. story points, sprint). Map your field IDs in `PROJECT_CONTEXT.md`; `/onboard` tries to discover them.

## Disclaimer

This is a personal, open-source project built independently.
It does not include or rely on any confidential or proprietary information.

## License

MIT. See [LICENSE](LICENSE.md) or repository root for full text.
