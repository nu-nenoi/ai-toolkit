# ADR (Architecture Decision Record) Patterns

An ADR captures **one** significant architectural decision: the context that forced it, the options considered, the choice made, and the consequences. ADRs are immutable historical records — once accepted, they are not edited; they are superseded by new ADRs.

## Numbering

Number ADRs sequentially: `adr-001-<slug>.md`, `adr-002-<slug>.md`. Look in the workspace for an existing ADR folder (e.g. `docs/architecture/architecture-decision-records/`) and pick the next number.

## Structure

Always include these H2 sections in this order:

1. **Status** — Proposed / Accepted / Superseded (link to superseding ADR if applicable)
2. **Context** — what problem or constraint forced the decision
3. **Decision** — the choice, in 1–2 sentences
4. **Options Considered** — table or list of alternatives with pros/cons
5. **Consequences** — positive, negative, neutral effects
6. **References** — links to related ADRs, RFCs, docs

## Status

Every ADR opens with a status table:

```markdown
| Field | Value |
|-------|-------|
| **Number** | ADR-003 |
| **Status** | Accepted |
| **Date** | 2026-04-25 |
| **Deciders** | {{names / teams}} |
| **Supersedes** | — |
| **Superseded by** | — |
```

For Superseded ADRs, fill in the `Superseded by` link to the new ADR.

## Options Considered

Use a table when there are 3+ options:

```markdown
| Option | Pros | Cons |
|--------|------|------|
| A — Use Redis | Fast, well-supported | Operational overhead |
| B — Use Memcached | Simpler ops | Less feature-rich |
| C — In-process cache | No new infra | Doesn't scale across nodes |
```

## Consequences

Group into three buckets — positive, negative, neutral. This forces honest accounting.

```markdown
**Positive**
- {{benefit 1}}
- {{benefit 2}}

**Negative**
- {{cost 1}}
- {{cost 2}}

**Neutral**
- {{change that is neither good nor bad but worth noting}}
```

## Anti-patterns

- **Do not** edit an Accepted ADR. Create a new ADR that supersedes it.
- **Do not** include implementation detail beyond what's needed to communicate the decision — link out to a design doc instead.
- **Do not** use ADRs for trivial decisions. Reserve them for choices that are hard to reverse.
