# AI Toolkit

![License](https://img.shields.io/badge/license-MIT-blue)

> A shared collection of AI tools, skills, and best practices for engineering teams.

## Overview

This repository is a central resource for anything AI-related that engineering teams find useful — reusable agent skills, prompt patterns, workflow guides, and documented best practices. The goal is to make AI tooling consistent, discoverable, and easy to adopt across projects.

Content here is technology-agnostic where possible. Skills and guides are written to work with any project or codebase, so teams can pick up and use them without modification.

As the toolkit grows, this repo will expand to cover more AI use cases: code generation, testing automation, documentation, review workflows, and more.

## What's Inside

| Category | Resource | Description |
|----------|----------|-------------|
| **Skills** | [`document`](.github/skills/document/) | Agent skill for producing structured Markdown documentation of any kind — READMEs, ADRs, runbooks, JIRA tickets, incident reports, and more |

## Using the Skills

Skills are agent instruction files designed for use with [GitHub Copilot Chat](https://docs.github.com/en/copilot/using-github-copilot/asking-github-copilot-questions-in-your-ide) in VS Code. Each skill lives in `.github/skills/<name>/` and contains a `SKILL.md` that the agent reads to follow a specific workflow.

**To use a skill:**

1. Open GitHub Copilot Chat in VS Code (`Ctrl+Shift+I` / `Cmd+Shift+I`).
2. Attach the skill's `SKILL.md` file to your chat message, or reference it by path.
3. Describe your task — the skill's workflow will guide the agent through the right steps.

Each skill folder also contains reference files and templates that the agent loads automatically during execution. You do not need to attach these manually.

<details>
<summary>Example: using the <code>document</code> skill</summary>

Attach `.github/skills/document/SKILL.md` to a Copilot Chat message, then say:

```
Write a runbook for rotating the database credentials in the payments service.
```

The agent will ask a short set of planning questions, draft a structure for your review, and then generate the document.

</details>

## Related Documentation

- [GitHub Copilot documentation](https://docs.github.com/en/copilot)
- [VS Code Copilot Chat overview](https://code.visualstudio.com/docs/copilot/overview)
- [VS Code agent customisation](https://code.visualstudio.com/docs/copilot/copilot-customization)