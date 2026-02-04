# Scrum Master AI Agent - Project Configuration Template

> **Note**: This is a **template file** that shows the structure of PROJECT_CONTEXT.md.
>
> The actual PROJECT_CONTEXT.md file is created during the `/onboard` command and is gitignored (user-specific).

---

# Project Context - [Your Project Name]

## About This File

This file serves as the **single source of truth** for all Scrum Master AI Agent operations. It is populated automatically during the `/onboard` command by discovering your Jira and Confluence configuration.

**Last Updated**: [Date] _(Auto-updated during onboarding)_

---

## Project Metadata

- **Project Name**: [Your project name, e.g., "E-Commerce Platform"]
- **Organization**: [Your organization name]
- **Team**: [Your team name]

---

## Jira Configuration

### Project Settings
- **Project Key**: `YOUR_PROJECT_KEY` _(e.g., ECOM, PROJ, etc.)_
- **Board ID**: `YOUR_BOARD_ID` _(e.g., 123)_
- **Active Sprint ID**: `YOUR_ACTIVE_SPRINT_ID` _(e.g., 456)_

### Sprint Information
- **Sprint Name**: Sprint X _(e.g., "Sprint 42", "PI 3.2 Iteration 1")_
- **Sprint Goal**: [The sprint goal/objective]
- **Start Date**: YYYY-MM-DD
- **End Date**: YYYY-MM-DD

---

## Team Information

### Team Members
- **Team Name**: [Team name, e.g., "Payment Services Team"]
- **Scrum Master**: [Name] _(optional)_
- **Product Owner**: [Name] _(optional)_

### Stakeholders
List key stakeholders who should receive sprint summaries:
- [Stakeholder 1 name/role]
- [Stakeholder 2 name/role]
- [Stakeholder 3 name/role]

---

## Custom Field Mappings

Jira custom fields vary by organization. This section maps custom field IDs to semantic names for the playbooks to use.

### Discovered Fields
- **Story Points**: `customfield_XXXXX` _(e.g., customfield_10016)_
- **Epic Link**: `customfield_XXXXX` _(e.g., customfield_10014)_
- **Business Value**: `customfield_XXXXX` _(e.g., customfield_10020, or "N/A" if not used)_
- **Sprint**: `customfield_XXXXX` _(e.g., customfield_10010)_

### Additional Custom Fields
_(Add any other important custom fields your organization uses)_
- **[Field Name]**: `customfield_XXXXX`
- **[Field Name]**: `customfield_XXXXX`

**Note**: Custom field IDs are discovered automatically during `/onboard` using the Jira MCP `get_fields()` tool.

---

## Team Norms

These are fetched from Confluence during the onboarding process. If your team doesn't have these documented in Confluence, you can manually add them here.

### Definition of Ready (DoR)
A user story is "Ready" when:
- [ ] [Criterion 1, e.g., "User story is written in the format: As a... I want... So that..."]
- [ ] [Criterion 2, e.g., "Acceptance criteria are clearly defined"]
- [ ] [Criterion 3, e.g., "Dependencies are identified and resolved"]
- [ ] [Criterion 4, e.g., "Story is estimated by the team"]
- [ ] [Add more criteria as needed]

### Definition of Done (DoD)
A user story is "Done" when:
- [ ] [Criterion 1, e.g., "Code is written and meets coding standards"]
- [ ] [Criterion 2, e.g., "Unit tests are written and passing"]
- [ ] [Criterion 3, e.g., "Code is peer-reviewed and approved"]
- [ ] [Criterion 4, e.g., "Changes are deployed to staging"]
- [ ] [Criterion 5, e.g., "Product Owner has accepted the work"]
- [ ] [Add more criteria as needed]

### Working Agreement
Our team agrees to:
- [Agreement 1, e.g., "Respond to messages within 2 hours during working hours"]
- [Agreement 2, e.g., "Update ticket status daily"]
- [Agreement 3, e.g., "No meetings before 9 AM"]
- [Agreement 4, e.g., "Blockers are raised in stand-up or Slack immediately"]
- [Add more agreements as needed]

### Ceremony Schedule
- **Sprint Length**: [Duration, e.g., "2 weeks"]
- **Daily Stand-up**: [Time, e.g., "9:00 AM daily"]
- **Sprint Planning**: [Time, e.g., "Monday 10:00 AM at sprint start"]
- **Sprint Review**: [Time, e.g., "Friday 2:00 PM at sprint end"]
- **Sprint Retrospective**: [Time, e.g., "Friday 3:30 PM at sprint end"]
- **Backlog Refinement**: [Time, e.g., "Wednesday 1:00 PM weekly"]

---

## Issue Templates

Templates are defined in `scrum-master/templates/` and enforced during `/health` and `/validate` commands.

### Template Enforcement
- **Enabled**: Yes _(Set to "No" to skip template validation in health checks)_
- **Strictness**: Medium _(Options: Strict, Medium, Relaxed)_
  - **Strict**: Flag all template violations
  - **Medium**: Flag only critical violations (default)
  - **Relaxed**: Flag only the most severe violations

### Template Customization
The templates in `scrum-master/templates/` can be customized per team:
- **Bug Template**: `scrum-master/templates/bug-template.md`
- **Story Template**: `scrum-master/templates/story-template.md`
- **Epic Template**: `scrum-master/templates/epic-template.md`

Edit these files to match your team's specific requirements.

---

## Configuration Notes

### Confluence Integration
- **Confluence Space**: [Space key, e.g., "TEAM" or "PROJ"]
- **Team Norms Page**: [Page ID or title where norms are documented]
- **Status**: [e.g., "Connected" or "Not Available"]

### Onboarding History
- **First Onboarded**: [Date]
- **Last Re-Onboarded**: [Date] _(Re-run `/onboard` to refresh configuration)_

---

## Usage Notes

### For AI Assistants
- **Read this file** before executing any `/health` or `/summary` commands
- Use the custom field mappings when querying Jira via MCP tools
- Reference team norms when providing recommendations
- Use stakeholder list when formatting sprint review summaries

### For Users
- **Update this file manually** if your sprint changes or custom fields are added
- **Re-run `/onboard`** to automatically refresh Jira/Confluence configuration
- **Keep this file gitignored** - it contains project-specific configuration that may vary per user

---

## Example Configuration

Below is an example of a fully populated configuration:

```markdown
# Project Context - E-Commerce Platform

**Last Updated**: 2026-02-02

## Project Metadata
- **Project Name**: E-Commerce Platform
- **Organization**: Acme Corp
- **Team**: Payment Services Team

## Jira Configuration
- **Project Key**: ECOM
- **Board ID**: 42
- **Active Sprint ID**: 128
- **Sprint Name**: Sprint 23
- **Sprint Goal**: Implement new checkout flow and payment gateway integration
- **Start Date**: 2026-01-27
- **End Date**: 2026-02-09

## Team Information
- **Team Name**: Payment Services Team
- **Stakeholders**: Jane Doe (VP Product), John Smith (CTO), Alice Johnson (Customer Success Lead)

## Custom Field Mappings
- **Story Points**: customfield_10016
- **Epic Link**: customfield_10014
- **Business Value**: customfield_10020
- **Sprint**: customfield_10010

## Team Norms
### Definition of Done
- Code is peer-reviewed
- Unit tests are passing
- Changes are deployed to staging
- Product Owner has accepted

(... and so on)
```

---

**Ready to get started?** Run the `/onboard` command to populate this configuration automatically!
