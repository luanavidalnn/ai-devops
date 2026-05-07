# Template - Weekly Risk Briefing

## When to use
Weekly predictability ritual per service (shared risk layer).

## Prompt
```text
Generate a weekly operational risk briefing for the service below.

Context:
- service: <SERVICE>
- environment: <ENVIRONMENT>
- weekly window: <PERIOD>
- incident signals (Datadog): <OBSERVABILITY_SIGNALS>
- delivery signals (CI/CD): <PIPELINE_RELEASE_SIGNALS>
- relevant changes during week: <CHANGES>

Required format:
1) Weekly risk level (low/medium/high)
2) Main risk drivers
3) Detected early warning patterns
4) Recommended preventive manual actions
5) Analysis confidence
6) Data gaps
7) Items to monitor next week

Rules:
- Advisory output only; no autonomous action.
- Cite evidence for each risk driver.
- If confidence is low, explicitly state why.
```
