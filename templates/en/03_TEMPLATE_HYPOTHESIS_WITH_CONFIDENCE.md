# Template - Hypothesis Generation with Confidence

## When to use
After initial triage, to prioritize investigation paths.

## Prompt
```text
Act as a DevOps analyst for root-cause hypothesis generation.

Incident context:
<INCIDENT_CONTEXT>

Collected evidence:
<EVIDENCE>

Task:
Generate root-cause hypotheses, ordered by probability.

Required response format:
- Hypothesis 1:
  - description
  - confidence (high/medium/low)
  - supporting evidence
  - contradicting evidence
  - manual validation check
- Hypothesis 2: (same format)
- Hypothesis 3: (same format)
- Recommended investigation order
- Escalation conditions

Rules:
- Make assumptions explicit.
- Do not propose autonomous production actions.
- If data is insufficient, list exactly which data is missing.
```
