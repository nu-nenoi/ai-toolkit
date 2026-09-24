# Karpathy LLM Wiki Scaffolding Prompt

Use this meta-prompt in any agent harness (Claude Code, Cursor, Antigravity, Codex, etc.) to set up an instruction-driven Karpathy Wiki knowledge architecture without external MCP servers.

---

```markdown
Set up the Karpathy LLM Wiki architecture in this repository using pure instructions, modular procedures, and a portable autonomous trigger. Do not use external MCP servers.

Implement the following structure:

### 1. Directories
- `/raw/`: Landing directory for all raw documentation, web scrapes, transcripts, articles, and unrefined source materials (append-only). Add a `.gitkeep`.
- `/wiki/`: Curated, synthesized, interconnected knowledge base. Create an initial `/wiki/index.md` containing sections for Concepts, Entities, Topics, and Logs.

### 2. Universal Agent Instructions (`AGENTS.md`)
Create an `AGENTS.md` file at the repository root that defines:
- **Intake Rule**: All new external documentation, reference material, logs, and drafts must be saved directly to `/raw/` by default.
- **Consultation Rule**: Check `/wiki/` before answering project-specific or domain questions to leverage existing curated knowledge.
- **Ingestion Rule**: When new files arrive in `/raw/`, synthesize them into `/wiki/` rather than leaving knowledge raw.
- **Format Rule**: Notes in `/wiki/` must be atomic Markdown files with standard Markdown links (`[Concept](./concept.md)`) and YAML frontmatter (`title`, `tags`, `last_updated`, `sources`).
- **Autonomous Cadence**: The agent must run the wiki lint check at the start of any new task/session and every 10–20 interactions.

### 3. Procedures / Workflows
Create two procedural instruction files under `.agent/workflows/` (or `.agent/skills/`):

1. `wiki-ingest.md`:
   - Step 1: Scan `/raw/` for unprocessed or recently modified files.
   - Step 2: Extract key concepts, relationships, and facts.
   - Step 3: Create or update atomic notes in `/wiki/`, adding cross-links to related notes.
   - Step 4: Register new notes in `/wiki/index.md` and log the ingestion in `/wiki/_log.md`.

2. `wiki-lint.md`:
   - Step 1: Check `/wiki/` for broken Markdown links, missing targets, and orphaned pages.
   - Step 2: Detect duplicate notes, overlapping concepts, or conflicting information; merge or update as needed.
   - Step 3: Ensure all pages are properly categorized in `/wiki/index.md`.
   - Step 4: Output a brief summary of corrections made.

### 4. Autonomous Lint Trigger
Create a standalone POSIX shell script at `scripts/wiki-lint-trigger.sh`:
- Maintains a local turn/session counter in `.wiki_counter` (ignored in `.gitignore`).
- Increments the counter each time it is executed.
- If the counter is `1` (new session) or a multiple of `15`, it prints:
  `"[WIKI MAINTENANCE DUE]: Check /wiki/ for broken links, duplicates, or pending /raw/ files, and run wiki-lint."`
- Otherwise, it exits silently with status code `0`.
- Make the script executable (`chmod +x`).

Ensure all files are completely self-contained with no external dependencies.
```
