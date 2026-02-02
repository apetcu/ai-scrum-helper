# Scrum Master AI Agent

## What is This Project?

A **100% markdown-based** Scrum Master AI Agent that helps agile teams:
- Identify bottlenecks and stale work
- Detect scope creep and unpointed work
- Generate stakeholder-ready sprint summaries
- Automate sprint health checks

All using **MCP tools** (Jira and Confluence) - no code, just natural language instructions!

## Quick Start

### 1. Onboard to Your Project
Run the `/onboard` command to discover your Jira and Confluence configuration automatically.

### 2. Check Sprint Health
Run the `/health` command during your sprint to identify risks and bottlenecks.

### 3. Generate Sprint Summary
Run the `/summary` command at sprint end to create a stakeholder-ready review.

## How It Works

### Architecture
- **[.claude/](.claude/)**: Claude Code configuration (slash commands, context)
- **[.cursor/](.cursor/)**: Cursor IDE configuration (rules)
- **[scrum-master/](scrum-master/)**: Core agent logic (persona, playbooks, config)

### Agent Persona
The AI acts as a **proactive Scrum Master** focused on:
- **Bottlenecks**: What's slowing down the team?
- **Business value**: What impact did we deliver?
- **Flow issues**: Stale work, scope creep, estimation gaps
- **Stakeholder communication**: Clear, non-technical language

See [scrum-master/AGENTS.md](scrum-master/AGENTS.md) for detailed behavior.

### Instruction Playbooks
All logic is defined in markdown playbooks:
- [onboard.md](scrum-master/playbooks/onboard.md) - Discover and save project configuration
- [sprint-health.md](scrum-master/playbooks/sprint-health.md) - Analyze sprint health
- [review-summary.md](scrum-master/playbooks/review-summary.md) - Generate stakeholder summaries

### State Management
- **[scrum-master/PROJECT_CONTEXT.md](scrum-master/project.md)** (template) acts as the single source of truth
- It's created during `/onboard` and contains:
  - Jira project key, board ID, active sprint ID
  - Custom field mappings (story points, epic link, etc.)
  - Team norms from Confluence
  - Stakeholder information

**Note**: PROJECT_CONTEXT.md is gitignored (user-specific).

## Design Philosophy

### 100% Markdown
No code - only natural language instructions that command the AI to use MCP tools.

### Assistant-Agnostic
Works with Claude Code, Cursor, and any AI assistant that supports MCP tools.

### MCP-Powered
Extensible to other tools beyond Jira/Confluence (GitHub, Linear, Notion, etc.).

### Proactive Persona
The AI doesn't just report data - it provides actionable recommendations and suggests next steps.

## Available Commands

| Command | Description | When to Use |
|---------|-------------|-------------|
| `/onboard` | Discover Jira/Confluence configuration | First time setup or when project changes |
| `/health` | Check sprint health (stale tickets, scope creep) | During sprint (daily or mid-sprint) |
| `/summary` | Generate stakeholder-ready sprint review | At sprint end or for retrospectives |

## Configuration

### Claude Code
Custom slash commands are defined in [.claude/commands/](.claude/commands/).

### Cursor IDE
Rules are defined in [.cursor/rules/index.mdc](.cursor/rules/index.mdc).

### Both Assistants
Both reference the same core logic in [scrum-master/](scrum-master/).

## Prerequisites

### MCP Servers Required
- **Jira MCP**: For fetching sprint issues, custom fields, project configuration
- **Confluence MCP**: For retrieving team norms and working agreements

Install these MCP servers before using this agent.

### Jira Access
- Read access to your Jira project and board
- Ability to query issues and custom fields

### Confluence Access (Optional)
- Read access to team documentation space
- If not available, team norms can be added manually to PROJECT_CONTEXT.md

## Example Workflow

1. **First Time Setup**:
   ```
   /onboard
   ```
   → Discovers your Jira project, board, sprint, and Confluence team norms
   → Saves to scrum-master/PROJECT_CONTEXT.md

2. **Mid-Sprint Check**:
   ```
   /health
   ```
   → Identifies stale tickets (no movement in 3+ days)
   → Detects scope creep (items added after sprint start)
   → Flags unpointed work
   → Provides actionable recommendations

3. **Sprint Review**:
   ```
   /summary
   ```
   → Fetches all "Done" issues
   → Groups by epic/theme
   → Synthesizes business value narrative
   → Generates stakeholder-ready summary

## Extending the Agent

Want to add more playbooks? Easy!

1. Create a new playbook in `scrum-master/playbooks/` (e.g., `retrospective.md`)
2. Add a command in `.claude/commands/` (e.g., `retrospective.md`)
3. Reference the new playbook in `.cursor/rules/index.mdc`

**Example**: Add a `/retrospective` command that analyzes sprint data and suggests discussion topics.

## Contributing

See [README.md](README.md) for contribution guidelines and more detailed documentation.

## License

MIT License - see [README.md](README.md) for details.

---

**Ready to get started?** Run `/onboard` to set up your project configuration! 🚀
