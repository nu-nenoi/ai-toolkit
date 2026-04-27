# Confluence Page Patterns

Guidance for generating Confluence-style documentation in Markdown. Since the output is a `.md` file that will be pasted into Confluence, use patterns that Confluence renders well from Markdown or that can be easily adapted.

## Structure

A Confluence page follows this structure:

1. **Title** (H1) — clear, descriptive, searchable
2. **Table of Contents** (if requested) — from H1 + H2 anchors only
3. **Status / metadata panel** — use a table at the top
4. **Body sections** (H2) — 5–8 major sections max
5. **Related pages / See also** — inline, not a trailing dump

## Status / Metadata Panel

Place a metadata table immediately after the title (before TOC if present):

```markdown
| Field | Value |
|-------|-------|
| **Status** | 🟢 Active / 🟡 Draft / 🔴 Deprecated |
| **Owner** | Team / Person name |
| **Last updated** | YYYY-MM-DD |
| **Confluence space** | SPACE-KEY |
| **Related JIRA** | [PROJECT-123](https://jira.example.com/browse/PROJECT-123) |
```

## Info / Note / Warning Panels

Confluence has coloured panels. In Markdown, use blockquote callouts that visually approximate them:

```markdown
> **ℹ️ Info:** Background context or additional detail.

> **⚠️ Warning:** Something that could cause issues if ignored.

> **✅ Tip:** A helpful shortcut or recommendation.

> **🔴 Critical:** This must be addressed before proceeding.
```

## Expandable Sections

Use `<details>` for content that is important but not needed on first read:

```markdown
<details>
<summary>Architecture decision context (click to expand)</summary>

Detailed content here...

</details>
```

## Diagrams

- Reference existing diagrams with image links or Mermaid blocks.
- Use Mermaid syntax where the target Confluence instance supports it:

```markdown
```mermaid
graph LR
  A[Service A] --> B[Service B]
  B --> C[Database]
```​
```

## Decision Log Pattern

If the page documents a decision, use this structure:

```markdown
## Decision

| Aspect | Detail |
|--------|--------|
| **Decision** | What was decided |
| **Date** | YYYY-MM-DD |
| **Participants** | Names / teams |
| **Status** | Accepted / Proposed / Superseded |

## Context

Why this decision was needed.

## Options Considered

| Option | Pros | Cons |
|--------|------|------|
| Option A | Fast to implement | Limited scalability |
| Option B | Scales well | Higher complexity |

## Outcome

What was chosen and why.
```

## Cross-Linking

- Link to other Confluence pages by title (the user can convert to Confluence links on paste).
- Link to JIRA tickets inline: `[PROJECT-123](https://jira.example.com/browse/PROJECT-123)`.
- Link to workspace docs using relative paths. See [linking-strategy.md](./linking-strategy.md).

## Emoji Usage

Use emoji sparingly for status indicators and visual scanning:

| Emoji | Meaning |
|-------|---------|
| 🟢 | Active / Done / OK |
| 🟡 | In progress / Draft / Warning |
| 🔴 | Blocked / Deprecated / Critical |
| ℹ️ | Information |
| ⚠️ | Warning |
| ✅ | Tip / Approved |
| 📌 | Pinned / Important |
