# JIRA Ticket Patterns

Guidance for generating JIRA-style tickets in Markdown. The output is a `.md` file that captures the full ticket content, ready to paste into JIRA's description or comment fields.

## Ticket Types

Tailor the structure based on the ticket type:

| Type | Focus |
|------|-------|
| **Epic** | High-level goal, linked stories, success criteria |
| **Story** | User value, acceptance criteria, definition of done |
| **Bug** | Reproduction steps, expected vs actual, environment |
| **Task** | Actionable steps, deliverables, dependencies |
| **Spike** | Research question, timebox, expected output |

## Standard Structure

Every JIRA ticket should include at minimum:

```markdown
# [TYPE] Title of the ticket

| Field | Value |
|-------|-------|
| **Type** | Story / Bug / Task / Epic / Spike |
| **Priority** | 🔴 Critical / 🟠 High / 🟡 Medium / 🟢 Low |
| **Labels** | `label-1`, `label-2` |
| **Epic** | [EPIC-123](link) |
| **Sprint** | Sprint name (if known) |

## Description

Clear, concise description of what this ticket is about and why it matters.

## Acceptance Criteria

- [ ] Criterion 1 — specific, testable
- [ ] Criterion 2 — specific, testable
- [ ] Criterion 3 — specific, testable

## Definition of Done

- [ ] Code reviewed and approved
- [ ] Unit tests written and passing
- [ ] Integration tests passing
- [ ] Documentation updated
- [ ] Deployed to staging and verified
```

## Bug-Specific Sections

For bug tickets, add these sections after the description:

```markdown
## Steps to Reproduce

1. Go to...
2. Click on...
3. Observe...

## Expected Behaviour

What should happen.

## Actual Behaviour

What actually happens.

## Environment

| Field | Value |
|-------|-------|
| **Browser/Device** | Chrome 120 / iPhone 15 |
| **OS** | macOS 14.2 / iOS 17 |
| **Environment** | Staging / Production |
| **Build/Version** | v2.3.1 |

## Screenshots / Logs

Attach or describe relevant evidence.
```

## Epic-Specific Sections

For epics, include:

```markdown
## Goal

What business or technical outcome does this epic deliver?

## Scope

**In scope:**
- Item 1
- Item 2

**Out of scope:**
- Item A
- Item B

## Success Metrics

| Metric | Target | How measured |
|--------|--------|-------------|
| Latency | < 200ms p99 | CloudWatch |
| Error rate | < 0.1% | Datadog |

## Stories

| Ticket | Title | Status |
|--------|-------|--------|
| PROJ-101 | Story 1 | 🟡 In Progress |
| PROJ-102 | Story 2 | 🟢 Done |
| PROJ-103 | Story 3 | ⚪ To Do |
```

## Spike-Specific Sections

```markdown
## Research Question

What are we trying to learn or prove?

## Timebox

X days / X story points — hard stop.

## Expected Output

- Document / ADR summarising findings
- Proof of concept (if applicable)
- Recommendation for next steps
```

## Linking

- Link to related JIRA tickets: `[PROJ-123](https://jira.example.com/browse/PROJ-123)`
- Link to Confluence pages for context
- Link to workspace code/docs with relative paths. See [linking-strategy.md](./linking-strategy.md).

## Labels and Components

Suggest labels based on the topic. Common cross-cutting labels:
- `tech-debt`, `security`, `accessibility`, `performance`, `reliability`, `documentation`

Domain-specific labels (component name, service name, area) should be inferred from the workspace or the user's topic context.
