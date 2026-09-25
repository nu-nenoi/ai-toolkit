# Karpathy LLM Wiki Scaffolding Prompt

Use this meta-prompt in any agent harness (Claude Code, Cursor, Antigravity, Codex, etc.) to set up an instruction-driven Karpathy LLM Wiki knowledge architecture without external MCP servers or vector databases — just markdown files.

---

```markdown
Set up the Karpathy LLM Wiki architecture in this repository using pure instructions, modular procedures, and a portable autonomous trigger. Do not use external MCP servers, vector databases, or embedding pipelines — the entire system is markdown files navigated by index links.

---

### Step 0 — Configuration (ask before doing anything else)

Ask ALL of the following questions and wait for answers before creating any files:

[QUESTION 1] Which AI agent harness or tool are you using in this repository?
Options:
  - Claude Code (Anthropic) → will write rules to `CLAUDE.md`
  - Cursor → will write rules to `.cursor/rules/wiki.mdc`
  - GitHub Copilot → will write rules to `.github/copilot-instructions.md`
  - Google Antigravity / Gemini → will write rules to `GEMINI.md`
  - Windsurf → will write rules to `.windsurfrules`
  - Multiple / unsure → will write to `AGENTS.md` (universal fallback)
  - Other (specify) → ask for the target file path

[QUESTION 2] What is this wiki for? This shapes how the wiki is organized and what categories the index uses.
Options:
  - Research / reading list — articles, papers, PDFs on a topic
  - Personal second brain — meetings, notes, business context, personal projects
  - Content archive — video transcripts, podcast notes, newsletters
  - Codebase knowledge — architecture decisions, runbooks, team conventions
  - Other (describe briefly)

[QUESTION 3] How should the wiki be organized?
Options:
  - Flat — all wiki pages at the top level of `/wiki/`, no subfolders (simpler, Karpathy's preferred default)
  - Structured — subfolders by category (e.g. `concepts/`, `people/`, `sources/`, `analysis/`) — better for large archives
  - Let the agent decide — choose structure based on the content of the first ingest

[QUESTION 4] Should the Karpathy Wiki functionality be enabled immediately?
Options:
  - Yes — create `.wiki_enabled` sentinel file (wiki is active)
  - No — skip creating `.wiki_enabled` (wiki is inactive; run `touch .wiki_enabled` to enable later)

Use all answers from Step 0 to configure Steps 1–4 below.

---

### 1. Directories and Sentinel File

- `/raw/`: Landing directory for all raw source material — articles, transcripts, PDFs, meeting notes, dumps (append-only intake). Add a `.gitkeep`.
- `/wiki/`: Curated, synthesized, interlinked knowledge base. Structure based on Q3 answer:
  - Flat: all `.md` pages directly in `/wiki/`.
  - Structured: create subfolders appropriate to the project type from Q2 (e.g. for research: `concepts/`, `people/`, `organizations/`, `sources/`, `analysis/`; for second brain: `projects/`, `people/`, `decisions/`, `logs/`).
- `/wiki/index.md`: Master index with categorized links to every wiki page. Auto-maintained. Categories should match the project type from Q2.
- `/wiki/_log.md`: Append-only operation log — each ingest and lint run appends a timestamped entry.
- `/wiki/hot.md` *(optional, recommended for second brain or assistant use cases)*: A ~500-word rolling cache of the most recently ingested or most queried context. Allows an agent to orient itself quickly without reading the full wiki. Create this if Q2 is "Personal second brain" or "Codebase knowledge".
- `.wiki_enabled`: Empty sentinel file at repository root. The trigger script checks for its existence — `rm .wiki_enabled` to disable, `touch .wiki_enabled` to re-enable. Create only if Q4 is Yes.
- Add `.wiki_counter` and `.wiki_enabled` to `.gitignore`.

### 2. Agent Instructions

Using the instruction file from Q1, append a dedicated `## Karpathy Wiki Rules` section (preserve all existing content):

- **Intake Rule**: All raw source material — articles, transcripts, docs, notes — goes directly to `/raw/`. Never synthesize directly into `/wiki/` without going through ingest.
- **Consultation Rule**: Before answering domain-specific questions, read `/wiki/index.md` first to locate relevant pages, then read only those pages. Do not read the full wiki unless necessary.
- **Hot Cache Rule** *(if hot.md was created)*: Read `/wiki/hot.md` first as a quick-orient step before deciding whether deeper wiki access is needed.
- **Ingestion Rule**: When files appear in `/raw/`, run the `wiki-ingest.md` workflow. A single source document may produce many atomic wiki pages — do not collapse everything into one file.
- **Format Rule**: Every wiki page must be an atomic Markdown file covering one concept, person, organization, source, or decision. Use standard Markdown links (`[Topic](./topic.md)`) for cross-links. Include YAML frontmatter: `title`, `tags`, `last_updated`, `sources`.
- **Index Rule**: After every ingest, update `/wiki/index.md` with links to any new pages. The index is the navigation hub — keep it current.
- **Autonomous Cadence**: After any session where at least one repository file was created, edited, or deleted, run `scripts/wiki-lint-trigger.sh`. If it prints `[WIKI MAINTENANCE DUE]`, immediately run the `wiki-lint.md` workflow. Do not run the trigger on read-only or purely conversational sessions.

### 3. Procedures / Workflows

Create procedural instruction files under `.agent/workflows/` (or `.agent/skills/`):

**`wiki-ingest.md`**:
- Step 1: Scan `/raw/` for new or recently modified files not yet recorded in `/wiki/_log.md`.
- Step 2: For each source, read it fully and ask clarifying questions if scope or focus is unclear (e.g. "What should be emphasized? How granular?").
- Step 3: Decompose the source into multiple atomic wiki pages — one per distinct concept, person, organization, event, or theme. A single article may produce 5–25 pages. Do not create one monolithic file per source.
- Step 4: For each page, write YAML frontmatter, a concise summary, key facts or insights, and cross-links to related wiki pages.
- Step 5: Update `/wiki/index.md` with links to all new pages under the appropriate categories.
- Step 6: Update `/wiki/hot.md` (if it exists) with a brief summary of what was just ingested.
- Step 7: Append a timestamped entry to `/wiki/_log.md` listing the source and pages created.

**`wiki-lint.md`**:
- Step 1: Check all `/wiki/` pages for broken Markdown links and missing link targets. Fix or flag each.
- Step 2: Identify orphaned pages (not linked from index or any other page) and add them to the index.
- Step 3: Detect conceptual duplicates or heavily overlapping pages and consolidate where appropriate.
- Step 4: Check for factual inconsistencies between pages covering the same topic. Resolve or flag them.
- Step 5: Identify knowledge gaps — topics referenced by multiple pages but lacking their own wiki entry. Create stub pages or flag for future ingest.
- Step 6: Suggest new article or source candidates that would fill identified gaps (web search optional if available).
- Step 7: Ensure `/wiki/index.md` is complete and all categories reflect the current set of pages.
- Step 8: Update `/wiki/hot.md` (if it exists) to reflect the current state of the wiki.
- Step 9: Append a timestamped lint summary to `/wiki/_log.md`.

### 4. Autonomous Lint Trigger

Create a POSIX shell script at `scripts/wiki-lint-trigger.sh`:
- If `.wiki_enabled` does not exist at repository root, exit silently with code `0` (wiki is disabled — no output).
- Maintain a session counter in `.wiki_counter` (gitignored). Increment on each execution.
- If the counter is `1` (first run) or a multiple of `15`, print exactly:
  `[WIKI MAINTENANCE DUE]: Check /wiki/ for broken links, duplicates, gaps, and pending /raw/ files, then run wiki-lint.`
- Otherwise, exit silently with code `0`.
- Make executable with `chmod +x`.

Ensure all files are completely self-contained with no external dependencies.
```
