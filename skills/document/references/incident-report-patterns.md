# Incident Report (Post-Mortem) Patterns

An incident report (also called post-mortem) documents what happened during an incident, what caused it, and what actions will prevent or mitigate similar incidents in the future.

> **Important:** The tone must be **blameless**. Focus on systems and process, not individuals.

## Severity-driven depth

| Severity | Description | Report depth |
|----------|-------------|--------------|
| **SEV-1** | Full outage, critical data loss | Full report — every section in detail |
| **SEV-2** | Major degradation, partial outage | Full report |
| **SEV-3** | Minor degradation, limited impact | Standard report — may compress timeline |
| **SEV-4** | Internal-only or near-miss | Lightweight report |
| **SEV-5** | Cosmetic / no user impact | Optional report |

## Structure

1. **Summary** — 2–3 sentences: what, when, who was affected
2. **Status** — Active / Mitigated / Resolved + current owner
3. **Impact** — users / services / data affected with quantified scope
4. **Timeline** — chronological events in UTC
5. **Root Cause** — the underlying cause (not just the trigger)
6. **Contributing Factors** — conditions that made it worse or harder to detect
7. **Detection** — how the incident was discovered (alert / user report / chance)
8. **Response** — what the team did, in order
9. **Resolution** — what fixed it, with link to the change
10. **Action Items** — concrete follow-ups with owners and due dates
11. **Lessons Learned** — what went well, what didn't, what to change

## Metadata table

```markdown
| Field | Value |
|-------|-------|
| **Incident ID** | INC-{{number}} |
| **Severity** | SEV-{{1-5}} |
| **Status** | Active / Mitigated / Resolved |
| **Started** | YYYY-MM-DD HH:MM UTC |
| **Detected** | YYYY-MM-DD HH:MM UTC |
| **Mitigated** | YYYY-MM-DD HH:MM UTC |
| **Resolved** | YYYY-MM-DD HH:MM UTC |
| **Duration** | {{hh:mm}} |
| **Reporter** | {{name}} |
| **Owner** | {{name}} |
```

## Timeline

Use a table sorted chronologically:

```markdown
| Time (UTC) | Event |
|------------|-------|
| 10:23 | First alert: latency p99 above threshold |
| 10:25 | On-call engineer paged |
| 10:31 | Root cause hypothesis: db connection pool exhausted |
| 10:42 | Mitigation deployed: pool size increased |
| 10:55 | Latency back to baseline |
| 11:30 | Incident declared resolved |
```

## Action items

Every action item must have an owner, a JIRA-equivalent link (or `{{TODO: link}}`), and a due date:

```markdown
- [ ] Add alert for connection-pool saturation — Owner: {{name}}, Ticket: [PROJ-456](url), Due: 2026-05-01
- [ ] Document pool-size tuning in runbook — Owner: {{name}}, Ticket: [PROJ-457](url), Due: 2026-05-08
```

## Blameless principle

- **Do** describe systems, processes, gaps in monitoring
- **Don't** name individuals as causing the problem
- **Do** name individuals as having contributed to detection / response (credit good work)
- **Don't** speculate on motivation or competence
