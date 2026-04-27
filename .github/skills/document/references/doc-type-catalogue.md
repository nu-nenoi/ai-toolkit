# Document Type Catalogue

The full list of document types this skill supports, each with the description shown in `vscode_askQuestions`, branching subtypes, and TOC eligibility.

## Types

| Type | Description | Subtypes | TOC asked? |
|------|-------------|----------|-----------|
| **README** | Entry-point doc for a project, package, or repo — what it does, how to run it | Application / Library / Monorepo root / Sub-package | Yes |
| **ADR** | Architecture Decision Record — captures one decision: context, options, choice, consequences | Status: Proposed / Accepted / Superseded | Yes |
| **Design doc / Tech spec** | Proposes an implementation before building — problem, design, alternatives, plan | — | Yes |
| **RFC** | Proposal opened for team feedback before a decision is taken | — | Yes |
| **Runbook** | Step-by-step operational procedure for a known scenario (deploy, rotate keys, restart) | — | Yes |
| **Playbook** | Strategic guide for handling a class of situations (incident response, on-call) | — | Yes |
| **Release notes** | What changed in a release, who is affected, migration steps | Audience: Developers / End users (also ask for version) | No |
| **Incident report** | What broke, timeline, root cause, action items | Severity (SEV-1…SEV-5) + Status (active / resolved) | Yes |
| **API documentation** | Endpoint or library reference — params, responses, examples | REST / GraphQL / Library or SDK / CLI | Yes |
| **Troubleshooting guide** | Common problems → diagnosis → fix | — | Yes |
| **FAQ** | Question-and-answer list for recurring queries | — | No |
| **Onboarding guide** | New-team-member orientation: setup, key concepts, who to ask | — | Yes |
| **Contributing guide** | How to contribute code/docs, branching, review process | — | Yes |
| **Changelog** | Chronological version history (Keep-a-Changelog format) | — | No |
| **User guide** | End-user-facing how-to documentation | — | Yes |
| **Meeting notes** | Discussion record — attendees, agenda, decisions, actions | Standup / Planning / Review / Retrospective / Ad-hoc | No |
| **Confluence page** | Generic knowledge-base page (when none of the above fits) | — | Yes |
| **JIRA ticket** | Tracker entry | Story / Bug / Task / Epic / Spike / Subtask | No |
| **Other** | Free text — agent picks the closest pattern | — | Yes |

## TOC eligibility rule

The TOC question (Pass 2 Q7) is **skipped** for these short-form types:

- JIRA ticket
- Meeting notes
- FAQ
- Changelog
- Release notes

For all other types, ask. Even when the user opts in, only render a TOC if the final document has at least 4 H2 sections — otherwise it adds noise without value.

## Subtype-specific guidance

**README subtype branching** — see [readme-patterns.md](./readme-patterns.md). Application/Library/Monorepo-root/Sub-package each emphasise different sections.

**JIRA subtype branching** — see [jira-patterns.md](./jira-patterns.md). Story/Bug/Epic/Spike/Subtask have distinct section sets.

**Meeting-notes subtype branching** — see [short-doc-patterns.md](./short-doc-patterns.md). Retrospective uses a "Went well / Didn't go well / Actions" structure; standup uses "Yesterday / Today / Blockers"; planning/review/ad-hoc use the standard agenda structure.

**ADR status** — captured in the front-matter table. "Superseded" should also link to the superseding ADR.

**Release notes audience** — Developers see breaking changes, deprecations, migration steps; end users see features, fixes, known issues in plain language.

**Incident report severity** — SEV-1 (full outage) through SEV-5 (cosmetic). Severity drives the depth of the timeline and root-cause sections.

**API doc subtype** — REST emphasises endpoints/methods/status codes; GraphQL emphasises schema/queries/mutations; Library/SDK emphasises imports/types/examples; CLI emphasises commands/flags/exit codes.
