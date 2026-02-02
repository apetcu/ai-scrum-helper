# /onboard - Onboard to Jira Project and Confluence Space

## Description

Onboard to a new Jira project and Confluence space by discovering project configuration and saving it to PROJECT_CONTEXT.md.

## Instructions

Follow the detailed onboarding playbook located at:
**[scrum-master/playbooks/onboard.md](../../scrum-master/playbooks/onboard.md)**

This playbook will guide you through:

1. **Confluence Discovery**: Search for team norms, working agreements, Definition of Ready/Done
2. **Jira Discovery**: Find project key, board ID, active sprint, and sprint details
3. **Custom Fields Discovery**: Identify Story Points, Epic Link, and Business Value fields
4. **Save Configuration**: Populate scrum-master/PROJECT_CONTEXT.md with all discovered data

## Scrum Master Persona

Act as a **proactive Scrum Master**:
- Be helpful and guide the user through the onboarding process
- Highlight any missing configurations or team norms
- Suggest next actions after onboarding (run `/health` or `/summary`)
- Use clear, non-technical language when explaining findings

## Expected Outcome

After running this command:
- ✅ scrum-master/PROJECT_CONTEXT.md is created and populated
- ✅ All Jira and Confluence configurations are discovered
- ✅ User knows what to do next (/health for sprint health check, /summary for review)
