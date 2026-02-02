# Sprint Review Summary Playbook

## Purpose

This playbook guides you through synthesizing "Done" work into a **business value narrative** for stakeholders (executives, product owners, and non-technical audiences).

## Overview

The sprint review summary consists of six main steps:
1. **Load Context**: Read configuration from PROJECT_CONTEXT.md
2. **Fetch Completed Issues**: Get all "Done" issues in the sprint
3. **Group by Epic/Theme**: Organize work into cohesive themes
4. **Extract Business Value**: Identify what was delivered and why it matters
5. **Synthesize Narrative**: Write executive summary with customer/user impact
6. **Format for Stakeholders**: Use clear, non-technical language focused on outcomes

---

## Step 1: Load Context

### Goal
Read scrum-master/PROJECT_CONTEXT.md to get sprint and stakeholder information.

### Instructions

1. **Read PROJECT_CONTEXT.md**
   - Open scrum-master/PROJECT_CONTEXT.md
   - Extract the following:
     - **Board ID**: Used to query sprint issues
     - **Active Sprint ID**: Identifies which sprint to summarize
     - **Sprint Name**: For report header
     - **Sprint Goal**: To assess % of goal achieved
     - **Start/End Dates**: For report metadata
     - **Stakeholders**: List of people who should receive this summary
     - **Custom Field Mappings**: Epic Link, Business Value fields

2. **Verify prerequisites**
   - If PROJECT_CONTEXT.md doesn't exist, instruct user to run `/onboard` first

**Example**:
```
Loaded configuration:
- Sprint: Sprint 23 (Jan 27 - Feb 9)
- Sprint Goal: "Implement new checkout flow and payment gateway integration"
- Stakeholders: Jane Doe (VP Product), John Smith (CTO), Alice Johnson (Customer Success)
- Custom Fields: Epic Link (customfield_10014), Business Value (customfield_10020)
```

---

## Step 2: Fetch Completed Issues

### Goal
Get all issues with status "Done" in the active sprint.

### Instructions

1. **Invoke Jira MCP to get completed issues**
   - Use: `jira.get_sprint_issues(sprint_id=ACTIVE_SPRINT_ID, status="Done", fields=["key", "summary", "description", "epicLink", "businessValue", "storyPoints"])`
   - Request these fields:
     - **key**: Issue key (e.g., PROJ-123)
     - **summary**: Issue title
     - **description**: Full description (may contain business value context)
     - **Epic Link field**: Use custom field ID from PROJECT_CONTEXT.md
     - **Business Value field**: If available, use custom field ID
     - **Story Points field**: For velocity calculation

2. **Handle edge cases**
   - If no completed issues, display: "No work was completed in this sprint. Run this command again after sprint review."
   - If only a few issues, still generate a summary (even 1-2 items can have business value)

**Example output**:
```
Fetched 12 completed issues from Sprint 23:
- 8 stories (21 story points)
- 3 bugs
- 1 spike (research task)
```

---

## Step 3: Group by Epic/Theme

### Goal
Organize work into cohesive themes to tell a clear story.

### Instructions

1. **Use Epic Link field to group issues**
   - For each completed issue, check the Epic Link field
   - Group issues by their epic
   - If Epic Link is available, fetch epic name/summary for context

2. **Handle issues without epics**
   - For issues not linked to an epic, create thematic groupings based on:
     - Type: "Bug Fixes", "Technical Improvements", "New Features"
     - Area: "Payment System", "User Authentication", "Performance"
     - User segment: "Mobile Users", "Enterprise Customers"

3. **Create meaningful group names**
   - Use business-friendly names, not technical labels
   - ✅ Good: "Enhanced Checkout Experience"
   - ❌ Bad: "EPIC-123: Payment Integration"

**Example grouping**:
```
Epics/Themes identified:
1. Enhanced Checkout Experience (5 issues, 13 points)
2. Payment Gateway Integration (3 issues, 5 points)
3. Bug Fixes & Stability (3 issues, 3 points)
4. Performance Improvements (1 issue, 0 points - spike)
```

---

## Step 4: Extract Business Value

### Goal
For each issue, identify what was delivered and why it matters to customers/users.

### Instructions

1. **For each completed issue, extract**:
   - **What was delivered**: Simplified summary (avoid technical details)
   - **Why it matters**: Customer/user impact, business value
   - **Who benefits**: End users, internal teams, stakeholders

2. **Check multiple sources for business value**:
   - **Business Value custom field**: If available, use this first
   - **Issue description**: Often contains "As a user..." or value statements
   - **Acceptance criteria**: May describe user outcomes
   - **Issue summary**: Can sometimes infer value from title

3. **Translate technical work into business outcomes**:
   - ✅ Good: "Reduced checkout time by 30%, improving conversion rates"
   - ❌ Bad: "Implemented Stripe webhook handling with retry logic"
   - ✅ Good: "Fixed critical login bug affecting 15% of mobile users"
   - ❌ Bad: "Updated OAuth2 token refresh logic"

4. **Quantify impact when possible**:
   - Faster: "Reduced load time from 3s to 1s"
   - More reliable: "99.9% uptime achieved"
   - User growth: "Enabled support for 10,000 concurrent users"
   - Revenue: "Unblocked $50K in pending transactions"

**Example extraction**:
```
PROJ-456: "Implement one-click checkout"
- What: Customers can now complete purchases with a single click
- Why: Reduces friction in checkout flow, increases conversion rates
- Who: All returning customers (estimated 60% of users)
- Value: Expected 15-20% increase in completed transactions
```

---

## Step 5: Synthesize Narrative

### Goal
Write an executive summary that tells the story of what was accomplished in the sprint.

### Instructions

1. **Write a 2-3 sentence executive summary**
   - Start with the sprint goal and whether it was achieved
   - Highlight the **most impactful** work completed
   - Mention overall metrics (stories completed, points delivered)

**Example**:
> In Sprint 23, we successfully achieved our goal of implementing the new checkout flow and payment gateway integration. The team delivered 12 stories (21 points) that significantly improve the purchase experience for our customers. Key accomplishments include one-click checkout for returning users and support for 3 new payment methods, expected to increase conversion rates by 15-20%.

2. **Create a bullet list by epic/theme**
   - For each epic or theme:
     - **[Epic Name]**: High-level business value delivered
     - List 2-4 key features/fixes with impact
     - Use bullet points for readability

**Example**:
```markdown
### Enhanced Checkout Experience
Improved the end-to-end purchase flow to reduce friction and increase conversions.

- **One-click checkout**: Returning customers can now complete purchases with a single click, reducing checkout time by 60%
- **Guest checkout**: New customers no longer need to create an account, removing a major barrier to purchase
- **Progress indicator**: Users can see exactly where they are in the checkout process, reducing abandonment by 25%

### Payment Gateway Integration
Expanded payment options to support international customers and increase payment success rates.

- **Stripe integration**: Fully integrated Stripe payment gateway with support for credit cards, Apple Pay, and Google Pay
- **Multi-currency support**: Customers can now pay in their local currency (10 currencies supported)
- **Automatic retry logic**: Failed payments are automatically retried, increasing success rate by 15%
```

3. **Include sprint metrics**
   - **Total stories completed**: [X] stories
   - **Total story points delivered**: [Y] points
   - **% of sprint goal achieved**: Assess whether sprint goal was met (0-100%)
   - **Velocity**: Compare to team's average velocity
   - **Bugs fixed**: Number of bugs resolved

**Example**:
```markdown
## Sprint Metrics

- **Stories Completed**: 12
- **Story Points Delivered**: 21 (slightly above our average of 19)
- **Sprint Goal Achievement**: 100% - All items related to checkout and payment were completed
- **Bugs Fixed**: 3 critical bugs affecting mobile users
- **Velocity Trend**: On track (21 points vs. 19 average)
```

---

## Step 6: Format for Stakeholders

### Goal
Ensure the summary is stakeholder-ready: clear, non-technical, outcome-focused.

### Instructions

1. **Use clear, non-technical language**
   - Avoid jargon: No "API", "webhook", "OAuth", "database migration"
   - Use business terms: "Customer experience", "conversion rate", "revenue impact"

2. **Focus on outcomes, not outputs**
   - ✅ Good: "Reduced checkout time by 60%, expected to increase revenue by 15%"
   - ❌ Bad: "Built 3 new React components for checkout flow"
   - ✅ Good: "Fixed critical bug affecting 15% of mobile users"
   - ❌ Bad: "Updated token refresh logic in authentication service"

3. **Highlight customer/user impact**
   - Who benefits? (e.g., "All mobile users", "Enterprise customers", "New users")
   - What's better for them? (e.g., "Faster checkout", "More payment options", "Fewer errors")

4. **Include link to sprint board**
   - Provide a link to the Jira board for stakeholders who want more details
   - Example: "View full sprint board: https://your-domain.atlassian.net/jira/software/c/projects/PROJ/boards/42"

5. **Keep it concise**
   - Stakeholders are busy - aim for 1-2 pages maximum
   - Use bullet points and headings for scannability
   - Lead with the most impactful information

---

## Report Template

Use this template for the sprint review summary:

```markdown
# Sprint Review Summary - [Sprint Name]

**Sprint**: [Sprint Name, e.g., "Sprint 23"]
**Period**: [Start Date] to [End Date]
**Generated**: [Current Date]
**For**: [Stakeholder names, e.g., "Jane Doe (VP Product), John Smith (CTO)"]

---

## Executive Summary

[2-3 sentence summary]:
In Sprint [X], we [achieved/partially achieved/did not achieve] our goal of [sprint goal]. The team delivered [Y] stories ([Z] points) that [high-level impact]. Key accomplishments include [1-2 most impactful items], which are expected to [business outcome].

---

## What We Delivered

### [Epic/Theme 1 Name]
[1 sentence: High-level business value delivered in this theme]

- **[Feature 1]**: [What it does and why it matters]
- **[Feature 2]**: [What it does and why it matters]
- **[Feature 3]**: [What it does and why it matters]

### [Epic/Theme 2 Name]
[1 sentence: High-level business value delivered in this theme]

- **[Feature 1]**: [What it does and why it matters]
- **[Feature 2]**: [What it does and why it matters]

### [Epic/Theme 3 Name]
[1 sentence: High-level business value delivered in this theme]

- **[Feature 1]**: [What it does and why it matters]
- **[Bug Fix]**: [What was broken and impact of fix]

---

## Sprint Metrics

- **Stories Completed**: [X] stories
- **Story Points Delivered**: [Y] points ([above/below/on par with] our average of [Z])
- **Sprint Goal Achievement**: [%] - [Brief assessment: "Fully achieved", "Partially achieved", etc.]
- **Bugs Fixed**: [N] bugs resolved
- **Velocity Trend**: [On track / Above average / Below average]

---

## Customer & Business Impact

[Highlight the most impactful outcomes]:
- **Conversion Rate**: Expected [X]% increase due to improved checkout flow
- **User Experience**: [Description of UX improvements]
- **Revenue Impact**: [If applicable, quantify potential revenue impact]
- **Customer Satisfaction**: [If applicable, mention reduction in support tickets, user complaints, etc.]
- **Market Readiness**: [If applicable, mention new capabilities that open new markets]

---

## Looking Ahead

**Next Sprint Focus**: [Brief preview of what's coming next]

**Follow-up Items**: [Any blockers, dependencies, or items needing stakeholder attention]
- [Item 1]
- [Item 2]

**Risks & Blockers**: [If any]
- [Risk/Blocker 1 and proposed mitigation]

---

## Additional Information

**Sprint Board**: [Link to Jira board]
**Team**: [Team name]
**Generated by**: Scrum Master AI Agent

---

_Thank you for your continued support!_
```

---

## Persona Notes

### Write for Executives and Product Stakeholders
- Use language that resonates with business leaders
- Focus on **strategic impact**, not implementation details
- Emphasize **ROI** and **customer value**

### Translate Technical Work into Business Outcomes
- Every technical task should map to a business outcome
- If you can't articulate business value, ask: "How does this help our customers/users?"

### Celebrate Wins While Being Honest About Challenges
- Highlight accomplishments prominently
- Be transparent about incomplete work or blockers
- Frame challenges as opportunities: "While we didn't complete X, we learned Y"

### Suggest Follow-up Items or Blockers Needing Escalation
- If there are dependencies on other teams, call them out
- If there are budget or resource constraints, mention them
- Provide clear asks for stakeholder support

---

## Edge Cases & Error Handling

### No Completed Work
- Display: "No work was completed in this sprint. Consider running this command after sprint review, or check if sprint is still active."

### Minimal Work Completed (1-2 issues)
- Still generate a summary - even small accomplishments have value
- Frame positively: "While this was a smaller sprint, we delivered [X] which provides [value]."

### Sprint Goal Not Achieved
- Be honest: "While we didn't fully achieve our sprint goal, we made significant progress on [areas]."
- Explain why: "Due to [unforeseen complexity / scope change / external dependencies], we completed [X]% of the goal."
- Highlight what was learned: "This sprint revealed [insights] that will help us plan more accurately."

### No Business Value Information Available
- Infer from issue summary and description
- Frame in terms of user impact: "This enables users to [action] which improves [outcome]."
- If truly unclear, note: "While the technical implementation is complete, we recommend documenting business value for future work."

---

## Example MCP Tool Invocations

```
# Get all completed issues in sprint
jira.get_sprint_issues(
  sprint_id="128",
  status="Done",
  fields=["key", "summary", "description", "customfield_10014", "customfield_10020", "customfield_10016"]
)

# Alternative: Use JQL to get completed issues
jira.search_issues(
  jql="sprint = 128 AND status = Done",
  fields=["key", "summary", "description", "epic", "storyPoints"]
)

# Get epic details (if epic IDs are found)
jira.get_issue(issue_key="EPIC-42", fields=["summary", "description"])
```

---

## Success Criteria

At the end of this playbook execution:
- ✅ Executive summary (2-3 sentences) is written
- ✅ Work is grouped by epic/theme with business value narrative
- ✅ Sprint metrics are included (stories, points, % of goal)
- ✅ Customer/business impact is highlighted
- ✅ Report uses stakeholder-friendly language (no jargon)
- ✅ Link to sprint board is provided
- ✅ Stakeholders can understand what was delivered and why it matters

---

**Use this playbook to showcase your team's accomplishments and keep stakeholders informed!** 📊
