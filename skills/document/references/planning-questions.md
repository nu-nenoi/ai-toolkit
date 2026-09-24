# Planning Questions

Full `vscode_askQuestions` payloads for the planning phase. The main `SKILL.md` lists the workflow; this file is the source of truth for question wording, options, and routing.

Each pass is a **separate** `vscode_askQuestions` call. Wait for answers before issuing the next call.

## Preflight Check (always asked first)

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

If the user does NOT select "All provided", pause and prompt with hints:

> **Hint — types of context that improve output quality:**
> - **Written description**: purpose, key points, decisions, constraints, background
> - **Source files or directories**: code, configs, schemas, existing docs — attach or reference by path
> - **Links**: JIRA tickets, Confluence pages, RFCs, vendor docs, design mocks, Slack threads
> - **Examples**: sample outputs, similar docs from other projects, style references
> - **Data**: metrics, API responses, log excerpts, error messages, test results
> - **People / roles**: stakeholders, reviewers, topic owners

Repeat the preflight check until the user confirms readiness. The user can request to add context at any later point — accept it without restarting.

## Pass 1 — universal (always asked, after Preflight)

Always show a 1-line description on each option. Pull document-type options and descriptions from [doc-type-catalogue.md](./doc-type-catalogue.md).

```
1. header: "document-type"
   question: "What type of document are you creating?"
   options: <full list from doc-type-catalogue.md, each with its description>
   allowFreeformInput: true

2. header: "topic"
   question: "Topic / context (optional). Subject, key points, links — anything that helps. NOT the final title."
   (free text)

3. header: "audience"
   question: "Who is the primary audience?"
   options:
     - "Developers — engineers reading code or building on the work"
     - "End users — people using the product, no technical background assumed"
     - "Stakeholders — product, leadership, business; care about outcomes"
     - "Mixed — write for the broadest audience with technical detail in collapsible sections"

4. header: "length"
   question: "How detailed should the document be?"
   options:
     - "Concise — minimum viable doc, only the essentials"
     - "Standard — balanced detail; covers the topic without exhaustive depth"
     - "Detailed — thorough; include background, alternatives, edge cases"
```

## Pass 2 — conditional (asked after Pass 1 answers received)

Only include each question when it applies. Consult [doc-type-catalogue.md](./doc-type-catalogue.md) for which types have subtypes and which warrant a TOC.

```
5. header: "subtype"
   ASKED ONLY IF the chosen document type has subtypes. Examples:
     - JIRA ticket → Story / Bug / Task / Epic / Spike / Subtask
     - README → Application / Library / Monorepo root / Sub-package
     - ADR → Status: Proposed / Accepted / Superseded
     - Release notes → Audience: Developers / End users (and ask for version)
     - Incident report → Severity (SEV-1…SEV-5) and Status (active / resolved)
     - API documentation → REST / GraphQL / Library or SDK / CLI
     - Meeting notes → Standup / Planning / Review / Retrospective / Ad-hoc

6. header: "special-sections"
   question: "Which special sections should be included? (optional)"
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
   question: "Include a Table of Contents?"
   ASKED ONLY IF the doc type warrants it. Skip for short types (JIRA ticket,
   meeting notes, FAQ, changelog, release notes — see doc-type-catalogue.md).
   options: [Yes, No]

8. header: "diagram-intent"
   question: "How should diagrams be handled?"
   options:
     - "⚠️ EXPERIMENTAL: Draw and insert diagrams — agent generates diagram code (review before publishing)"
     - "Embed diagrams I attached"
     - "Placeholders only — I will create the diagrams separately"
     - "No diagrams — text only"

9. header: "diagram-type"
   ASKED ONLY IF Q8 = "Draw and insert diagrams"
   multiSelect: true
   options:
     - "C4 — system context, container, component or code"
     - "Flow diagram — process or decision flow"
     - "Sequence diagram"
     - "Class diagram"
     - "Entity-Relationship (ER)"
     - "Architecture overview — freeform topology"
     - "Other (describe in the topic field)"

10. header: "diagram-tool"
    ASKED ONLY IF Q8 = "Draw and insert diagrams"
    options:
      - "Mermaid — fenced code blocks in Markdown"
      - "draw.io / diagrams.net — XML or shared link"
      - "Miro — link to board"
      - "Excalidraw — embed or link"
      - "PlantUML — text-based UML"
      - "Other (specify)"
    allowFreeformInput: true
```

## Diagram routing (used in Step 6)

| Q8 answer | Q9 + Q10 asked? | Step 6 behaviour |
|---|---|---|
| Draw and insert (experimental) | Yes | Generate diagrams in chosen tool's format; add `> ⚠️ Auto-generated — verify before publishing` callout beneath each |
| Embed attached | No | Reference attached files/images inline at the first relevant point |
| Placeholders only | No | Insert `> **📊 Diagram placeholder:** …` blocks |
| No diagrams | No | Text only |

## Output-format question (asked in Step 4)

```
header: "output-format"
question: "How should the documentation be written?"
options:
  - "Single file — one .md file"
  - "Directory — folder with multiple .md files (e.g. epic with sub-tickets, multi-chapter guide)"
allowFreeformInput: true
```

## Mid-review question (asked in Step 5)

```
header: "structure-review"
question: "Review the proposed structure. What would you like to do?"
options:
  - "Approve — proceed to fill in content"
  - "Add a section"
  - "Remove a section"
  - "Reorder sections"
  - "Edit a section heading or summary"
  - "Add more context — attach files, links, or details I missed earlier"
allowFreeformInput: true
```

If "Add more context", accept the input (use the Preflight hints) and revise the structure before re-presenting. Loop until approved.
