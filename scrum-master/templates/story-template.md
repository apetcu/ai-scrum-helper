# Story Template

## Required in Description

**User Story:**
As a [user type], I want [action], so that [value/benefit]

**Acceptance Criteria:** (minimum 2)
- AC1: [Testable condition]
- AC2: [Testable condition]

## Required Fields
- Story points set before sprint (≤13)
- Linked to epic (or labeled "standalone")
- Must include "So that" clause

## Validation Rules
✅ PASS: Has "As a/I want/So that", 2+ ACs, points ≤13, epic link
⚠️ WARN: No Given/When/Then, added to sprint without points, stale >3 days
❌ FAIL: Missing user story format, no "So that", <2 ACs, points >13, no epic link
