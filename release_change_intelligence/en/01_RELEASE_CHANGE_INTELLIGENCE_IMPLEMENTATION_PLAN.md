# Implementation plan - Release change intelligence (Point 1)

## Goal

Establish a repeatable process where each **release** with a **task list** lets the DevOps team (and engineering partners) use **AI assistance** to:

1. **Correlate** task intent with **actual changes** in PRs (code, config, migrations, flags).
2. **Anticipate** plausible production issues (**evidence** and **confidence** must be explicit).
3. **Produce an observability package** for the post-release window (metrics, logs, symptoms, manual checks) — without autonomous infrastructure execution.

This process does **not** require CI/CD orchestration as its driver; pipelines may be an **optional** context source (build status, artifact, version).

## Non-negotiable principles

- **AI recommends; humans decide.** No deploy/restart/scale/infra monitor edits triggered autonomously by AI.
- **No secrets and no PII** in prompts. Use IDs, minimal code excerpts, sanitized descriptions.
- **Critical conclusions** must cite **evidence** (task excerpt, file, diff) or be listed as **data gaps**.
- **Continuous monitoring** remains the observability platform’s job; AI **defines what to watch**; the team **implements** alerts/dashboards or active review during the agreed post-release window.

## Scope

### In scope

- Minimum **inputs per release** (tasks + linkage to PRs/commits).
- **Analysis template** (prompt + fixed output schema) and human pre/post go-live checklist.
- Pilot across **N releases** (suggested: 3–5) and a retro to refine the template.
- **Observability handoff**: prioritized signal list and suggested queries (text) attached to the release record.

### Out of scope (initial phase)

- Full n8n/webhook automation (can be **Point 2** after the manual ritual is validated).
- Automated external risk scoring without human calibration.
- AI access to unnecessary production data (full logs with PII, credentials).

## Prerequisites

| Area | Minimum |
|------|---------|
| Traceability | Tasks reference PRs/branches, or a reviewed manual mapping exists |
| AI tool | Cursor (or equivalent) with repo access and an approved usage policy |
| Governance | Hub policy under `policies/en/` acknowledged by the team |
| Observability | Impacted services already have baseline dashboards/monitors to anchor suggestions |

## Rollout phases

### Phase 0 - Alignment (0.5–1 day)

**Outputs**: owner + backup; definition of “release” in your org; mandatory task fields (risk, validation plan, PR link).

**Done when**: team agrees on minimum inputs and what counts as the observability package.

### Phase 1 - Intake (2–3 days)

**Outputs**: per-release **collection checklist** under `release_change_intelligence/artefacts/en/`; convention for exporting task text safely.

**Done when**: a sample release can be assembled in under 30 minutes by the release owner.

### Phase 2 - Analysis template v1 (3–5 days)

**Outputs**: approved prompt + **fixed output format** (per task: risks, evidence, confidence, gaps, what to monitor); v1 stored under `artefacts/en/`.

**Done when**: two different people produce structurally comparable outputs for the same test release.

### Phase 3 - Controlled pilot (2–4 weeks)

**Outputs**: **N real releases** with analysis completed pre go-live; **post-release note** (1 page): observed vs anticipated.

**Done when**: at least one **data gap** per release is addressed; at least one **monitoring suggestion** is adopted or declined with rationale.

### Phase 4 - Consolidation (1 week)

**Outputs**: template **v2** from pilot learnings; lightweight KPIs (time to build package; useful post-release signal rate; critical gap count).

**Done when**: retro captures **3 learnings** and **2 process/prompt changes**.

## Suggested roles

Release owner: task list + PR mapping + attach package to release record.  
DevOps/SRE: translate “what to watch” into monitors/dashboards or active review.  
Engineering: sufficient task context + PR links.  
Security: consult on high-risk releases and data handling.

## Plan revision history

| Version | Date | Notes |
|---------|------|-------|
| 0.1 | 2026-05-14 | Initial plan in hub |
