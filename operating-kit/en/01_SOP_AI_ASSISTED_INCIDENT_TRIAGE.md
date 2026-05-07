# SOP - AI-Assisted Incident Triage

## Purpose
Define a repeatable process to triage incidents with AI support while keeping human accountability.

## Scope
- On-call operations
- Incident triage and first response
- Production and staging incidents

## Inputs
- Alert source and timestamp
- Service and environment impacted
- Initial logs, metrics, and traces
- Current deployment and recent changes

## Procedure
1. Confirm incident severity and business impact.
2. Collect evidence from approved observability sources.
3. Use approved prompt template to generate hypotheses.
4. Validate hypotheses against real evidence.
5. Decide next action and assign owner.
6. Record decisions and rationale in incident timeline.

## Human Review Checklist
- Evidence is explicit and linked.
- Assumptions are identified.
- Security and compliance constraints are respected.
- Proposed actions are safe and reversible.

## Output
- Prioritized hypotheses
- Action plan with owner and ETA
- Escalation decision, if needed

## Revision History
- 2026-05-07: Initial template.
