---
name: document
description: "Write structured Markdown documentation. Use when: creating a README, ADR, design doc, RFC, runbook, playbook, release notes, incident report, API doc, troubleshooting guide, FAQ, onboarding guide, contributing guide, changelog, user guide, meeting notes, Confluence page, JIRA ticket, or any technical document. Handles tables of contents, summaries, conclusions, comparison tables, decision matrices, glossaries, and diagram placeholders."
argument-hint: "Provide as much detail as possible: what this document is about, its purpose, the audience, key decisions or points to capture, any constraints, related tickets, links to resources, and files or directories the agent should read. The more context you provide here, the less the agent will need to ask."
---

# Document

A skill for producing well-structured Markdown documentation of any kind. Output is always a `.md` file (or a directory of `.md` files) that can be pasted into Confluence, JIRA, Dropbox Paper, GitHub, or any Markdown-compatible surface.

The skill is **format-neutral** — it has no opinion about the project, team, company, or domain. Domain-specific links (e.g. internal wikis, design systems, vendor docs) are added based on what the agent finds in the workspace and what the user provides as context.

## Workflow Overview

```
Preflight check   →   Pass 1 (universal)   →   Pass 2 (conditional)
       ↓                     ↓                         ↓
  context ready?        type, topic,            subtype, sections,
                        audience, length        TOC, diagrams
                                                       ↓
                                               Draft structure
                                                       ↓
                                               Output format (file or directory)
                                                       ↓
                                               Mid-review checkpoint
                                                       ↓
                                               Fill in details
                                                       ↓
                                               Self-review → Write output
```

## Step Compliance — Every Step Is Mandatory

**Every step in this workflow and every first-level question within a pass MUST be executed in order.** No step may be skipped, inferred from context, collapsed into another step, or assumed from the user's initial prompt. If the user's request appears to make a step unnecessary, ask anyway — the user may have additional input.

If you realise a step was missed after moving past it, **stop and go back** to complete it before continuing. Output generated without completing all steps is invalid.

The complete mandatory sequence is:

1. **Preflight Check** — ask the context-readiness question; loop until the user confirms "All provided"
2. **Pass 1** — ask all 4 questions (document-type, topic, audience, length); wait for answers
3. **Pass 2** — ask all applicable conditional questions (subtype, special-sections, TOC, diagrams); wait for answers
4. **Step 1** — Load type-specific references and templates
5. **Step 2** — Discover existing documentation in the workspace
6. **Step 3** — Draft structure (skeleton only — no body content)
7. **Step 4** — Ask the user to choose output format (single file or directory) via `vscode_askQuestions`
8. **Step 5** — Present draft structure for mid-review; loop until user approves
9. **Step 6** — Fill in details (content generation begins here — not before)
10. **Step 7** — Self-review checklist
11. **Step 8** — Write the output

## Planning Phase — ALWAYS RUN FIRST

Before generating any content, run the two question passes via `vscode_askQuestions`. **Wait for answers before each pass — do not batch both passes into one call.**

### Preflight Check — context readiness (always asked first)

Run this single question as its own `vscode_askQuestions` call **before** Pass 1. Its purpose is to prompt the user to share all available context upfront — the quality of the output depends entirely on what is provided here.

```
header: "context-readiness"
question: "Before we start: to produce the most accurate document possible, please share everything relevant upfront. Have you provided all of the following?"
options:
  - "Description — what the document is about, its purpose, key points, decisions to capture"
  - "Files or directories — source code, configs, existing docs, schemas the agent should read"
  - "Links or resources — external docs, RFCs, vendor pages, related tickets, design mocks"
  - "I need to add more — let me paste or attach additional context before continuing"
  - "All provided — proceed to planning questions"
multiSelect: true
allowFreeformInput: true
```

**If the user selects "I need to add more" (or does NOT select "All provided"),** pause and prompt them with hints about what might be missing:

> **Hint — types of context that improve output quality:**
> - **Written description**: purpose of the doc, key points, decisions, constraints, background
> - **Source files or directories**: code, configs, schemas, existing docs the agent should read — attach or reference by path
> - **Links**: JIRA tickets, Confluence pages, RFCs, vendor docs, design mocks, Slack threads
> - **Examples**: sample outputs, similar docs from other projects, style references
> - **Data**: metrics, API responses, log excerpts, error messages, test results
> - **People / roles**: who the stakeholders are, who will review, who owns the topic

Repeat the preflight check after they respond — do not proceed to Pass 1 until the user confirms they are ready. The user can request to add more context at **any point** during the workflow by saying "I want to add more context" — the agent should always accept additional files, links, or descriptions mid-flow without restarting.

### Pass 1 — universal (always asked)

Always include a 1-line description on each option so the user knows what they are picking. See [doc-type-catalogue.md](./references/doc-type-catalogue.md) for the full list of types and their descriptions.

```
1. header: "document-type"
   question: "What type of document are you creating?"
   options: <full list from doc-type-catalogue.md, each with its description>
   allowFreeformInput: true

2. header: "topic"
   question: "Topic / context (optional — leave blank to skip). Tell the agent what the doc is about: subject, key points, links, anything that helps. This is NOT the final title — it is context."
   (free text, no options)

3. header: "audience"
   question: "Who is the primary audience?"
   options:
     - "Developers — engineers reading code or building on the work"
     - "End users — people using the product, no technical background assumed"
     - "Stakeholders — product, leadership, business; care about outcomes not implementation"
     - "Mixed — write for the broadest audience with technical detail in collapsible sections"

4. header: "length"
   question: "How detailed should the document be?"
   options:
     - "Concise — minimum viable doc, only the essentials"
     - "Standard — balanced detail; covers the topic without exhaustive depth"
     - "Detailed — thorough; include background, alternatives, edge cases"
```

### Pass 2 — conditional (asked AFTER Pass 1 answers are received)

Only include each question when it applies. See [doc-type-catalogue.md](./references/doc-type-catalogue.md) for which types have subtypes and which warrant a TOC.

```
5. header: "subtype"
   question: "<type-specific subtype question>"
   ASKED ONLY IF the chosen document type has subtypes. Examples:
     - JIRA ticket → Story / Bug / Task / Epic / Spike / Subtask
     - README → Application / Library / Monorepo root / Sub-package
     - ADR → Status: Proposed / Accepted / Superseded
     - Release notes → Audience: Developers / End users (and ask for version)
     - Incident report → Severity (SEV-1…SEV-5) and Status (active / resolved)
     - API documentation → REST / GraphQL / Library or SDK / CLI
     - Meeting notes → Standup / Planning / Review / Retrospective / Ad-hoc

6. header: "special-sections"
   question: "Which special sections should be included? (optional — leave blank to skip)"
   multiSelect: true
   options:
     - "Summary / TL;DR — 3–5 bullet recap at the top"
     - "Conclusions — clear closing statements"
     - "Comparison table — side-by-side option comparison"
     - "Decision matrix — structured decision record (decision, date, rationale)"
     - "Key risks — risk × likelihood × impact × mitigation table"
     - "Next steps / Action items — checkboxed follow-ups with owners"
     - "Glossary — term-definition table"
   allowFreeformInput: true

7. header: "table-of-contents"
   question: "Include a Table of Contents? (a clickable list of H2 section links at the top — useful for long documents)"
   ASKED ONLY IF the document type warrants it. Skip for short types (JIRA ticket,
   meeting notes, FAQ, changelog, release notes — see doc-type-catalogue.md).
   options:
     - Yes
     - No

8. header: "diagram-intent"
   question: "How should diagrams be handled in this document?"
   options:
     - "⚠️ EXPERIMENTAL: Draw and insert diagrams — agent generates diagram code/markup (auto-generated diagrams may not be accurate; review before publishing)"
     - "Embed diagrams I attached — I have pasted or attached diagram files/images; reference them in the document"
     - "Placeholders only — reserve clearly-marked spots; I will create the diagrams separately"
     - "No diagrams — text only"

9. header: "diagram-type"
   question: "What type of diagram(s) do you need? (multi-select)"
   ASKED ONLY IF the user chose "Draw and insert diagrams" for question 8.
   multiSelect: true
   options:
     - "C4 — system context, container, component or code diagram"
     - "Flow diagram — process or decision flow"
     - "Sequence diagram — interactions between components or actors over time"
     - "Class diagram — object structure and relationships"
     - "Entity-Relationship (ER) — data model"
     - "Architecture overview — freeform system topology"
     - "Other (describe in the topic field)"

10. header: "diagram-tool"
    question: "Which tool should render the diagram(s)?"
    ASKED ONLY IF the user chose "Draw and insert diagrams" for question 8.
    options:
      - "Mermaid — embedded as fenced code blocks directly in Markdown"
      - "draw.io / diagrams.net — XML export or link to shared file"
      - "Miro — link to board"
      - "Excalidraw — embed or link"
      - "PlantUML — text-based UML markup"
      - "Other (specify)"
    allowFreeformInput: true
```

**Diagram routing logic (used in Step 6):**

| Q8 answer | Q9 + Q10 asked? | Step 6 behaviour |
|-----------|----------------|------------------|
| Draw and insert (experimental) | Yes | Generate diagram in the chosen tool's format; add `> ⚠️ Auto-generated — verify accuracy before publishing` callout beneath each diagram |
| Embed attached | No | Reference attached files/images inline at the first relevant point |
| Placeholders only | No | Insert `> **📊 Diagram placeholder:** …` blocks |
| No diagrams | No | Text only |

## Generation Procedure

### Step 1 — Load references

Always load:
- [formatting-rules.md](./references/formatting-rules.md) — header hierarchy, TOC rules, callouts
- [doc-type-catalogue.md](./references/doc-type-catalogue.md) — full type catalogue and per-type metadata (subtypes, TOC eligibility)
- [linking-strategy.md](./references/linking-strategy.md) — how to find and embed useful links
- [structure-review.md](./references/structure-review.md) — how the mid-review step works

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

Apply [linking-strategy.md](./references/linking-strategy.md):
- Search the workspace for related docs (`README.md`, `*.md`, `docs/` folders, ADRs)
- Identify standards, RFCs, vendor docs that relate to the topic
- Note these for inline linking during Step 6

### Step 3 — Draft structure ONLY

Produce a skeleton: the H1 title, the H2 section headers, and a single sentence per section describing what it will contain. **Do not write the body content yet.**

If the content is complex or naturally splits into multiple files (e.g. an epic with sub-tickets, a multi-chapter guide, nested documentation), note this in the skeleton — the output format question in Step 4 will let the user decide.

### Step 4 — Choose output format

Present the output format question to the user via `vscode_askQuestions`:

```
header: "output-format"
question: "How should the documentation be written?"
options:
  - "Single file — one .md file containing all content"
  - "Directory — a folder with multiple .md files (e.g. an epic with linked sub-tickets, nested docs, a guide split into chapters)"
allowFreeformInput: true
```

**If the user chooses "Directory":**
- Include the proposed directory structure (folder name, file names, and a one-line purpose for each file) in the draft structure from Step 3.
- Name each file descriptively to match its subject — use kebab-case (e.g. `overview.md`, `story-per-context-instructions.md`, `spike-ai-limitations.md`). No mandatory `index.md` or `README.md` — let the content dictate the names.
- Each file follows the same formatting rules as a single-file output.
- Use relative links between files within the directory.

**If the user chooses "Single file":**
- Proceed as before — one `.md` file.

### Step 5 — Mid-review checkpoint

Present the draft structure to the user via `vscode_askQuestions`:

```
header: "structure-review"
question: "Review the proposed structure. What would you like to do?"
options:
  - "Approve — proceed to fill in content"
  - "Add a section"
  - "Remove a section"
  - "Reorder sections"
  - "Edit a section heading or summary"
  - "Add more context — I want to attach files, links, or details I missed earlier"
allowFreeformInput: true
```

If the user selects "Add more context", accept the additional input (with the same hints listed in the Preflight Check) and revise the structure accordingly before re-presenting.

Apply requested changes and re-present until the user approves. See [structure-review.md](./references/structure-review.md) for examples.

### Pre-Generation Gate — verify before writing content

Before proceeding to Step 6, verify that **every** prior step has been completed. Walk through the checklist below — if any box is unchecked, **STOP and go back to complete the missing step. Do not generate content until all boxes are checked.**

- [ ] Preflight Check was asked and user confirmed readiness ("All provided")
- [ ] Pass 1 Q1 answered — document type chosen
- [ ] Pass 1 Q2 answered — topic / context provided (or explicitly skipped by user)
- [ ] Pass 1 Q3 answered — audience chosen
- [ ] Pass 1 Q4 answered — length / detail level chosen
- [ ] All applicable Pass 2 questions asked and answered (subtype, special-sections, TOC, diagrams)
- [ ] Step 1 — type-specific references and templates loaded
- [ ] Step 2 — existing workspace documentation discovered
- [ ] Step 3 — draft structure (skeleton) produced
- [ ] Step 4 — output format explicitly chosen by user (single file **or** directory)
- [ ] Step 5 — draft structure approved by user via mid-review checkpoint

### Step 6 — Fill in details

With the approved structure:

1. Fill each section with content based on the user's topic and audience
2. Apply formatting rules from [formatting-rules.md](./references/formatting-rules.md) strictly
3. Add only the special sections selected in Pass 2 question 6
4. Insert TOC only if the user answered Yes to question 7
5. Handle diagrams according to the Q8 routing logic above (draw / embed / placeholder / none)
6. Embed links discovered in Step 2 inline at the first relevant mention

### Step 7 — Self-review

Before writing the output, validate:

- [ ] Exactly one H1 per file
- [ ] No H4 (`####`) used anywhere — replace with **bold text** + content
- [ ] If a TOC is present, every H2 appears in it (and vice versa)
- [ ] No unfilled `{{placeholders}}` remain
- [ ] No relative links point to non-existent paths
- [ ] Special sections match the user's selection — no extras, none missing
- [ ] Length matches the user's preference (concise / standard / detailed)
- [ ] If directory output: all inter-file relative links resolve; each file has exactly one H1

If any check fails, fix and re-validate before writing.

### Step 8 — Write the output

Write the documentation according to the output format chosen in Step 4:

**Single file** — write one `.md` file. The agent picks a sensible filename based on the topic and type (e.g. `adr-001-caching-strategy.md`, `runbook-rotate-database-credentials.md`). Use kebab-case.

**Directory** — create the folder and all files as proposed in the approved structure:
- Create the directory with a kebab-case name (e.g. `epic-adopt-ai-copilot/`, `api-docs-ingress/`)
- Write each `.md` file inside with descriptive subject-based names
- Verify all relative links between files resolve correctly

## Formatting Rules (Quick Reference)

Full details in [formatting-rules.md](./references/formatting-rules.md):

- **H1** (`#`) — Document title only. Exactly one per file.
- **H2** (`##`) — Major sections. Only these go in a TOC.
- **H3** (`###`) — Use sparingly for subsections.
- **H4** (`####`) — **Forbidden.** Replace with **bold text** + content or a table.
- **Tables** — Prefer tables over nested header hierarchies.
- **Callouts** — Use `> **Note:**`, `> **Warning:**`, `> **Tip:**` blockquotes.
- **Links** — Inline `[text](url)`, not reference-style. Link on first mention.
- **Code** — Always specify the language for syntax highlighting.

## Interactivity

Make every document useful by:
- Linking to related internal docs with relative paths
- Linking to external standards / specs / vendor docs where relevant
- Using `<details>` for lengthy supplementary content
- Using tables for any structured comparison
- Adding "See also" inline references after key paragraphs, not at the end
