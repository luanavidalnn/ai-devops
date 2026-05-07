# Template - Incident Handoff

## When to use
During shift change, escalation, or incident ownership transition.

## Prompt
```text
Prepare a concise incident handoff for the next owner.

Input data:
- incident: <INCIDENT_ID>
- service/environment: <SERVICE_ENVIRONMENT>
- current status: <STATUS>
- impact: <IMPACT>
- timeline summary: <TIMELINE>
- key evidence: <EVIDENCE>
- actions already executed: <EXECUTED_ACTIONS>
- pending items: <PENDING_ITEMS>

Required format:
1) Current incident state
2) What has already been validated
3) What is still unvalidated
4) Immediate risks
5) Next 3 recommended manual actions
6) Suggested owner per action
7) Next update time

Rules:
- Use objective bullet points.
- Do not include sensitive data.
- Do not propose autonomous infrastructure actions.
```
