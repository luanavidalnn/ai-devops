# Prompt Template Library (Approved)

## Purpose
Provide standardized templates for consistent Incident and CI/CD analysis.

## Mandatory guardrails (all prompts)
- Assistive AI only. Do not execute infrastructure changes.
- Do not include secrets, tokens, credentials, or PII in prompts.
- Always declare confidence and data gaps.
- Always recommend next manual checks.

## Official library
- `02_TEMPLATE_INCIDENT_TRIAGE.md`
- `03_TEMPLATE_HYPOTHESIS_WITH_CONFIDENCE.md`
- `04_TEMPLATE_INCIDENT_HANDOFF.md`
- `05_TEMPLATE_POSTMORTEM_DRAFT.md`
- `06_TEMPLATE_CICD_PIPELINE_FAILURE.md`
- `07_TEMPLATE_DOCKER_IMAGE_ECR_TROUBLESHOOTING.md`
- `08_TEMPLATE_WEEKLY_RISK_BRIEFING.md`

## Minimum output contract
Every response must include, in this order:
1. Current situation
2. User/business impact
3. Evidence
4. Hypotheses/probable cause (with confidence)
5. Data gaps
6. Next manual checks
7. Recommendation risk

## Approval workflow
1. Draft by platform team.
2. Security and operations review.
3. Publish in official library.
4. Biweekly quality revalidation.

## Revision history
- 2026-05-07: Library completed with operational templates.
