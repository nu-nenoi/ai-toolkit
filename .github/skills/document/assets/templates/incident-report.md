<!-- Template: Incident Report (Post-Mortem) -->
<!-- Tone must be BLAMELESS. Replace all {{PLACEHOLDER}} values. -->

# Incident Report — {{Short title}}

| Field | Value |
|-------|-------|
| **Incident ID** | INC-{{number}} |
| **Severity** | SEV-{{1-5}} |
| **Status** | {{Active / Mitigated / Resolved}} |
| **Started** | {{YYYY-MM-DD HH:MM UTC}} |
| **Detected** | {{YYYY-MM-DD HH:MM UTC}} |
| **Mitigated** | {{YYYY-MM-DD HH:MM UTC}} |
| **Resolved** | {{YYYY-MM-DD HH:MM UTC}} |
| **Duration** | {{hh:mm}} |
| **Reporter** | {{name}} |
| **Owner** | {{name}} |

## Summary

{{2–3 sentences: what happened, when, who was affected.}}

## Impact

- **Users affected:** {{number / percentage / segment}}
- **Services affected:** {{list}}
- **Data affected:** {{none / loss / corruption details}}
- **Revenue / SLA impact:** {{quantified if known}}

## Timeline

| Time (UTC) | Event |
|------------|-------|
| {{HH:MM}} | {{event}} |
| {{HH:MM}} | {{event}} |
| {{HH:MM}} | {{event}} |

## Root Cause

{{The underlying cause, not just the trigger. Include the chain of conditions that allowed the incident to happen.}}

## Contributing Factors

- {{Factor that made it worse or harder to detect}}
- {{Factor}}

## Detection

{{How was the incident discovered? Alert / user report / chance? How long after onset?}}

## Response

{{What did the team do, in order? Note what worked and what slowed things down.}}

## Resolution

{{What fixed the incident? Link to the change ([PR / commit](url)).}}

## Action Items

- [ ] {{action}} — Owner: {{name}}, Ticket: [{{TICKET-ID}}](url), Due: {{YYYY-MM-DD}}
- [ ] {{action}} — Owner: {{name}}, Ticket: [{{TICKET-ID}}](url), Due: {{YYYY-MM-DD}}

## Lessons Learned

**What went well**
- {{thing}}

**What didn't go well**
- {{thing}}

**What to change**
- {{change}}
