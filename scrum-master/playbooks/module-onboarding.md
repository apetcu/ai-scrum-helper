# Module Business Context Onboarding Playbook

## Purpose

This playbook guides you through discovering and saving all relevant business context for a specific module into a single file: `business/<module-name>.md`. This context is used to enhance bug fixes and feature work with deep business understanding.

**Key idea**: When someone reports "drag & drop from invoices not working", having the full business context means you understand the upload flow, accepted file types, validation steps, storage integration, and expected behavior — so you can fix the bug properly, not just superficially.

## Overview

1. **Ask the user** which module they're working on
2. **Search Confluence** for all relevant documentation
3. **Search Jira** for completed stories, bugs, and acceptance criteria
4. **Save everything** into a single `business/<module-name>.md` file

---

## Step 1: Module Selection

### Instructions

1. **Ask the user which module they're working on**
   - Use the AskUserQuestion tool to present module choices
   - If the project has known modules (check `business/` folder or PROJECT_CONTEXT.md), present them as options
   - Always allow free-text input for modules not in the list

2. **Confirm scope with the user**
   - Repeat back the module name
   - Ask if there's a specific area of the module they're focused on (optional, helps narrow search)

**Example**:
> Which module are you working on? I'll gather all the business context, documentation, and completed stories so we have full understanding when working on bugs or features.

---

## Step 2: Confluence Discovery

### Goal
Find all Confluence pages related to the module: architecture docs, solution designs, feature specs, and any other relevant documentation.

### Instructions

1. **Get accessible Atlassian resources**
   - Use `getAccessibleAtlassianResources` to get the cloud ID

2. **Search for module documentation**
   - Use Rovo Search with the module name: `search(query="<module-name>")`
   - Also search with variations:
     - `"<module-name> architecture"`
     - `"<module-name> solution design"`

3. **Search within known Confluence spaces**
   - If a project space is known, search within it using CQL:
     - `searchConfluenceUsingCql(cql='type = page AND text ~ "<module-name>"')`
   - Look for child pages under known parent pages (e.g., under "Modules" or "Architecture")

4. **Retrieve full page content**
   - For each relevant page found, fetch the full content:
     - `getConfluencePage(pageId=PAGE_ID, contentFormat="markdown")`
   - Also check for child pages:
     - `getConfluencePageDescendants(pageId=PAGE_ID)`
   - Retrieve child page content as well

---

## Step 3: Jira Discovery

### Goal
Find all completed Jira stories, bugs, and epics related to the module.

### Instructions

1. **Search for completed stories**
   - Use JQL: `project = "<PROJECT_KEY>" AND type = Story AND status = Done AND text ~ "<module-name>" ORDER BY created DESC`
   - Fetch up to 50 results with fields: summary, description, status, issuetype, priority, created

2. **Search for resolved bugs**
   - Use JQL: `project = "<PROJECT_KEY>" AND type = Bug AND status = Done AND text ~ "<module-name>" ORDER BY created DESC`

3. **Search for epics**
   - Use JQL: `project = "<PROJECT_KEY>" AND type = Epic AND text ~ "<module-name>" ORDER BY created DESC`

4. **Get full issue details**
   - For each issue found, get the full details including description:
     - `getJiraIssue(issueIdOrKey=ISSUE_KEY)`
   - Extract: summary, description (contains acceptance criteria), status, priority

---

## Step 4: Save to Single File

### Goal
Consolidate all discovered content into one file: `business/<module-name>.md`

### Instructions

Write `business/<module-name>.md` with this structure:

```markdown
# <Module Name> - Business Context

> Generated on <date> via /module-onboarding

---

## Overview
<2-3 sentence summary based on discovered documentation>

## Key User Flows
<List the main user interactions based on stories and docs>

## Business Rules & Acceptance Criteria
<Key business rules extracted from stories and documentation>

## Known Edge Cases & Past Bugs
<Summary of resolved bugs — these are likely regression areas>

## Integration Points
<How this module connects to other modules>

---

## Confluence Documentation

### <Page Title 1>
**Source**: <page URL>

<full page content in markdown>

### <Page Title 2>
**Source**: <page URL>

<full page content in markdown>

---

## Jira Stories (Completed)

### PROJ-123: Story title
**Status**: Done | **Priority**: Medium | **Created**: 2026-01-15

<full description with acceptance criteria>

---

### PROJ-124: Another story
...

---

## Jira Bugs (Resolved)

### PROJ-200: Bug title
**Status**: Done | **Priority**: High | **Created**: 2026-01-20

<full description>

---

## Jira Epics

### PROJ-50: Epic title
**Status**: Done | **Priority**: Medium

<full description>
```

### Handle empty sections
- If no Confluence docs found, write: "No Confluence documentation found for this module. Consider documenting it."
- If no Jira issues found for a category, omit that section entirely
- If nothing found at all, note it and suggest the user document the module

---

## Step 5: Confirm to User

Present a summary to the user:

**Example**:
> Module onboarding complete for **Invoices**!
>
> Saved to `business/invoices.md`:
> - **5 Confluence pages** including architecture and SD01 upload flow
> - **10 completed stories** with acceptance criteria
> - **3 resolved bugs** with reproduction steps
>
> This context will be used when working on bugs or features for this module.

---

## How This Context Is Used

Once saved, the business context file enhances all future work:

### Bug Fixes
When a bug is reported (e.g., "drag & drop from invoices not working"):
1. Read `business/invoices.md` for full context
2. Find the expected behavior from the solution design section
3. Check acceptance criteria from completed stories
4. Review past bugs for similar issues
5. Fix the bug with full understanding of the business flow

### Feature Work
When adding new functionality:
1. Understand existing flows from the business context
2. Ensure new features align with documented architecture
3. Follow patterns established in completed stories

---

## Edge Cases & Error Handling

### Module Name Ambiguity
- If search returns results from multiple unrelated areas, ask the user to narrow the scope

### Large Volume of Results
- If more than 20 Confluence pages found, ask the user which are most relevant
- If more than 50 Jira issues found, limit to the most recent

### Refreshing Context
- If `business/<module-name>.md` already exists, ask the user if they want to refresh or keep existing

---

## Success Criteria

At the end of this playbook:
- `business/<module-name>.md` exists with all consolidated context
- Confluence docs, Jira stories, bugs, and epics are all in one file
- The user understands what was collected and how it will be used
