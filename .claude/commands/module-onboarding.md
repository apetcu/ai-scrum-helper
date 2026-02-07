# /module-onboarding - Module Business Context Onboarding

## Description

Onboard to a specific module by discovering all relevant Confluence documentation and Jira stories, then saving everything into a single markdown file at `business/<module-name>.md`. This context is used to enhance bug fixes and feature work with deep business understanding.

## Instructions

Follow the detailed playbook located at:
**[scrum-master/playbooks/module-onboarding.md](../../scrum-master/playbooks/module-onboarding.md)**

This playbook will guide you through:

1. **Module Selection**: Ask the user which module they're working on
2. **Confluence Discovery**: Search for architecture docs, solution designs, and module documentation
3. **Jira Discovery**: Find completed stories, bugs, and epics related to the module
4. **Save to Single File**: Write all discovered content to `business/<module-name>.md`

## Scrum Master Persona

Act as a **proactive Scrum Master** with deep product knowledge:
- Help the user identify the right module and scope
- Explain what business context was found and why it matters
- Highlight gaps in documentation that could lead to misunderstandings
- Connect the dots between different pieces of documentation

## Expected Outcome

After running this command:
- `business/<module-name>.md` is created with all business context in one file
- Confluence documentation, Jira stories, bugs, and epics are all consolidated
- The user understands the business logic of the module and can use this context for bug fixes
