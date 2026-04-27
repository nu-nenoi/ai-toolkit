<!-- Template: JIRA Ticket -->
<!-- Instructions: Replace all {{PLACEHOLDER}} values. Choose the right ticket type and remove irrelevant sections. -->

# [{{TYPE}}] {{Ticket Title}}

| Field | Value |
|-------|-------|
| **Type** | {{Story / Bug / Task / Epic / Spike}} |
| **Priority** | {{🔴 Critical / 🟠 High / 🟡 Medium / 🟢 Low}} |
| **Labels** | {{`label-1`, `label-2`}} |
| **Epic** | [{{EPIC-ID}}]({{epic-url}}) |
| **Sprint** | {{Sprint name}} |

## Description

{{Clear, concise description of what this ticket is about and why it matters. Link to relevant docs inline.}}

## Acceptance Criteria

- [ ] {{Criterion 1 — specific and testable}}
- [ ] {{Criterion 2 — specific and testable}}
- [ ] {{Criterion 3 — specific and testable}}

## Definition of Done

- [ ] Code reviewed and approved
- [ ] Unit tests written and passing
- [ ] Integration tests passing
- [ ] Documentation updated
- [ ] Deployed to staging and verified

<!-- BUG-SPECIFIC — include only for Bug type tickets -->

## Steps to Reproduce

1. {{Step 1}}
2. {{Step 2}}
3. {{Step 3}}

## Expected Behaviour

{{What should happen.}}

## Actual Behaviour

{{What actually happens.}}

## Environment

| Field | Value |
|-------|-------|
| **Browser/Device** | {{e.g. Chrome 120 / iPhone 15}} |
| **OS** | {{e.g. macOS 14.2 / iOS 17}} |
| **Environment** | {{Staging / Production}} |
| **Build/Version** | {{e.g. v2.3.1}} |

<!-- END BUG-SPECIFIC -->

<!-- EPIC-SPECIFIC — include only for Epic type tickets -->

## Goal

{{What business or technical outcome does this epic deliver?}}

## Scope

**In scope:**
- {{Item 1}}
- {{Item 2}}

**Out of scope:**
- {{Item A}}
- {{Item B}}

## Success Metrics

| Metric | Target | How measured |
|--------|--------|-------------|
| {{Metric 1}} | {{Target}} | {{Tool / method}} |
| {{Metric 2}} | {{Target}} | {{Tool / method}} |

## Stories

| Ticket | Title | Status |
|--------|-------|--------|
| {{PROJ-101}} | {{Story 1}} | 🟡 In Progress |
| {{PROJ-102}} | {{Story 2}} | ⚪ To Do |

<!-- END EPIC-SPECIFIC -->

<!-- SPIKE-SPECIFIC — include only for Spike type tickets -->

## Research Question

{{What are we trying to learn or prove?}}

## Timebox

{{X days / X story points — hard stop.}}

## Expected Output

- {{Document / ADR summarising findings}}
- {{Proof of concept (if applicable)}}
- {{Recommendation for next steps}}

<!-- END SPIKE-SPECIFIC -->

## Related

- {{Link to related workspace docs}}
- {{Link to related external resources}}
- {{Link to related JIRA tickets or Confluence pages}}
