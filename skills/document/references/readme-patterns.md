# README Patterns

A README is the entry point for any developer encountering a project, package, or repo for the first time. The exact structure depends on the **subtype**:

| Subtype | Emphasises |
|---------|-----------|
| **Application** | Getting started, configuration, deployment |
| **Library** | Installation, API, code examples, usage |
| **Monorepo root** | Layout, getting started, links to sub-package READMEs |
| **Sub-package** | What it exports, how it fits into the parent monorepo |

## Common structure

In order (omit sections that don't apply):

1. **Title** (H1) with optional badge row
2. **One-liner** — what this does in a single sentence (use a blockquote)
3. **Table of Contents** — only if requested and the doc is long
4. **Overview** — 2–3 paragraphs explaining purpose and context
5. **Getting Started** — prerequisites, installation, running locally
6. **Usage** / **API** — most common patterns with code examples
7. **Configuration** — env vars, config files (use a table)
8. **Testing** — how to run tests
9. **Architecture** — brief overview, link out for detail
10. **Deployment** — how to deploy (applications only)
11. **Contributing** — link to a `CONTRIBUTING.md` if one exists
12. **Related Documentation** — links to sibling/parent docs

## Badges

Place badges on a single line right after the H1 title:

```markdown
# Project Name

![Build](https://img.shields.io/badge/build-passing-brightgreen)
![Coverage](https://img.shields.io/badge/coverage-95%25-brightgreen)
![License](https://img.shields.io/badge/license-MIT-blue)
```

Adapt to the actual CI/CD system and license in use. If unknown, leave a `{{TODO: badge URL}}` placeholder.

## Subtype-specific guidance

**Application READMEs** emphasise commands the user runs:

```markdown
## Getting Started

​```bash
{{install command}}
{{run command}}
​```
```

**Library READMEs** emphasise the import + minimal usage example:

```markdown
## Usage

​```typescript
import { someFunction } from 'my-library';

const result = someFunction({ option: 'value' });
​```
```

**Monorepo-root READMEs** emphasise layout and a per-package table:

```markdown
## Packages

| Package | Description |
|---------|-------------|
| [`packages/foo`](./packages/foo/README.md) | Brief description |
| [`packages/bar`](./packages/bar/README.md) | Brief description |
```

**Sub-package READMEs** stay short. Link to the parent README rather than duplicating setup steps.

## Configuration table

Use a table for env vars and config:

```markdown
| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `API_URL` | Yes | — | Backend API URL |
| `LOG_LEVEL` | No | `info` | Logging verbosity |
```

## Architecture section

Keep it brief — 2–3 sentences plus a link out:

```markdown
## Architecture

This service receives messages from the upstream queue and forwards them to downstream consumers.

​```
Upstream → This Service → Downstream
​```

For details, see [architecture docs](./docs/architecture.md).
```

If diagram placeholders were requested, reserve a slot here:

```markdown
> **📊 Diagram placeholder:** system architecture diagram
```
