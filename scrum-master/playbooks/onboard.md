# Onboarding Playbook

## Purpose

This playbook guides you through the automated onboarding process to discover and save Jira and Confluence configuration to **scrum-master/PROJECT_CONTEXT.md**.

## Overview

The onboarding process consists of four main steps:
1. **Confluence Discovery**: Find team norms and working agreements
2. **Jira Discovery**: Identify project, board, and active sprint
3. **Custom Fields Discovery**: Map important custom fields (story points, epic link, etc.)
4. **Save Configuration**: Populate PROJECT_CONTEXT.md with all discovered data

---

## Step 1: Welcome & Context

Start by welcoming the user and explaining what onboarding does:

**Example message**:
> Welcome to the Scrum Master AI Agent! 🎯
>
> I'll help you onboard to your Jira project and Confluence space. This process will:
> - Discover your Jira project, board, and active sprint
> - Find team norms and working agreements in Confluence
> - Map custom fields (story points, epic link, business value)
> - Save all configuration to scrum-master/PROJECT_CONTEXT.md
>
> This will take just a few minutes. Let's get started!

---

## Step 2: Confluence Discovery

### Goal
Find team norms, working agreements, Definition of Ready (DoR), and Definition of Done (DoD) from Confluence.

### Instructions

1. **Check if Confluence MCP is available**
   - If not available, skip this step and note it in PROJECT_CONTEXT.md

2. **List available Confluence spaces**
   - Invoke the Confluence MCP tool: `confluence.list_spaces()`
   - Present the list to the user and ask which space contains team documentation

3. **Search for team norms**
   - Once you have the space key, search for relevant pages
   - Invoke: `confluence.search_pages(space_key=USER_PROVIDED_SPACE, query="team norms")`
   - Also search for:
     - "Definition of Done"
     - "Definition of Ready"
     - "Working Agreement"
     - "Sprint cadence" or "ceremony schedule"

4. **Retrieve page content**
   - For each relevant page found, get the full content
   - Invoke: `confluence.get_page(page_id=PAGE_ID)`
   - Extract key information:
     - **Definition of Ready**: Checklist of criteria for a story to be ready for sprint
     - **Definition of Done**: Checklist of criteria for a story to be considered complete
     - **Working Agreement**: Team agreements and expectations
     - **Ceremony Schedule**: When sprints start/end, stand-up time, etc.

5. **Handle missing data**
   - If no team norms are found in Confluence, suggest the team document them
   - Note in PROJECT_CONTEXT.md: "Team norms not found in Confluence - consider documenting them"

---

## Step 3: Jira Discovery

### Goal
Identify the Jira project key, board ID, and active sprint ID.

### Instructions

1. **Ask for project key or list projects**
   - Option A: Ask the user: "What is your Jira project key? (e.g., PROJ, ECOM, etc.)"
   - Option B: List available projects using: `jira.get_project(project_key=?)`
     - If the user doesn't know the project key, you can use Jira search or ask them to check their Jira board URL

2. **Get project details**
   - Once you have the project key, get project info
   - Invoke: `jira.get_project(project_key=USER_PROVIDED_KEY)`
   - Extract: project name, description

3. **Find the board**
   - Get all boards for the project
   - Invoke: `jira.get_boards(project_key=USER_PROVIDED_KEY)`
   - If multiple boards exist, ask the user which one they use
   - Extract: **board ID**

4. **Find the active sprint**
   - Get all sprints for the board
   - Invoke: `jira.get_sprints(board_id=DISCOVERED_BOARD_ID)`
   - Identify the active sprint (status = "active")
   - Extract:
     - **Sprint ID**
     - **Sprint name** (e.g., "Sprint 42")
     - **Sprint goal** (if available)
     - **Start date**
     - **End date**

5. **Handle no active sprint**
   - If no active sprint is found, note this in PROJECT_CONTEXT.md
   - Suggest: "Run `/onboard` again when a sprint starts to update configuration"

---

## Step 4: Custom Fields Discovery

### Goal
Map Jira custom field IDs to semantic names (story points, epic link, business value, etc.).

### Instructions

1. **List all fields**
   - Invoke: `jira.get_fields()`
   - This returns all standard and custom fields with their IDs and names

2. **Identify key custom fields**
   - Search for these common fields by name:
     - **Story Points**: Look for field names like "Story Points", "Estimate", "Points"
     - **Epic Link**: Look for "Epic Link" or "Parent Epic"
     - **Business Value**: Look for "Business Value", "Value", "Impact"
     - **Sprint**: Look for "Sprint" field (often used for sprint assignment)

3. **Map field IDs**
   - For each field found, record its custom field ID (e.g., `customfield_10016`)
   - If a field is not found, mark it as "N/A" in PROJECT_CONTEXT.md

4. **Ask about additional fields**
   - Ask the user: "Are there any other custom fields you'd like me to track?" (e.g., "Priority Score", "Customer Segment", etc.)
   - Add any user-specified fields to the mapping

**Example output**:
```
Custom Field Mappings:
- Story Points: customfield_10016
- Epic Link: customfield_10014
- Business Value: N/A (field not found)
- Sprint: customfield_10010
```

---

## Step 5: Save to PROJECT_CONTEXT.md

### Goal
Populate scrum-master/PROJECT_CONTEXT.md with all discovered data.

### Instructions

1. **Use the template from scrum-master/project.md**
   - Read the template structure
   - Fill in all sections with discovered data

2. **Populate the following sections**:
   - **Project Metadata**: Project name, organization (ask user if not available), team name
   - **Jira Configuration**: Project key, board ID, active sprint ID, sprint name, sprint goal, start/end dates
   - **Team Information**: Team name, stakeholders (ask user to provide names)
   - **Custom Field Mappings**: All discovered custom field IDs
   - **Team Norms**: Definition of Ready, Definition of Done, Working Agreement, Ceremony Schedule (from Confluence)
   - **Configuration Notes**: Confluence space, onboarding date

3. **Write the file**
   - Create scrum-master/PROJECT_CONTEXT.md with all populated data
   - Use clear markdown formatting
   - Include the current date as "Last Updated"

4. **Confirm successful onboarding**
   - Display a success message with summary:

**Example message**:
> ✅ **Onboarding complete!**
>
> I've discovered and saved your configuration to scrum-master/PROJECT_CONTEXT.md:
> - **Project**: ECOM (E-Commerce Platform)
> - **Board**: Board 42
> - **Active Sprint**: Sprint 23 (Jan 27 - Feb 9)
> - **Custom Fields**: Story Points, Epic Link mapped
> - **Team Norms**: Loaded from Confluence space "TEAM"
>
> **Next steps**:
> - Run `/health` to check your current sprint health
> - Run `/summary` to generate a sprint review summary
>
> You can re-run `/onboard` anytime to refresh your configuration!

---

## Step 6: Persona Notes

### Be Proactive
- Suggest next actions after onboarding (run `/health` or `/summary`)
- Highlight any missing configurations:
  - "No team norms found in Confluence - consider documenting them for better sprint alignment"
  - "No Business Value field detected - consider adding one to track impact"

### Be Helpful
- If the user doesn't know their project key, guide them:
  - "You can find your project key in the Jira board URL: https://your-domain.atlassian.net/jira/software/projects/PROJ/boards/42"
- If multiple boards exist, ask which one they use most often

### Be Clear
- Use non-technical language when possible
- Explain what each step is doing: "I'm searching Confluence for your team's working agreements..."
- Provide progress updates: "Found 3 relevant Confluence pages, reading them now..."

---

## Edge Cases & Error Handling

### No Confluence Access
- Note in PROJECT_CONTEXT.md: "Confluence not available"
- Suggest manually adding team norms to PROJECT_CONTEXT.md if needed

### No Active Sprint
- Note in PROJECT_CONTEXT.md: "No active sprint found"
- Suggest: "Run `/onboard` again when your sprint starts"

### Multiple Boards
- Present all boards to the user and ask which one they use
- Save the selected board ID

### Custom Field Not Found
- Mark as "N/A" in PROJECT_CONTEXT.md
- Note: "This field can be added manually if needed"

### User Doesn't Know Project Key
- Guide them to find it in their Jira URL
- Or use Jira search to help them discover it

---

## Example MCP Tool Invocations

Here are concrete examples of how to invoke MCP tools (these may vary slightly based on the actual MCP server implementation):

```
# List Confluence spaces
confluence.list_spaces()

# Search for team norms in a space
confluence.search_pages(space_key="TEAM", query="team norms")

# Get a specific Confluence page
confluence.get_page(page_id="12345678")

# Get Jira project details
jira.get_project(project_key="ECOM")

# List boards for a project
jira.get_boards(project_key="ECOM")

# Get sprints for a board
jira.get_sprints(board_id="42")

# List all Jira fields (standard + custom)
jira.get_fields()
```

---

## Success Criteria

At the end of this playbook execution:
- ✅ scrum-master/PROJECT_CONTEXT.md exists and is populated
- ✅ Jira project key, board ID, and active sprint ID are saved
- ✅ Custom field mappings are discovered and recorded
- ✅ Team norms are fetched from Confluence (or noted as unavailable)
- ✅ User knows what to do next (/health or /summary)

---

**You're now ready to run `/health` or `/summary`!** 🚀
