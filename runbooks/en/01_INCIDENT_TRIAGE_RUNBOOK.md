# Incident Triage Runbook

## Trigger
Use this runbook when a production or staging incident is opened.

## Step-by-Step
1. Confirm incident scope, severity, and stakeholders.
2. Gather evidence: logs, metrics, traces, recent deploys.
3. Use approved prompt template for hypothesis generation.
4. Validate top hypotheses with direct checks.
5. Apply safe mitigation or escalate.
6. Communicate status and next update time.

## Decision Gates
- Is user impact ongoing?
- Is there enough evidence for action?
- Is rollback safer than forward fix?

## Escalation
Escalate to platform lead and service owner when impact is high or uncertainty remains.

## Closure
- Confirm service stability.
- Capture root cause summary.
- Open follow-up actions.

## Revision History
- 2026-05-07: Initial runbook template.
