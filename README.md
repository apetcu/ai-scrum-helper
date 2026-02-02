# Scrum Master AI Agent 🎯

A **100% markdown-based**, **assistant-agnostic** AI agent that helps agile teams automate sprint ceremonies and surface actionable insights using MCP tools (Jira and Confluence).

## Why This Project?

Scrum Masters spend significant time on:
- Analyzing sprint health and identifying bottlenecks
- Detecting stale work, scope creep, and estimation gaps
- Translating technical work into business value for stakeholders
- Generating sprint review summaries

**This agent automates those tasks** - no code, just natural language instructions that any AI assistant can follow.

## Features

### 🔍 Sprint Health Checks (`/health`)
- Identify **stale tickets** (no movement in 3+ days)
- Detect **scope creep** (items added after sprint start)
- Flag **unpointed work** (missing estimates)
- Generate **actionable recommendations** for the team

### 📊 Stakeholder Summaries (`/summary`)
- Fetch all completed work from the sprint
- Group by epic/theme for cohesive narrative
- Translate technical work into **business value**
- Generate executive-ready summaries with metrics

### 🚀 Automated Onboarding (`/onboard`)
- Discover Jira project, board, and active sprint automatically
- Fetch team norms from Confluence (Definition of Done, Working Agreement, etc.)
- Map custom fields (story points, epic link, business value)
- Save configuration to PROJECT_CONTEXT.md for reuse

## Quick Start

### Prerequisites

1. **Install MCP Servers**:
   - Jira MCP server (for fetching sprint issues, custom fields)
   - Confluence MCP server (for team norms, optional)

2. **AI Assistant**:
   - Claude Code (with slash command support)
   - OR Cursor IDE (with rules support)
   - OR any AI assistant with MCP tool access

3. **Jira/Confluence Access**:
   - Read access to your Jira project and board
   - Read access to Confluence team documentation (optional)

### Installation

1. **Clone or copy this repository** into your project:
   ```bash
   git clone <repository-url>
   cd ai-scrum-helper
   ```

2. **No dependencies to install** - it's 100% markdown!

### First-Time Setup

1. **Run the onboarding command**:
   ```
   /onboard
   ```
   This will:
   - Discover your Jira project key, board ID, and active sprint
   - Fetch team norms from Confluence
   - Map custom fields (story points, epic link, etc.)
   - Save everything to `scrum-master/PROJECT_CONTEXT.md`

2. **You're ready!** Now you can use `/health` and `/summary` commands.

## Usage

### Command Reference

| Command | Description | When to Use |
|---------|-------------|-------------|
| `/onboard` | Discover and save Jira/Confluence configuration | First time setup, or when project/sprint changes |
| `/health` | Check sprint health (stale tickets, scope creep, unpointed work) | Daily or mid-sprint to identify risks |
| `/summary` | Generate stakeholder-ready sprint review summary | At sprint end or for retrospectives |

### Example Workflow

#### 1. Onboard to Your Project
```
/onboard
```

**Output**:
```
✅ Onboarding complete!

I've discovered and saved your configuration:
- Project: ECOM (E-Commerce Platform)
- Board: Board 42
- Active Sprint: Sprint 23 (Jan 27 - Feb 9)
- Custom Fields: Story Points, Epic Link mapped
- Team Norms: Loaded from Confluence space "TEAM"

Next steps: Run /health or /summary
```

#### 2. Check Sprint Health (Mid-Sprint)
```
/health
```

**Output**:
```markdown
# Sprint Health Check - Sprint 23

## Stale Tickets (3 found)
| Key | Summary | Assignee | Status | Last Updated |
|-----|---------|----------|--------|--------------|
| PROJ-123 | Implement payment gateway | Alice | In Progress | 6 days ago |
| PROJ-456 | Add error handling | Alice | In Progress | 4 days ago |

Recommendation: Schedule a sync with Alice to identify blockers.

## Scope Creep (2 items added after start)
- PROJ-234: Fix critical login bug (added +2 days into sprint)
- PROJ-567: Update marketing banner (added +3 days into sprint)

13% scope creep detected - discuss with Product Owner.

## Recommendations
1. Unblock Alice's work on PROJ-123 and PROJ-456
2. Review whether PROJ-567 should stay in sprint
3. Ensure all work is estimated before taking it into sprint
```

#### 3. Generate Sprint Summary (Sprint End)
```
/summary
```

**Output**:
```markdown
# Sprint Review Summary - Sprint 23

## Executive Summary
In Sprint 23, we successfully achieved our goal of implementing the new checkout flow and payment gateway integration. The team delivered 12 stories (21 points) that significantly improve the purchase experience for our customers.

## What We Delivered

### Enhanced Checkout Experience
- One-click checkout: Returning customers can now complete purchases with a single click, reducing checkout time by 60%
- Guest checkout: New customers no longer need to create an account, removing a major barrier to purchase

### Payment Gateway Integration
- Stripe integration: Support for credit cards, Apple Pay, and Google Pay
- Multi-currency support: 10 currencies now available for international customers

## Sprint Metrics
- Stories Completed: 12
- Story Points Delivered: 21 (above our average of 19)
- Sprint Goal Achievement: 100%
```

## Architecture

### Directory Structure

```
/
├── .claude/                        # Claude Code configuration
│   ├── context.md                  # Project context
│   └── commands/                   # Custom slash commands
│       ├── onboard.md              # /onboard command
│       ├── health.md               # /health command
│       └── summary.md              # /summary command
├── .cursor/                        # Cursor IDE configuration
│   └── rules/
│       └── index.mdc               # Main Cursor rules
├── scrum-master/                   # Core Scrum Master Agent logic
│   ├── AGENTS.md                   # Agent persona and behavior
│   ├── project.md                  # Project configuration template
│   ├── PROJECT_CONTEXT.md          # Actual context (gitignored)
│   └── playbooks/                  # Instruction playbooks
│       ├── onboard.md              # Onboarding logic
│       ├── sprint-health.md        # Health check logic
│       └── review-summary.md       # Summary logic
├── .gitignore                      # Ignore PROJECT_CONTEXT.md
├── CLAUDE.md                       # Root-level Claude context
└── README.md                       # This file
```

### How It Works

1. **Slash Commands** (`.claude/commands/` or `.cursor/rules/`):
   - Define what the command does
   - Reference the playbook to execute

2. **Playbooks** (`scrum-master/playbooks/`):
   - Step-by-step instructions for the AI
   - Use semantic language: "Invoke the Jira tool to fetch issues where..."
   - Include example MCP tool calls

3. **Agent Persona** (`scrum-master/AGENTS.md`):
   - Defines behavior: proactive, action-oriented, high-level
   - Communication style: business language for stakeholders, agile terminology for team

4. **State Management** (`scrum-master/PROJECT_CONTEXT.md`):
   - Single source of truth for Jira/Confluence configuration
   - Created during `/onboard`, persists between sessions
   - Gitignored (user-specific)

### Design Philosophy

#### 100% Markdown
- **No code** - only natural language instructions
- Any AI assistant with MCP tool access can follow the playbooks
- Easy to read, modify, and extend

#### Assistant-Agnostic
- Works with Claude Code, Cursor, and other AI assistants
- Uses semantic language: "Invoke the Jira tool" (not tool-specific syntax)
- IDE-specific config files reference the same core playbooks

#### MCP-Powered
- Relies on Model Context Protocol (MCP) for tool access
- Currently uses Jira and Confluence MCP servers
- Extensible to other tools (GitHub, Linear, Notion, etc.)

#### Proactive Persona
- Doesn't just report data - provides actionable recommendations
- Suggests specific conversations: "Talk to Alice about blockers on PROJ-123"
- Focuses on outcomes, not outputs

## Configuration

### For Claude Code

Slash commands are defined in `.claude/commands/`:
- `onboard.md` → `/onboard`
- `health.md` → `/health`
- `summary.md` → `/summary`

Each command references its playbook in `scrum-master/playbooks/`.

### For Cursor IDE

Rules are defined in `.cursor/rules/index.mdc` with frontmatter:

```yaml
---
description: "Scrum Master AI Agent - Use MCP tools and follow playbooks"
globs: ["**/*"]
alwaysApply: true
---
```

The rule instructs Cursor to:
- Use `scrum-master/PROJECT_CONTEXT.md` as source of truth
- Follow playbooks in `scrum-master/playbooks/`
- Maintain Scrum Master persona

### For Other AI Assistants

1. **Read** `CLAUDE.md` for project overview
2. **Follow** playbooks in `scrum-master/playbooks/`
3. **Reference** `scrum-master/AGENTS.md` for persona and behavior
4. **Use** `scrum-master/PROJECT_CONTEXT.md` for configuration

## Extending the Agent

### Add a New Command

Want to add a `/retrospective` command? Here's how:

1. **Create the playbook**: `scrum-master/playbooks/retrospective.md`
   - Define step-by-step instructions
   - Use MCP tools to fetch relevant data
   - Provide actionable output

2. **Add Claude Code command**: `.claude/commands/retrospective.md`
   - Command name: `/retrospective`
   - Description: "Generate retrospective insights"
   - Reference the playbook

3. **Update Cursor rules**: `.cursor/rules/index.mdc`
   - Add reference to new playbook

4. **Test it**: Run `/retrospective` and verify output

### Example: Retrospective Playbook

```markdown
# Retrospective Playbook

## Purpose
Analyze sprint data to generate retrospective discussion topics.

## Step 1: Load Context
Read scrum-master/PROJECT_CONTEXT.md for sprint info.

## Step 2: Fetch Sprint Data
Invoke: jira.get_sprint_issues(sprint_id=ACTIVE_SPRINT_ID)
Analyze: cycle time, blockers, velocity trends

## Step 3: Generate Discussion Topics
- What went well: Velocity above average, no critical bugs
- What didn't go well: 3 tickets stale for >5 days
- Action items: Improve estimation, add daily blocker check

## Step 4: Format Output
Present as markdown with discussion questions.
```

### Add Support for New Tools

The playbooks use MCP tools abstractly. To add GitHub support:

1. **Install GitHub MCP server**
2. **Update playbooks** to reference GitHub tools:
   - `github.get_pull_requests(repo=?, state="open")`
   - `github.get_issues(repo=?, labels=["bug"])`
3. **Update PROJECT_CONTEXT.md template** to include GitHub repo info

No code changes needed!

## MCP Tool Reference

### Jira MCP Tools (Expected)

```
jira.get_project(project_key: string)
jira.get_boards(project_key?: string)
jira.get_sprints(board_id: string)
jira.get_sprint_issues(sprint_id: string, status?: string, fields?: string[])
jira.get_fields()
jira.search_issues(jql: string, fields?: string[])
```

### Confluence MCP Tools (Expected)

```
confluence.list_spaces()
confluence.search_pages(space_key: string, query: string)
confluence.get_page(page_id: string)
```

**Note**: Atlassian official MCP servers may not exist yet. Check the [MCP ecosystem](https://github.com/modelcontextprotocol) for community implementations.

## Troubleshooting

### "No active sprint found"
- Run `/onboard` to refresh configuration
- Check that your sprint is marked as "Active" in Jira

### "Confluence not available"
- Confluence MCP server may not be installed
- Team norms can be added manually to PROJECT_CONTEXT.md

### "Custom field not found"
- Some organizations don't use certain fields (e.g., Business Value)
- The playbook will mark them as "N/A" and continue

### "PROJECT_CONTEXT.md doesn't exist"
- Run `/onboard` first to create it
- Or manually create it using `scrum-master/project.md` as a template

## Contributing

Contributions are welcome! Here's how you can help:

### Report Issues
- Found a bug? Open an issue on GitHub
- Include: Which command, what happened, what you expected

### Suggest Improvements
- Have an idea for a new playbook? Open a discussion
- Want to improve the agent persona? Submit a PR

### Add Playbooks
- Create new playbooks for other ceremonies (retrospective, planning, etc.)
- Share your custom commands with the community

### Guidelines
- **Keep it markdown**: No code, only natural language instructions
- **Be assistant-agnostic**: Use semantic language, not tool-specific syntax
- **Test thoroughly**: Try your playbook with real Jira/Confluence data
- **Document well**: Add examples and edge cases to your playbook

## FAQ

### Is this production-ready?
Yes! It's being used by teams to automate sprint ceremonies. However:
- Atlassian MCP servers may not be officially available yet (check community implementations)
- Playbooks assume certain Jira fields exist (story points, epic link) - customize as needed

### Does this work with GitHub Issues?
Not out of the box, but it's extensible. You'd need to:
- Install a GitHub MCP server
- Update playbooks to use GitHub tools instead of Jira
- Modify PROJECT_CONTEXT.md to store GitHub repo info

### Can I use this without Confluence?
Yes! Confluence is optional. If not available:
- Team norms sections in PROJECT_CONTEXT.md will be empty
- You can manually add team norms to the file

### How do I customize the staleness threshold?
In `scrum-master/playbooks/sprint-health.md`, change the threshold:
```markdown
Calculate staleness threshold:
- Current date minus 3 days = threshold date
```
Change `3 days` to your preferred threshold (e.g., `5 days`).

### Can I use this with Linear, Asana, or other tools?
Yes, if MCP servers exist for those tools. The playbooks use semantic language like "Invoke the project management tool", so they can be adapted to any MCP-compatible tool.

## License

MIT License

Copyright (c) 2026

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

## Acknowledgments

This project is inspired by:
- **OpenSpec**: Spec-driven development framework for AI coding assistants
- **Model Context Protocol (MCP)**: Standardized interface for AI tools
- **Claude Code**: AI-powered command-line assistant

### Resources
- [OpenSpec](https://openspec.dev/)
- [Claude Code Documentation](https://code.claude.com/docs/)
- [Cursor IDE](https://cursor.com/)
- [Model Context Protocol](https://github.com/modelcontextprotocol)

---

**Ready to automate your sprint ceremonies?** Run `/onboard` to get started! 🚀

Questions or feedback? Open an issue or discussion on GitHub!
