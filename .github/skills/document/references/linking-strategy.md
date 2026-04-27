# Linking Strategy

Every document is more useful when it links to related material. Follow this strategy.

## Discovery

Before generating content, search the current workspace for relevant material:

1. **Workspace docs** — `README.md`, `CONTRIBUTING.md`, `ARCHITECTURE.md`, `docs/**/*.md`, `**/adr-*.md`, `**/AGENTS.md`
2. **Source code** — when documenting a service or library, locate its entry-point file (e.g. `src/main.ts`, `index.ts`) so you can mention real symbols accurately
3. **Configuration** — `package.json`, `tsconfig.json`, `pyproject.toml`, etc., for accurate command names

## Link types and when to use each

| Link type | When | Example |
|-----------|------|---------|
| **Relative workspace link** | Pointing to a file in the same repo | `[CDK overview](../../cdk/README.md)` |
| **Anchor link** | Pointing to a section in the same doc | `[See architecture](#architecture)` |
| **External standard / spec** | Citing an authoritative spec | `[RFC 7807](https://www.rfc-editor.org/rfc/rfc7807)` |
| **Vendor / library docs** | Referring to a tool the doc relies on | `[AWS Lambda Powertools](https://docs.powertools.aws.dev/)` |
| **Issue tracker / PR** | Tying a doc to a specific change | `[PROJ-123](https://example.atlassian.net/browse/PROJ-123)` |
| **Internal knowledge base** | Cross-linking related team docs | (only when the user provided URLs in their topic context) |

## Placement rules

- **Inline on first mention** — link a concept the first time it appears, not every time
- **Avoid trailing link dumps** — only add a "See also" or "Related Resources" section for links that didn't fit naturally inline
- **Never invent URLs** — if you don't know the exact URL, mark it as `{{TODO: link to X}}` and let the user fill in
- **Prefer relative paths** for workspace docs — they survive repo moves and renames

## Don't force it

If a topic genuinely doesn't connect to anything in the workspace, don't pad the doc with irrelevant links. Quality over quantity.
