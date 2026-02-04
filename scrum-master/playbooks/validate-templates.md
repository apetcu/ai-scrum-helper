# Issue Template Validation Playbook

## Purpose
Validate Jira issues against team templates to ensure quality and consistency.

## Context Required
- **PROJECT_CONTEXT.md**: Contains Jira configuration (project key, board ID, custom fields)
- **Templates**: Bug, Story, and Epic templates in `scrum-master/templates/`

## Validation Scope

### Option 1: Current Sprint Validation (Default)
Validate all issues in the active sprint.

### Option 2: Specific Issue Validation
Validate a specific issue key (e.g., PROJ-123).

### Option 3: Recent Issues Validation
Validate issues created/updated in the last N days.

## Execution Steps

### Step 1: Load Configuration
1. Read `scrum-master/PROJECT_CONTEXT.md`
2. Extract:
   - Jira project key
   - Active sprint ID (if validating sprint)
   - Story points custom field ID
   - Epic link custom field ID
   - Any other custom field mappings

### Step 2: Load Templates
1. Read template files:
   - `scrum-master/templates/bug-template.md`
   - `scrum-master/templates/story-template.md`
   - `scrum-master/templates/epic-template.md`
2. Parse validation rules from each template

### Step 3: Fetch Issues
Use MCP Jira tools to fetch issues based on scope:

**For Current Sprint:**
```
Use JQL: project = {PROJECT_KEY} AND sprint = {ACTIVE_SPRINT_ID}
```

**For Specific Issue:**
```
Use getJiraIssue with issueIdOrKey
```

**For Recent Issues:**
```
Use JQL: project = {PROJECT_KEY} AND updated >= -7d
```

Fetch these fields:
- `summary`
- `description`
- `issuetype`
- `status`
- `priority`
- `assignee`
- `created`
- `updated`
- Story points custom field
- Epic link custom field
- Labels
- Components

### Step 4: Validate Each Issue

For each issue, run validation based on issue type:

#### Bug Validation
Check against `bug-template.md` rules:

**Critical Failures (❌):**
- Missing or empty description
- Summary > 100 characters
- Description lacks "Steps to Reproduce" section
- Description lacks "Expected Behavior" section
- Description lacks "Actual Behavior" section
- Priority not set
- Vague summary (contains only: "bug", "broken", "doesn't work", "fix")

**Warnings (⚠️):**
- No environment information mentioned
- Bug unassigned for > 24 hours
- No labels applied
- Description < 50 characters (likely incomplete)
- Multiple bugs described (contains "and also", "another issue")

**Scoring:**
- Perfect: 0 failures, 0-1 warnings
- Good: 0 failures, 2-3 warnings
- Needs Improvement: 1-2 failures
- Poor: 3+ failures

#### Story Validation
Check against `story-template.md` rules:

**Critical Failures (❌):**
- Missing or empty description
- Summary > 100 characters
- No user story format ("As a", "I want", "So that")
- Missing "So that" clause (no value statement)
- Fewer than 2 acceptance criteria
- No story points (when in sprint or in progress)
- Story points > 13 (should be split)
- Not linked to an epic (and not labeled "standalone")

**Warnings (⚠️):**
- Acceptance criteria don't use Given/When/Then
- Vague acceptance criteria (< 20 chars each)
- Story added to sprint without points
- In progress > 3 days with no updates
- No assignee (when status = In Progress)
- Technical story without clear user value

**Scoring:**
- Perfect: 0 failures, 0-1 warnings
- Good: 0 failures, 2-3 warnings
- Needs Improvement: 1-2 failures
- Poor: 3+ failures

#### Epic Validation
Check against `epic-template.md` rules:

**Critical Failures (❌):**
- Missing or empty description
- No business objective section
- Fewer than 2 measurable success metrics
- Success metrics are vague (no numbers, no specific KPIs)
- No scope definition (in/out of scope)
- No epic owner assigned
- Missing start or target completion date
- No dependencies section (should say "None" if truly none)

**Warnings (⚠️):**
- Epic spans > 2 quarters (> 6 months)
- No linked stories after 2 weeks of epic creation
- Vague success metrics (no specific targets)
- No user personas identified
- No risk assessment
- Stale epic: no activity in 30+ days with status = "In Progress"
- Scope creep: >30% more child stories than originally planned

**Scoring:**
- Perfect: 0 failures, 0-2 warnings
- Good: 0 failures, 3-4 warnings
- Needs Improvement: 1-2 failures
- Poor: 3+ failures

### Step 5: Generate Report

Create a validation report with the following structure:

```markdown
# Issue Template Validation Report
**Generated**: {timestamp}
**Scope**: {sprint name / issue key / date range}

## Summary
- Total Issues Validated: X
- Perfect: X (score badge: 🟢)
- Good: X (score badge: 🟡)
- Needs Improvement: X (score badge: 🟠)
- Poor: X (score badge: 🔴)

## Issues by Type
### Bugs: X issues
- Perfect: X | Good: X | Needs Improvement: X | Poor: X

### Stories: X issues
- Perfect: X | Good: X | Needs Improvement: X | Poor: X

### Epics: X issues
- Perfect: X | Good: X | Needs Improvement: X | Poor: X

## Detailed Validation Results

### 🔴 Poor Issues (Immediate Attention Required)
For each poor issue:
**[PROJ-123](link) - {Summary}** | Type: Bug | Status: In Progress
- ❌ Missing "Steps to Reproduce" section
- ❌ Priority not set
- ⚠️ No assignee

**Recommended Actions:**
- Add reproduction steps to description
- Set priority based on severity
- Assign to team member or move to backlog

---

### 🟠 Issues Needing Improvement
[Similar format as above]

---

### 🟡 Good Issues (Minor Improvements)
[List with brief warnings only]

---

### 🟢 Perfect Issues
[Simple list with links]

## Common Patterns & Trends
Identify recurring issues:
- X bugs missing reproduction steps
- X stories lacking acceptance criteria
- X issues with vague summaries
- X items unpointed in active sprint

## Recommendations
Provide actionable team-level recommendations:
1. **For Team**: Consider a template reminder checklist in Jira
2. **For Product Owner**: Review unlinked stories and assign to epics
3. **For Scrum Master**: Facilitate refinement session for 8 poorly-defined stories
4. **Documentation**: Update team wiki with template examples

## Next Steps
- [ ] Review flagged issues in next daily standup
- [ ] Refinement session for issues needing improvement
- [ ] Update team wiki with validation results
- [ ] Re-run validation in 1 week to measure improvement
```

## Output Format

### Interactive Mode (Default)
Display the full report in a readable markdown format with:
- Color-coded badges (🟢🟡🟠🔴)
- Clickable Jira issue links
- Collapsible sections for perfect issues
- Actionable recommendations

### Summary Mode
If user specifies `--summary`, provide only:
- Total counts
- Poor and needs-improvement issues
- Recommendations

### Export Mode
If user specifies `--export`, also:
- Save report to `scrum-master/reports/validation-{date}.md`
- Generate CSV of validation scores for tracking trends

## Edge Cases

### No Issues Found
```
No issues found in scope. Either:
- Sprint is empty
- Issue key doesn't exist
- Date range has no activity
```

### Missing Custom Fields
If story points or epic link fields can't be found:
- Log warning about missing field mapping
- Skip validation rules that depend on those fields
- Suggest running `/onboard` to refresh configuration

### API Errors
If Jira MCP calls fail:
- Report the specific error
- Suggest checking MCP server connection
- Provide fallback: validate a single issue by key instead

## Proactive Recommendations

After validation, proactively suggest:

1. **If many bugs lack reproduction steps:**
   "Consider adding a bug report template checklist in Jira or GitHub issue forms"

2. **If stories frequently lack acceptance criteria:**
   "Schedule a refinement workshop focused on writing testable ACs"

3. **If epics have vague success metrics:**
   "Work with product leadership to define clear KPIs for all active epics"

4. **If validation shows improvement over time:**
   "Great progress! Validation scores improved by X% since last check 🎉"

## Integration with Other Playbooks

### Called from `/health` Check
When validation is run as part of sprint health:
- Use summary mode
- Only flag poor/needs-improvement issues
- Integrate into overall health score

### Standalone `/validate` Command
When run independently:
- Use full interactive mode
- Provide detailed analysis
- Export report to reports directory

## Success Criteria
This playbook succeeds when:
- All in-scope issues are evaluated
- Validation rules are correctly applied based on issue type
- Report is clear, actionable, and stakeholder-friendly
- Team can immediately act on recommendations
