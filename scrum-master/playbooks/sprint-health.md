# Sprint Health Check Playbook

## Purpose

This playbook guides you through analyzing the active sprint to identify **bottlenecks**, **stale tickets**, **scope creep**, and **unpointed work** that may impact sprint success.

## Overview

The sprint health check consists of six main steps:
1. **Load Context**: Read configuration from PROJECT_CONTEXT.md
2. **Fetch Active Sprint Issues**: Get all issues in the current sprint
3. **Analyze Stale Tickets**: Identify issues with no movement in 3+ days
4. **Detect Scope Creep**: Find items added after sprint started
5. **Flag Unpointed Work**: Identify issues without story point estimates
6. **Generate Report**: Create actionable markdown report with recommendations

---

## Step 1: Load Context

### Goal
Read scrum-master/PROJECT_CONTEXT.md to get Jira configuration.

### Instructions

1. **Read PROJECT_CONTEXT.md**
   - Open scrum-master/PROJECT_CONTEXT.md
   - Extract the following:
     - **Board ID**: Used to query sprint issues
     - **Active Sprint ID**: Identifies which sprint to analyze
     - **Sprint Start Date**: Used to detect scope creep
     - **Custom Field Mappings**: To access story points field

2. **Verify prerequisites**
   - If PROJECT_CONTEXT.md doesn't exist, instruct user to run `/onboard` first
   - If no active sprint is configured, note: "No active sprint found. Run `/onboard` to refresh configuration."

**Example**:
```
Loaded configuration:
- Board ID: 42
- Active Sprint ID: 128
- Sprint: Sprint 23 (Jan 27 - Feb 9)
- Story Points Field: customfield_10016
```

---

## Step 2: Fetch Active Sprint Issues

### Goal
Get all issues in the active sprint with relevant fields.

### Instructions

1. **Invoke Jira MCP to get sprint issues**
   - Use: `jira.get_sprint_issues(sprint_id=ACTIVE_SPRINT_ID, fields=["key", "summary", "status", "assignee", "updated", "created", "storyPoints"])`
   - Request these fields:
     - **key**: Issue key (e.g., PROJ-123)
     - **summary**: Issue title
     - **status**: Current status (To Do, In Progress, Done, etc.)
     - **assignee**: Who owns the issue
     - **updated**: Last updated timestamp
     - **created**: When the issue was created
     - **Story Points field**: Use the custom field ID from PROJECT_CONTEXT.md (e.g., customfield_10016)

2. **Store all issues**
   - Keep the full list for analysis in subsequent steps
   - Count total issues in sprint

**Example output**:
```
Fetched 23 issues from Sprint 23:
- 8 in "To Do"
- 10 in "In Progress"
- 5 in "Done"
```

---

## Step 3: Analyze Stale Tickets

### Goal
Identify issues with no movement in the last 3 days (configurable threshold).

### Instructions

1. **Calculate staleness threshold**
   - Current date minus 3 days = threshold date
   - Any issue with `updated` date older than this threshold is "stale"

2. **Filter stale issues**
   - For each issue in the sprint:
     - If `updated` date > 3 days ago AND status is NOT "Done"
     - Add to stale tickets list
   - Exclude issues in "Done" status (they're complete, so no movement is expected)

3. **Group by assignee**
   - Organize stale tickets by assignee to make it easier to identify who to follow up with
   - If unassigned, group separately

4. **Identify potential blockers**
   - Issues in "In Progress" that are stale are likely blocked
   - Issues in "To Do" that are stale may need to be prioritized or removed from sprint

**Example analysis**:
```
Stale Tickets (No movement in 3+ days):
- Alice (2 tickets):
  - PROJ-123: "Implement payment gateway" (In Progress, 6 days stale)
  - PROJ-456: "Add error handling" (In Progress, 4 days stale)
- Bob (1 ticket):
  - PROJ-789: "Update API documentation" (In Review, 4 days stale)
- Unassigned (1 ticket):
  - PROJ-101: "Refactor checkout flow" (To Do, 8 days stale)
```

---

## Step 4: Detect Scope Creep

### Goal
Find issues that were added to the sprint **after** it started.

### Instructions

1. **Get sprint start date**
   - Read from PROJECT_CONTEXT.md (e.g., "2026-01-27")

2. **Compare issue created date with sprint start date**
   - For each issue in the sprint:
     - If `created` date > sprint start date
     - Add to scope creep list

3. **Calculate scope creep percentage**
   - Formula: (Number of issues added after start / Total issues in sprint) × 100
   - Example: 5 issues added after start / 23 total issues = 21.7% scope creep

4. **Categorize severity**
   - **Low** (<10% of sprint): Acceptable, may be critical bug fixes
   - **Medium** (10-25%): Concerning, may affect sprint goal
   - **High** (>25%): Risky, significant deviation from plan

**Example analysis**:
```
Scope Creep (Added after sprint start):
- PROJ-234: "Fix critical login bug" (added Jan 29, +2 days into sprint)
- PROJ-567: "Update marketing banner" (added Jan 30, +3 days into sprint)
- PROJ-890: "Add analytics tracking" (added Feb 1, +5 days into sprint)

Total: 3 issues added after start (13% of sprint) - Medium risk
```

---

## Step 5: Flag Unpointed Work

### Goal
Identify issues without story point estimates.

### Instructions

1. **Get story points field**
   - Use the custom field mapping from PROJECT_CONTEXT.md (e.g., customfield_10016)

2. **Check each issue**
   - For each issue in the sprint:
     - If story points field is null, empty, or 0
     - Add to unpointed work list

3. **Assess impact on velocity**
   - Unpointed work makes velocity tracking unreliable
   - If many issues are unpointed, the team may not know if they're on track

**Example analysis**:
```
Unpointed Work (No story points):
- PROJ-345: "Write user guide" (In Progress, assigned to Carol)
- PROJ-678: "Set up monitoring" (To Do, assigned to Dave)
- PROJ-901: "Update dependencies" (To Do, unassigned)

Total: 3 unpointed issues (13% of sprint) - Risk to velocity tracking
```

---

## Step 6: Generate Report

### Goal
Create a markdown report with clear sections and actionable recommendations.

### Instructions

1. **Structure the report with these sections**:
   - **Sprint Overview**: High-level summary
   - **Stale Tickets**: Issues with no movement in 3+ days
   - **Scope Creep**: Items added after sprint started
   - **Unpointed Work**: Issues missing estimates
   - **Recommendations**: Actionable next steps

2. **Use markdown tables for clarity**
   - For stale tickets, use a table with columns: Key, Summary, Assignee, Status, Last Updated
   - For scope creep, use a table with: Key, Summary, Created Date, Days After Start
   - For unpointed work, use a table with: Key, Summary, Assignee, Status

3. **Provide actionable recommendations**
   - For each issue category, suggest specific actions:
     - **Stale tickets**: "Schedule a sync with [Assignee] to identify blockers"
     - **Scope creep**: "Discuss with Product Owner whether to deprioritize or remove items"
     - **Unpointed work**: "Hold a quick estimation session to point remaining work"

---

## Report Template

Use this template for the sprint health report:

```markdown
# Sprint Health Check - [Sprint Name]

**Sprint**: [Sprint Name, e.g., "Sprint 23"]
**Period**: [Start Date] to [End Date]
**Generated**: [Current Date]

---

## Sprint Overview

- **Total Issues**: [X] issues
- **Completed**: [Y] issues ([Z]%)
- **In Progress**: [A] issues
- **To Do**: [B] issues

---

## 🚨 Stale Tickets (No movement in 3+ days)

[If no stale tickets found]:
> ✅ No stale tickets found! All work is moving forward.

[If stale tickets found]:
> Found **[N]** stale tickets that haven't moved in 3+ days:

| Key | Summary | Assignee | Status | Last Updated | Days Stale |
|-----|---------|----------|--------|--------------|------------|
| PROJ-123 | Implement payment gateway | Alice | In Progress | Jan 26 | 6 days |
| PROJ-456 | Add error handling | Alice | In Progress | Jan 28 | 4 days |
| PROJ-789 | Update API documentation | Bob | In Review | Jan 28 | 4 days |

**Analysis**:
- Alice has 2 stale tickets in progress - possible blocker?
- PROJ-789 has been in review for 4 days - needs reviewer assignment

---

## 📈 Scope Creep (Added after sprint start)

[If no scope creep]:
> ✅ No scope creep detected. Sprint scope is stable.

[If scope creep detected]:
> **[N]** issues were added after the sprint started ([X]% of total sprint):

| Key | Summary | Created | Days After Start |
|-----|---------|---------|------------------|
| PROJ-234 | Fix critical login bug | Jan 29 | +2 days |
| PROJ-567 | Update marketing banner | Jan 30 | +3 days |

**Analysis**:
- 13% scope creep (Medium risk)
- PROJ-234 is a critical bug fix (justified)
- PROJ-567 may be a distraction from sprint goal

---

## ⚠️ Unpointed Work (No story points)

[If all work is pointed]:
> ✅ All work is estimated! Velocity tracking is reliable.

[If unpointed work exists]:
> **[N]** issues are missing story point estimates:

| Key | Summary | Assignee | Status |
|-----|---------|----------|--------|
| PROJ-345 | Write user guide | Carol | In Progress |
| PROJ-678 | Set up monitoring | Dave | To Do |

**Analysis**:
- 13% of sprint is unpointed - velocity tracking may be unreliable
- Team may not know if they're on track to meet sprint goal

---

## 💡 Recommendations

### Immediate Actions (Next 24 hours)
1. **Unblock stale work**: Schedule a 15-min sync with Alice to identify blockers on PROJ-123 and PROJ-456
2. **Assign reviewers**: PROJ-789 needs a reviewer assigned - reach out to the team
3. **Point unestimated work**: Quick estimation session for PROJ-345 and PROJ-678 (10 minutes)

### Short-term Actions (This week)
1. **Review scope creep**: Discuss with Product Owner whether PROJ-567 should stay in sprint
2. **Daily stand-up focus**: Ask team members about tickets in progress >3 days
3. **Consider removing items**: If sprint is overcommitted, discuss moving PROJ-101 back to backlog

### Sprint Process Improvements
1. **Lock scope after planning**: Minimize mid-sprint additions unless critical
2. **Estimate all work**: Make estimation part of refinement/planning
3. **Update tickets daily**: Encourage team to update status and add comments

---

## Next Steps

- Re-run `/health` tomorrow to track progress on recommendations
- Run `/summary` at sprint end to generate stakeholder-ready review

---

_Generated by Scrum Master AI Agent_
```

---

## Persona Notes

### Focus on Flow and Bottlenecks
- Don't just list issues - explain **why** they're problematic
- Highlight **risks to sprint success**
- Prioritize issues that are blocking other work

### Use Business Language
- Avoid jargon: "This ticket hasn't moved" (not "low velocity")
- Be clear about impact: "May miss sprint goal" (not "reduced throughput")

### Provide Actionable Recommendations
- Every insight should lead to a specific action
- Recommendations should say **who** should do **what** and **when**
- Prioritize: "Immediate" vs. "Short-term" vs. "Process Improvements"

### Suggest Conversations
- "Schedule a sync with Alice to identify blockers"
- "Ask Bob if he needs help getting PROJ-789 reviewed"
- "Discuss with Product Owner whether to deprioritize PROJ-567"

---

## Edge Cases & Error Handling

### No Active Sprint
- Display: "No active sprint found. Run `/onboard` to configure."

### All Issues Are Healthy
- Celebrate! Display: "✅ Sprint health looks great! No stale tickets, scope is stable, and all work is estimated."

### No Story Points Field
- Skip unpointed work section
- Note: "Story Points field not configured - unable to track velocity"

### No Issues in Sprint
- Display: "No issues found in sprint. Has sprint planning happened yet?"

---

## Example MCP Tool Invocations

```
# Get all issues in active sprint
jira.get_sprint_issues(
  sprint_id="128",
  fields=["key", "summary", "status", "assignee", "updated", "created", "customfield_10016"]
)

# Alternative: Use JQL to search for sprint issues
jira.search_issues(
  jql="sprint = 128 AND status != Done",
  fields=["key", "summary", "status", "assignee", "updated", "created"]
)
```

---

## Success Criteria

At the end of this playbook execution:
- ✅ Sprint health report is generated with all sections
- ✅ Stale tickets are identified and grouped by assignee
- ✅ Scope creep is detected and categorized by severity
- ✅ Unpointed work is flagged
- ✅ Actionable recommendations are provided
- ✅ User knows what actions to take next

---

**Use this playbook to catch issues early and keep your sprint on track!** 🎯
