# Short Document Patterns

Patterns for the lightweight document types: FAQ, changelog, meeting notes, troubleshooting guide. These docs are short, scannable, and update-friendly.

## FAQ

```markdown
# Frequently Asked Questions — {{Topic}}

## {{Question 1?}}

{{Concise answer. Link out for depth.}}

## {{Question 2?}}

{{Answer.}}
```

Rules:
- Each question is an H2 (so it can anchor-link from elsewhere)
- Keep answers under 100 words; link to fuller docs for depth
- Group related questions with a `### Category` divider only if there are 10+ entries

## Changelog

Follow [Keep a Changelog](https://keepachangelog.com/) format:

```markdown
# Changelog

All notable changes to this project follow [Semantic Versioning](https://semver.org).

## [Unreleased]

### Added
- {{new feature}}

## [1.2.0] — 2026-04-25

### Added
- {{feature}}

### Changed
- {{change}}

### Fixed
- {{fix}}

### Removed
- {{removal}}

### Security
- {{security fix}}
```

Categories per release: **Added / Changed / Deprecated / Removed / Fixed / Security**. Omit empty categories.

## Meeting notes

The structure depends on the meeting subtype:

**Standup**

```markdown
# Standup — {{YYYY-MM-DD}}

| Person | Yesterday | Today | Blockers |
|--------|-----------|-------|----------|
| {{name}} | {{what}} | {{what}} | {{none / blocker}} |
```

**Planning / Review / Ad-hoc**

```markdown
# {{Meeting type}} — {{YYYY-MM-DD}}

**Attendees:** {{names}}
**Facilitator:** {{name}}
**Note-taker:** {{name}}

## Agenda
- {{item 1}}
- {{item 2}}

## Discussion

### {{Agenda item 1}}
{{notes}}

## Decisions
- {{decision 1}}
- {{decision 2}}

## Action Items
- [ ] {{action}} — Owner: {{name}}, Due: {{date}}
```

**Retrospective** — uses a different structure:

```markdown
# Retrospective — {{Sprint / Period}}

**Attendees:** {{names}}

## Went Well
- {{item}}

## Didn't Go Well
- {{item}}

## Actions
- [ ] {{action}} — Owner: {{name}}, Due: {{date}}
```

## Troubleshooting guide

Organise by symptom, not by root cause — readers arrive knowing what they see, not what's broken:

```markdown
## Symptom: {{What the user observes}}

**Likely cause:** {{cause}}

**Diagnose:**

​```bash
{{command to confirm}}
​```

**Fix:**

​```bash
{{fix command or steps}}
​```

**See also:** [related runbook](./runbook-x.md)
```

Order symptoms from most common to least common. For each, keep the diagnosis to a single command or check.
