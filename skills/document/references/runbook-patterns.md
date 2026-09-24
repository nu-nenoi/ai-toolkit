# Runbook & Playbook Patterns

A **runbook** is a step-by-step procedure for one specific operational scenario (e.g. "Rotate database credentials"). A **playbook** is a broader strategic guide for handling a class of situations (e.g. "Incident response", "On-call rotation").

## Runbook structure

1. **Purpose** — one sentence: what scenario this runbook handles
2. **When to use** — symptoms or triggers
3. **Prerequisites** — access, tools, permissions required before starting
4. **Procedure** — numbered steps, each with the exact command or action
5. **Verification** — how to confirm the procedure worked
6. **Rollback** — how to undo if something goes wrong
7. **Escalation** — who to contact and when
8. **References** — related runbooks, dashboards, alerts

## Procedure steps

Each step should be self-contained and copy-pasteable:

```markdown
## Procedure

1. **Check current state**

   ​```bash
   {{command}}
   ​```

   Expected output: {{what success looks like}}

2. **Apply the change**

   ​```bash
   {{command}}
   ​```

3. **Verify**

   ​```bash
   {{command}}
   ​```
```

## Callouts

Use callouts for danger and shortcuts:

```markdown
> **⚠️ Warning:** This action is irreversible. Confirm with on-call before proceeding.

> **💡 Tip:** Skip step 3 if the resource was created in the last 24 hours.
```

## Playbook structure

A playbook is structurally similar but:
- Higher-level: principles and decision-making, not literal commands
- Includes a **Decision tree** or **Triage matrix** to route situations
- Links out to specific runbooks for the operational details

```markdown
## Triage Matrix

| Symptom | Severity | First action | Runbook |
|---------|----------|-------------|---------|
| Service returns 5xx > 5% | SEV-2 | Check upstream health | [runbook-upstream-check.md](./runbook-upstream-check.md) |
| Latency p99 > 2s | SEV-3 | Check DB load | [runbook-db-load.md](./runbook-db-load.md) |
```

## Mandatory rollback section

A runbook without a Rollback section is incomplete. Even if rollback is "no action required, the change is automatically reversed", state that explicitly.
