# Release Notes Patterns

Release notes summarise what changed in a release, who is affected, and what action (if any) the reader needs to take.

## Audience-driven structure

The audience determines tone and section depth:

| Audience | Tone | Emphasises |
|----------|------|-----------|
| **Developers** | Technical, precise | Breaking changes, deprecations, migration steps, API diffs |
| **End users** | Plain language | New features, fixes, known issues |

## Developer release notes

```markdown
# Release v{{X.Y.Z}} — {{YYYY-MM-DD}}

## Highlights
{{1–3 bullet points}}

## Breaking Changes
- **`functionName`** — signature changed. Migrate via: `{{example}}`

## Deprecations
- **`oldThing`** — will be removed in vX.Y. Use `newThing` instead.

## Features
- {{feature with PR / issue link}}

## Fixes
- {{fix with PR / issue link}}

## Migration Guide
Step-by-step instructions when breaking changes are present.
```

## End-user release notes

```markdown
# What's New — {{Month YYYY}}

## New
- {{feature in plain language}}

## Improved
- {{improvement}}

## Fixed
- {{user-visible fix}}

## Known Issues
- {{issue + workaround}}
```

## Versioning

Use [Semantic Versioning](https://semver.org) when applicable:
- **MAJOR** — breaking changes
- **MINOR** — new features, backwards compatible
- **PATCH** — fixes, backwards compatible

Always include the version number and release date in the H1 or as a metadata table.

## Linking

- Link each item to the PR / issue / changelog entry that introduced it
- For breaking changes, link to the migration guide section
- Group related items together — don't interleave breaking changes with cosmetic fixes
