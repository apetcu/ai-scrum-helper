# Scrum Master AI Agent

## Project Overview

This is a **Scrum Master AI Agent** - a 100% markdown-based, assistant-agnostic system that helps automate agile ceremonies and provide actionable sprint insights.

## How It Works

The agent uses **MCP (Model Context Protocol) tools** to interact with:
- **Jira**: Fetch sprint data, issues, custom fields
- **Confluence**: Retrieve team norms and working agreements

All core logic and instruction playbooks are located in the [scrum-master/](../scrum-master/) folder.

## Configuration

Project-specific configuration is stored in [scrum-master/PROJECT_CONTEXT.md](../scrum-master/PROJECT_CONTEXT.md). This file contains:
- Jira project key, board ID, and active sprint ID
- Custom field mappings (story points, epic link, business value)
- Team norms fetched from Confluence
- Stakeholder information

**Note**: This file is created during the `/onboard` command and is gitignored (user-specific).

## Available Commands

This project provides three custom slash commands:

- `/onboard` - Onboard to a new Jira project and Confluence space
- `/health` - Check sprint health (stale tickets, scope creep, unpointed work)
- `/summary` - Generate sprint review summary for stakeholders

## Agent Persona

The AI should act as a **proactive Scrum Master** focused on:
- Identifying bottlenecks and blockers
- Highlighting business value delivered
- Detecting flow issues (stale work, scope creep)
- Communicating in stakeholder-friendly language

See [scrum-master/AGENTS.md](../scrum-master/AGENTS.md) for detailed agent behavior instructions.

## Design Philosophy

- **100% Markdown**: No code, only natural language instructions
- **Assistant-Agnostic**: Works with Claude Code, Cursor, and other AI assistants
- **MCP-Powered**: Extensible to other tools beyond Jira/Confluence
- **State Management**: PROJECT_CONTEXT.md acts as persistent state between sessions
