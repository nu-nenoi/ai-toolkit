---
name: document
description: "Write structured Markdown documentation. Use when: creating a README, ADR, design doc, RFC, runbook, playbook, release notes, incident report, API doc, troubleshooting guide, FAQ, onboarding guide, contributing guide, changelog, user guide, meeting notes, Confluence page, JIRA ticket, or any technical document. Handles tables of contents, summaries, conclusions, comparison tables, decision matrices, glossaries, and diagram placeholders."
metadata:
  version: "1.0.0"
  argument-hint: "Provide as much detail as possible: what this document is about, its purpose, the audience, key decisions or points to capture, any constraints, related tickets, links to resources, and files or directories the agent should read. The more context you provide here, the less the agent will need to ask."
---

# Document

Produces well-structured Markdown documentation. Output is one `.md` file or a directory of `.md` files, paste-compatible with Confluence, JIRA, GitHub, Dropbox Paper, etc.

The skill is **format-neutral** — domain-specific links (internal wikis, design systems, vendor docs) are added based on what the agent finds in the workspace and what the user provides.

## Mandatory workflow

Run every step in order; do not skip or collapse steps even if the user's prompt seems to make one unnecessary. If a step was missed, stop and go back.

1. **Preflight** — context-readiness question; loop until user confirms "All provided"
2. **Pass 1** — 4 universal questions (document-type, topic, audience, length); wait for answers
3. **Pass 2** — applicable conditional questions (subtype, special-sections, TOC, diagrams); wait for answers
4. **Step 1** — load type-specific references and templates
5. **Step 2** — discover existing documentation in the workspace
6. **Step 3** — draft skeleton (headings + 1-line section summaries; no body)
7. **Step 4** — ask: single file or directory output
8. **Step 5** — present skeleton for mid-review; loop until approved
9. **Step 6** — fill in content (only after every step above is complete)
10. **Step 7** — self-review checklist; fix and re-validate on failure
11. **Step 8** — write the output

## Planning phase

Run Preflight, Pass 1, and Pass 2 as **separate** `vscode_askQuestions` calls; wait for answers between each. See [planning-questions.md](./references/planning-questions.md) for full payloads, conditional rules, and the diagram routing table.

## Generation procedure

### Step 1 — Load references

Always load:
- [formatting-rules.md](./references/formatting-rules.md) — header hierarchy, TOC rules, callouts
- [doc-type-catalogue.md](./references/doc-type-catalogue.md) — type catalogue, subtypes, TOC eligibility
- [linking-strategy.md](./references/linking-strategy.md) — finding and embedding links
- [structure-review.md](./references/structure-review.md) — mid-review mechanics

Then load the type-specific reference and template:

| Document type | Reference file | Template |
|---|---|---|
| README | [readme-patterns.md](./references/readme-patterns.md) | [readme.md](./assets/templates/readme.md) |
| ADR | [adr-patterns.md](./references/adr-patterns.md) | [adr.md](./assets/templates/adr.md) |
| Runbook | [runbook-patterns.md](./references/runbook-patterns.md) | [runbook.md](./assets/templates/runbook.md) |
| Playbook | [runbook-patterns.md](./references/runbook-patterns.md) | [playbook.md](./assets/templates/playbook.md) |
| Release notes | [release-notes-patterns.md](./references/release-notes-patterns.md) | [release-notes.md](./assets/templates/release-notes.md) |
| Incident report | [incident-report-patterns.md](./references/incident-report-patterns.md) | [incident-report.md](./assets/templates/incident-report.md) |
| API documentation | [api-doc-patterns.md](./references/api-doc-patterns.md) | [api-doc.md](./assets/templates/api-doc.md) |
| Confluence page | [confluence-patterns.md](./references/confluence-patterns.md) | [confluence.md](./assets/templates/confluence.md) |
| JIRA ticket | [jira-patterns.md](./references/jira-patterns.md) | [jira.md](./assets/templates/jira.md) |
| FAQ / Changelog / Meeting notes / Troubleshooting / Onboarding / Contributing / User guide / Design doc / RFC / Other | [short-doc-patterns.md](./references/short-doc-patterns.md) | one of: [faq.md](./assets/templates/faq.md), [changelog.md](./assets/templates/changelog.md), [meeting-notes.md](./assets/templates/meeting-notes.md), [generic.md](./assets/templates/generic.md) |

### Step 2 — Discover existing documentation

Apply [linking-strategy.md](./references/linking-strategy.md): search for `README.md`, `*.md`, `docs/` folders, ADRs, related vendor docs and standards. Note them for inline linking in Step 6.

### Step 3 — Draft skeleton only

Produce H1 + H2 headers + a single sentence per section describing what it will contain. **No body content.**

If the content naturally splits into multiple files (e.g. an epic with sub-tickets, a multi-chapter guide), note this in the skeleton — Step 4 lets the user decide.

### Step 4 — Choose output format

Ask the output-format question (see [planning-questions.md](./references/planning-questions.md)).

If **Directory**: include the proposed structure (folder name, file names, one-line purpose each) in the Step 3 skeleton. Use kebab-case file names that match the subject (e.g. `overview.md`, `spike-ai-limitations.md`). No mandatory `index.md`. Each file follows the same formatting rules; use relative links between files.

### Step 5 — Mid-review

Present the skeleton via the structure-review question (see [planning-questions.md](./references/planning-questions.md)). Apply requested changes and re-present until approved. If the user adds more context, revise the structure before re-presenting. See [structure-review.md](./references/structure-review.md) for examples.

### Step 6 — Fill in content

Verify every prior step is complete; if anything is missing, stop and complete it. Then with the approved structure:

1. Fill each section based on the user's topic and audience
2. Apply [formatting-rules.md](./references/formatting-rules.md) strictly
3. Add only the special sections selected in Pass 2 Q6
4. Insert TOC only if Q7 = Yes
5. Handle diagrams per the routing table in [planning-questions.md](./references/planning-questions.md)
6. Embed Step 2 links inline at the first relevant mention

### Step 7 — Self-review

- [ ] Exactly one H1 per file
- [ ] No H4 (`####`) — replace with **bold** + content
- [ ] If TOC present, every H2 appears in it (and vice versa)
- [ ] No unfilled `{{placeholders}}`
- [ ] No relative links point to non-existent paths
- [ ] Special sections match user's selection — no extras or omissions
- [ ] Length matches user's preference (concise / standard / detailed)
- [ ] If directory output: all inter-file relative links resolve; each file has exactly one H1

If any check fails, fix and re-validate before writing.

### Step 8 — Write the output

**Single file**: write one `.md` with a kebab-case filename based on topic and type (e.g. `adr-001-caching-strategy.md`, `runbook-rotate-database-credentials.md`).

**Directory**: create a kebab-case folder (e.g. `epic-adopt-ai-copilot/`) and write each `.md` inside. Verify all relative links resolve.

## Hard rules

These are enforced by the Step 7 checklist; full details in [formatting-rules.md](./references/formatting-rules.md):

- Exactly one H1 per file; **never** use H4 (replace with **bold** + content or a table)
- TOC contains every H2 and only H2s
- Inline links on first mention; specify the language on every fenced code block
