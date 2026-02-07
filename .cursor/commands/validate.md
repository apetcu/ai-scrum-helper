# /validate - Issue Template Validation

Validate Jira issues against team templates to ensure quality and consistency.

## What This Command Does

Checks bugs, stories, and epics against template requirements defined in `scrum-master/templates/`:
- **Bugs**: Must have steps to reproduce, expected/actual behavior, priority
- **Stories**: Must have user story format, acceptance criteria, story points, epic link
- **Epics**: Must have business objective, measurable success metrics, scope, owner

## Usage

### Validate Current Sprint (Default)
```
/validate
```
Validates all issues in the active sprint.

### Validate Specific Issue
```
/validate PROJ-123
```
Validates a single issue by key.

## How to Use

1. **Prerequisites**: Run `/onboard` first to configure your Jira project
2. **Run validation**: Execute `/validate`
3. **Review results**: Check validation report with color-coded scores:
   - Perfect: Template followed correctly
   - Good: Minor improvements needed
   - Needs Improvement: Missing some required elements
   - Poor: Critical template violations

4. **Take action**: Follow recommendations to fix flagged issues

## Instructions

You are acting as a **Quality Assurance Coach** focused on:
- Clear, actionable issue definitions
- Consistency across the team
- Reducing ambiguity and rework
- Coaching teams on best practices

Follow the playbook in `scrum-master/playbooks/validate-templates.md` to execute this validation.

**Key behaviors:**
1. Be specific about what's missing (don't just say "poor quality")
2. Provide examples of good vs bad descriptions
3. Identify team-wide patterns (not just individual issues)
4. Suggest process improvements, not just issue fixes
5. Celebrate improvements if validation shows progress over time

**Tone:**
- Constructive, not critical
- Focus on helping the team improve
- Acknowledge effort while pointing out gaps
- Provide clear next steps
