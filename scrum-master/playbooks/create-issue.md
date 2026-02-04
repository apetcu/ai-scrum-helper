# Create Issue Playbook

## Purpose
Guide users through creating template-compliant Jira issues (bugs, stories, epics).

## When to Use
- User asks to "create a story/bug/epic"
- User says "write a Jira ticket for..."
- User requests "add an issue for..."
- Any request to create new Jira issues

## Prerequisites
1. Read `scrum-master/PROJECT_CONTEXT.md` for Jira configuration
2. Load appropriate template from `scrum-master/templates/`
3. Understand the issue type being requested

---

## Execution Steps

### Step 1: Determine Issue Type
Ask if not clear: "What type of issue should I create?"
- **Bug**: Something is broken and needs fixing
- **Story**: New feature or enhancement
- **Epic**: Large initiative spanning multiple stories

### Step 2: Load Template
Read the appropriate template:
- Bug: `scrum-master/templates/bug-template.md`
- Story: `scrum-master/templates/story-template.md`
- Epic: `scrum-master/templates/epic-template.md`

### Step 3: Gather Information

#### For Bugs:
Ask the user:
1. **Summary**: Brief description (< 100 chars)
2. **Steps to Reproduce**: How to trigger the bug?
3. **Expected Behavior**: What should happen?
4. **Actual Behavior**: What actually happens?
5. **Environment**: Browser/OS/Version?
6. **Priority**: Critical/High/Medium/Low?

**Use interactive questions** if information is missing.

#### For Stories:
Ask the user:
1. **Summary**: Brief description (< 100 chars)
2. **User Story**:
   - Who is the user? (As a...)
   - What do they want? (I want...)
   - Why? (So that...)
3. **Acceptance Criteria**: At least 2 testable conditions
4. **Story Points**: Estimate (1,2,3,5,8,13)
5. **Epic Link**: Which epic does this belong to?

**Use interactive questions** if information is missing.

#### For Epics:
Ask the user:
1. **Summary**: Brief description (< 100 chars)
2. **Business Objective**: What problem are we solving?
3. **Success Metrics**: At least 2 with numbers
   - e.g., "Reduce X from A to B"
4. **Scope**: What's in and out?
5. **Dependencies**: What's needed? (or "None")
6. **Owner**: Who owns this epic?
7. **Dates**: Start and target completion

**Use interactive questions** if information is missing.

### Step 4: Build Description
Format the description following the template:

**Bug Format:**
```
**Steps to Reproduce:**
1. [Step 1]
2. [Step 2]

**Expected:** [Expected behavior]
**Actual:** [Actual behavior]
**Environment:** [Browser/OS/Version]
```

**Story Format:**
```
**User Story:**
As a [user type], I want [action], so that [value]

**Acceptance Criteria:**
- AC1: [Testable condition]
- AC2: [Testable condition]
```

**Epic Format:**
```
**Business Objective:** [Problem statement]

**Success Metrics:**
- [Metric 1 with numbers]
- [Metric 2 with numbers]

**Scope:**
- In: [Features included]
- Out: [Features excluded]

**Dependencies:** [List or "None"]
```

### Step 5: Validate Against Template
Before creating, check:
- All required sections present?
- Format matches template?
- No template violations?

If violations exist, prompt user to fix them.

### Step 6: Create Issue in Jira
Use Jira MCP tool:
```
createJiraIssue({
  cloudId: [from PROJECT_CONTEXT],
  projectKey: [from PROJECT_CONTEXT],
  issueTypeName: "Bug" | "Story" | "Epic",
  summary: [summary],
  description: [formatted description],
  additional_fields: {
    priority: [for bugs],
    storyPoints: [for stories],
    epicLink: [for stories],
    assignee_account_id: [if specified]
  }
})
```

### Step 7: Confirm Creation
Return to user:
- ✅ Issue created: [PROJ-123](link)
- Show summary and key details
- Mention: "This issue follows the team template"

---

## Interactive Approach

When information is missing, use AskUserQuestion to gather it:

**Example for Story:**
```
I need a few details to create a template-compliant story:

1. **Who is the user?** (As a...)
   - Options: End user, Admin, Developer, Other

2. **What should happen?** (I want...)
   - [User provides description]

3. **Why is this valuable?** (So that...)
   - [User provides benefit]

4. **Acceptance Criteria?** (At least 2)
   - [User provides criteria]
```

---

## Key Behaviors

### Be Template-Aware
- Always reference templates before creating issues
- Guide users to provide template-required information
- Never create issues that violate templates

### Be Conversational
- Don't dump all questions at once
- Ask follow-ups naturally
- Offer to draft and let user refine

### Validate Early
- Check template compliance before creating
- Point out what's missing
- Suggest improvements

### Educate Users
- Explain why template fields matter
- "Acceptance criteria help developers know when it's done"
- "Reproduction steps let us fix the bug faster"

---

## Example Flow

**User:** "Create a story for adding a logout button"

**AI:**
> I'll help you create a template-compliant story. Let me gather the details:

**Question 1:** Who will use this logout button?
- [ ] End users
- [ ] Admins
- [ ] Both

**User:** End users

**AI:**
> Great! Now, what's the main benefit of having a logout button?

**User:** So users can securely end their session

**AI:**
> Perfect! What are the acceptance criteria? For example:
> - AC1: When clicked, user is logged out and redirected to login page
> - AC2: Session is cleared on the server

**User:** Yes, and also show a confirmation message

**AI:**
> Excellent! How many story points?

**User:** 3

**AI:**
> Which epic should this belong to?

**User:** User Authentication

**AI:**
> Creating story...
>
> ✅ Created: [PROJ-456](link) - Add logout button for users
>
> **Summary:**
> - User Story: As an end user, I want to log out, so that I can securely end my session
> - Acceptance Criteria: 3 defined
> - Story Points: 3
> - Epic: User Authentication
>
> This issue follows the team template ✓

---

## Edge Cases

### Minimal Information Provided
User: "Create a bug for broken login"

**Response:**
> I need more details to create a template-compliant bug:
> 1. What steps reproduce the issue?
> 2. What should happen vs what actually happens?
> 3. What environment? (browser, OS, version)

### User Provides Full Details
User: "Create a story: As a user, I want to filter by date, so that I can see recent orders. AC1: Date picker shown. AC2: Results filtered. 5 points. Epic: Dashboard."

**Response:**
> Creating template-compliant story...
> ✅ Done! [PROJ-789](link)

### Template Violation Detected
If user's description doesn't follow template:

**Response:**
> ⚠️ This issue is missing required template fields:
> - Stories need "So that" clause (value statement)
> - At least 2 acceptance criteria required
>
> Should I help you add these?

---

## Success Criteria
- ✅ All created issues follow templates
- ✅ No template validation failures on new issues
- ✅ Users understand why template fields matter
- ✅ Issues are created efficiently without tedious back-and-forth
