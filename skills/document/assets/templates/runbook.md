<!-- Template: Runbook -->
<!-- Replace all {{PLACEHOLDER}} values. -->

# Runbook — {{Scenario name}}

| Field | Value |
|-------|-------|
| **Owner** | {{Team / person}} |
| **Last updated** | {{YYYY-MM-DD}} |
| **Severity** | {{When this applies}} |

## Purpose

{{One sentence: what scenario this runbook handles.}}

## When to Use

{{Symptoms or triggers that mean you should reach for this runbook.}}

- {{Symptom 1}}
- {{Symptom 2}}

## Prerequisites

- {{Required access / role}}
- {{Required tools (e.g. AWS CLI, kubectl)}}
- {{Any preflight checks}}

## Procedure

1. **{{Step 1 name}}**

   ```bash
   {{command}}
   ```

   Expected output: {{what success looks like}}

2. **{{Step 2 name}}**

   ```bash
   {{command}}
   ```

3. **{{Step 3 name}}**

   ```bash
   {{command}}
   ```

## Verification

{{How to confirm the procedure worked. Include exact commands and expected output.}}

```bash
{{verification command}}
```

## Rollback

{{How to undo the change. If rollback is automatic, state that explicitly.}}

```bash
{{rollback command}}
```

## Escalation

| Condition | Contact |
|-----------|---------|
| {{Procedure fails at step 2}} | {{on-call team / channel}} |
| {{Verification fails}} | {{on-call team / channel}} |

## References

- {{Related runbooks}}
- {{Dashboards}}
- {{Related alerts}}
