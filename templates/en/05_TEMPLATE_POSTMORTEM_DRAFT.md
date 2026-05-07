# Template - Postmortem Draft

## When to use
After incident stabilization, to generate a human-reviewed RCA draft.

## Prompt
```text
Create a technical and objective postmortem draft.

Inputs:
- incident: <INCIDENT_ID>
- service/environment: <SERVICE_ENVIRONMENT>
- incident period: <START_END>
- observed impact: <IMPACT>
- event timeline: <TIMELINE>
- evidence (logs/metrics/traces): <EVIDENCE>
- changes around incident time: <CHANGES>
- applied mitigation: <MITIGATION>

Required format:
1) Executive summary (5 bullets)
2) Impact and scope
3) Incident timeline
4) Root-cause hypothesis (and confidence level)
5) Contributing factors
6) What worked well
7) What failed
8) Corrective and preventive actions (with priority)
9) Observability/data gaps

Rules:
- If root cause is not conclusive, state it explicitly.
- Do not invent evidence that was not provided.
- Include manual recommendations only.
```
