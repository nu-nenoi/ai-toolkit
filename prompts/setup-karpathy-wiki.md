# Karpathy LLM Wiki Scaffolding Prompt

Universal meta-prompt for setting up a Karpathy LLM Wiki knowledge architecture. Works with any AI agent harness. No external MCP servers, vector databases, or embedding pipelines required — just markdown files with structured frontmatter.

---

```markdown
Set up a Karpathy LLM Wiki in this repository: a self-maintaining knowledge base where raw source material is ingested into interlinked atomic markdown files that any AI agent can navigate by reading indexes and following typed frontmatter relations.

Do not use external MCP servers, vector databases, or embedding pipelines.
Do not hard-code any specific AI tool or harness into the wiki structure itself.

---

### Step 0 — Configuration (ask before creating any files)

Ask ALL of the following questions and wait for answers:

[Q1] Which AI agent instruction file should the wiki rules be written to?
  - `AGENTS.md` (universal, works across most harnesses)
  - `CLAUDE.md` (Claude Code / Anthropic)
  - `.cursor/rules/wiki.mdc` (Cursor)
  - `.github/copilot-instructions.md` (GitHub Copilot)
  - `GEMINI.md` (Google Gemini / Antigravity)
  - `.windsurfrules` (Windsurf)
  - Other — specify path

  Append a `## Karpathy Wiki Rules` section to that file. Preserve all existing content.

[Q2] What is this wiki for?
  - Research / reading list — articles, papers, PDFs on a topic
  - Personal second brain — meetings, notes, business context, personal projects
  - Content archive — transcripts, podcast notes, newsletters
  - Codebase knowledge — architecture decisions, runbooks, team conventions
  - Other (describe briefly)

[Q3] How should the wiki be organized?
  - Flat — all pages at the top level of `/wiki/` (simpler, good default)
  - Structured — subfolders by category, chosen based on Q2 answer
  - Agent decides — infer structure from the first batch of ingested content

[Q4] Enable wiki automation now?
  - Yes — create `.wiki_enabled` sentinel file
  - No — skip for now (`touch .wiki_enabled` to enable later)

---

### 1. Directories and Files

**`/raw/`** — Append-only intake directory. All original source material goes here unmodified. Add `.gitkeep`.

**`/wiki/`** — Curated knowledge base of atomic markdown pages. Structure per Q3:
  - Flat: all pages directly in `/wiki/`
  - Structured: subfolders matching the project type from Q2:
    - Research → `concepts/`, `people/`, `organizations/`, `sources/`, `analysis/`
    - Second brain → `projects/`, `people/`, `decisions/`, `logs/`
    - Content archive → `sources/`, `people/`, `tools/`, `concepts/`
    - Codebase → `architecture/`, `decisions/`, `runbooks/`, `people/`

**`/wiki/index.md`** — Master navigation index. Categorized links to every wiki page. Auto-maintained after every ingest and lint run.

**`/wiki/_log.md`** — Append-only operation log. Every ingest and lint run appends a timestamped entry.

**`/wiki/hot.md`** *(create only for second brain or codebase wiki)* — Rolling ~500-word cache of the most recently relevant context. Lets an agent orient without reading the full wiki.

**`.wiki_enabled`** — Sentinel file. Presence = wiki active. Absence = all automation skipped silently. Create only if Q4 is Yes. Add to `.gitignore`.

**`.wiki_counter`** — Session counter used by the lint trigger script. Add to `.gitignore`.

---

### 2. Wiki Page Format

Every wiki page is a single atomic markdown file covering one concept, entity, source, or decision.

**Required YAML frontmatter:**

```yaml
---
title: ""
tags: []
last_updated: YYYY-MM-DD
# Typed relation fields — paths relative to /wiki/
sources: []          # /raw/ files this page was derived from
related: []          # thematically related wiki pages
extends: []          # pages this one builds upon or specialises
contradicts: []      # pages with conflicting information
mentioned_in: []     # pages that link to this one (maintained by lint)
---
```

**Body:** concise summary, key facts or insights, and inline standard Markdown links (`[Label](./path.md)`) where contextually useful in prose.

The relation graph lives in frontmatter, not in prose links. Agents traverse the graph by reading frontmatter fields, not by scanning body text.

---

### 3. Agent Instructions

In the file chosen in Q1, append:

```markdown
## Karpathy Wiki Rules

- **Intake**: All source material (articles, transcripts, docs, notes) goes to `/raw/` unmodified. Never write directly to `/wiki/` without ingesting.
- **Orientation**: Before answering domain questions, read `/wiki/index.md` to find relevant pages, then read only those pages. If `hot.md` exists, read it first as a quick-orient step.
- **Ingestion**: When files appear in `/raw/`, run the `wiki-ingest` workflow. One source document typically produces many atomic pages — do not collapse a source into a single file.
- **Cadence**: After any session where repository files were created, edited, or deleted, run `scripts/wiki-lint-trigger.sh`. If it outputs `[WIKI MAINTENANCE DUE]`, run the `wiki-lint` workflow before stopping.
- **No-op sessions**: Do not run the trigger after read-only or purely conversational sessions where no files changed.
```

---

### 4. Workflows

Create workflow instruction files at `.agent/workflows/` (or `.agent/skills/` depending on the harness):

**`wiki-ingest.md`**:
1. Scan `/raw/` for files not yet recorded in `/wiki/_log.md`.
2. For each source, read it fully. If scope or depth is unclear, ask one round of clarifying questions before proceeding.
3. Decompose into atomic wiki pages — one per distinct concept, person, organization, event, or theme. A single article commonly produces 5–25 pages.
4. For each page, write the required frontmatter (including `sources`, `related`, `extends`, `contradicts`). Leave `mentioned_in` empty — lint maintains it.
5. Write a concise body with inline links where contextually helpful.
6. Update `/wiki/index.md` with links to all new pages.
7. Update `/wiki/hot.md` if it exists.
8. Append a timestamped entry to `/wiki/_log.md`.

**`wiki-lint.md`**:
1. For every page, verify that all paths in frontmatter relation fields (`related`, `extends`, `contradicts`, `sources`) resolve to existing files. Fix or flag broken paths.
2. Recompute `mentioned_in` for every page by scanning all other pages' relation fields and inline links. Update the field.
3. Find orphaned pages (not reachable from `index.md` or any `mentioned_in` field). Add them to the index.
4. Detect duplicate or heavily overlapping pages. Consolidate and update relations.
5. Check for factual inconsistencies between pages in `contradicts` relations or covering the same topic.
6. Identify knowledge gaps: concepts referenced in relation fields but lacking their own page. Create stub pages or flag for future ingest.
7. Suggest source candidates from `/raw/` or external searches to fill gaps.
8. Rebuild `/wiki/index.md` to reflect the current set of pages.
9. Update `/wiki/hot.md` if it exists.
10. Append a lint summary to `/wiki/_log.md`.

---

### 5. Autonomous Lint Trigger

Create `scripts/wiki-lint-trigger.sh` (POSIX shell):
- Exit silently with code `0` if `.wiki_enabled` does not exist (wiki is disabled).
- Read and increment a counter stored in `.wiki_counter`.
- If counter is `1` or a multiple of `15`, print:
  `[WIKI MAINTENANCE DUE]: Pending /raw/ files or wiki health checks detected. Run wiki-lint.`
- Otherwise exit silently with code `0`.
- Make executable: `chmod +x scripts/wiki-lint-trigger.sh`.

To disable: `rm .wiki_enabled` — To re-enable: `touch .wiki_enabled`

---

All files must be self-contained with no external dependencies.
```
