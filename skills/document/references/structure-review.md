# Structure Review (Mid-Review Checkpoint)

The mid-review step gives the user a chance to shape the document's skeleton before content is generated. This catches structural mistakes early — before time is spent writing detailed prose.

## What to present

Show only:
- The H1 title
- All H2 section headings
- One sentence per section describing what it will contain

Do **NOT** write any actual section content yet.

## Example

For an ADR titled "Use Redis for session storage":

```
# ADR 003 — Use Redis for session storage

## Status
One-line summary of current ADR status.

## Context
What's driving this decision: current pain points and constraints.

## Decision
The chosen approach in one or two sentences.

## Options Considered
Side-by-side comparison of Redis, Memcached, and in-process cache.

## Consequences
Positive, negative, and neutral effects of the decision.

## References
Links to related ADRs, RFCs, and team docs.
```

Then ask via `vscode_askQuestions`:

```
header: "structure-review"
question: "Review the proposed structure. What would you like to do?"
options:
  - "Approve — proceed to fill in content"
  - "Add a section"
  - "Remove a section"
  - "Reorder sections"
  - "Edit a section heading or summary"
allowFreeformInput: true
```

## Iteration

If the user picks anything other than Approve, capture their change (use the freeform input or a follow-up question for specifics), apply it, and re-present the updated structure. Loop until Approved.

## When to skip

For very short docs (JIRA ticket, FAQ entry, changelog line), the structure is trivial — skip the mid-review step. Use this rule:

| Doc type | Skip mid-review? |
|----------|-----------------|
| JIRA ticket | Yes |
| Meeting notes | Yes |
| Changelog | Yes |
| FAQ (single Q&A) | Yes |
| Everything else | No |

## What this prevents

- Generating 800 words in the wrong section order, then having to reshuffle
- Including or omitting the wrong special sections
- Misjudging whether the doc needs an "Alternatives" or "Implementation plan" section
- Writing a runbook that omits the "Rollback" step
