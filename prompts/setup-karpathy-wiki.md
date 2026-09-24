# Karpathy LLM Wiki Scaffolding Prompt

Use this meta-prompt in any agent harness (Claude Code, Cursor, Antigravity, Codex, etc.) to set up an instruction-driven Karpathy Wiki knowledge architecture without external MCP servers.

---

```markdown
Set up the Karpathy LLM Wiki architecture in this repository using pure instructions, modular procedures, and a portable autonomous trigger. Do not use external MCP servers.

---

### Step 0 — Configuration (ask before doing anything else)

Ask the following questions and wait for all answers before proceeding:

[QUESTION 1] Which AI agent harness or tool are you using in this repository?
Options:
  - Claude Code (Anthropic) → will write rules to `CLAUDE.md`
  - Cursor → will write rules to `.cursor/rules/wiki.mdc`
  - GitHub Copilot → will write rules to `.github/copilot-instructions.md`
  - Google Antigravity / Gemini → will write rules to `GEMINI.md`
  - Windsurf → will write rules to `.windsurfrules`
  - Multiple / unsure → will write to `AGENTS.md` (universal fallback)
  - Other (specify) → ask for the target file path

[QUESTION 2] Should the Karpathy Wiki functionality be enabled immediately?
Options:
  - Yes — create `.wiki_enabled` sentinel file (wiki is active)
  - No — skip creating `.wiki_enabled` (wiki is inactive; can be enabled later by creating this file)

Use the answers from Step 0 to determine the instruction file path in Step 2 and whether to create `.wiki_enabled` in Step 1.

---

### 1. Directories and Sentinel File

- `/raw/`: Landing directory for all raw documentation, web scrapes, transcripts, articles, and unrefined source materials (append-only). Add a `.gitkeep`.
- `/wiki/`: Curated, synthesized, interconnected knowledge base. Create an initial `/wiki/index.md` containing sections for Concepts, Entities, Topics, and Logs.
- `.wiki_enabled`: Empty sentinel file at repository root. The trigger script checks for its existence — delete it to disable all wiki automation, recreate it to re-enable. Create this file only if the user answered Yes to Question 2.
- Add `.wiki_counter` and `.wiki_enabled` to `.gitignore`.

### 2. Agent Instructions

Using the instruction file determined in Step 0, append a dedicated `## Knowledge Base & Wiki Rules` section (preserve all existing content in the file):

- **Intake Rule**: All new external documentation, reference material, logs, and drafts must be saved directly to `/raw/` by default.
- **Consultation Rule**: Check `/wiki/` before answering project-specific or domain questions to leverage existing curated knowledge.
- **Ingestion Rule**: When new files arrive in `/raw/`, synthesize them into `/wiki/` rather than leaving knowledge raw.
- **Format Rule**: Notes in `/wiki/` must be atomic Markdown files with standard Markdown links (`[Concept](./concept.md)`) and YAML frontmatter (`title`, `tags`, `last_updated`, `sources`).
- **Autonomous Cadence**: After any session where at least one repository file was created, edited, or deleted, run `scripts/wiki-lint-trigger.sh`. If it outputs `[WIKI MAINTENANCE DUE]`, immediately run the `wiki-lint.md` workflow. Do not run the trigger on read-only or conversational interactions where no files were modified.

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
- If `.wiki_enabled` does not exist at repository root, exit silently with code `0` (wiki is disabled).
- Maintains a local session counter in `.wiki_counter` (gitignored).
- Increments the counter each time it is executed.
- If the counter is `1` (first run after enable) or a multiple of `15`, prints:
  `[WIKI MAINTENANCE DUE]: Check /wiki/ for broken links, duplicates, or pending /raw/ files, and run wiki-lint.`
- Otherwise, exits silently with code `0`.
- Make the script executable (`chmod +x`).

To disable wiki automation at any time: `rm .wiki_enabled`
To re-enable: `touch .wiki_enabled` (counter resets on next run)

Ensure all files are completely self-contained with no external dependencies.
```
