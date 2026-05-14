# Checklist - Environment setup and per-release collection

Version: 0.1 | Date: 2026-05-14

This document combines **(A)** one-time setup before operating the flow with n8n/Cursor/Datadog/Jira/Release Manager, and **(B)** the repeatable **per-release** intake for the analysis package.

---

## Part A - Environment setup (once, or per dev/stage)

### A.1 Governance and data

- [ ] AI/data usage policy read (`policies/en/` in the hub).
- [ ] Agreed exclusions for prompts (PII, tokens, full customer payloads, secrets).
- [ ] Named approver(s) for Datadog monitor/dashboard changes (primary + backup).
- [ ] TTL or review cadence for temporary release artefacts (tags, post-release monitors).

### A.2 Jira (or task tracker)

- [ ] Service account or bot user with read access to project issues (write only if strictly required).
- [ ] Minimum fields on tasks: PR/branch link, impacted **service**, declared **risk**, acceptance criteria.
- [ ] Labelling or fix version / **Release** field convention to filter tasks per release.

### A.3 Release Manager (or equivalent)

- [ ] Identified **event** meaning “release ready for analysis” (webhook, API, manual export).
- [ ] If API/webhook: URL, method, sample payload, **secret** stored in a vault (not in git).
- [ ] Agreed archive location for the **analysis package** (custom field, attachment, Confluence, child ticket).

### A.4 Git / Bitbucket (or GitHub/GitLab)

- [ ] Token with read access to **repos**, **PRs**, and **diffs** for release repositories.
- [ ] Convention for linking tasks to PRs (URL field, `ABC-123` in PR title, etc.).

### A.5 Datadog

- [ ] API key and application key with **minimum scopes** for the pilot.
- [ ] If creating monitors/dashboards via API: `monitors_write` and/or `dashboards_write` only on the correct account; prefer role-scoped permissions.
- [ ] **Service** list (`service:` tag or equivalent) aligned with Jira.
- [ ] Base **dashboard URLs** per service (for prompts and human validation).

### A.6 LLM and n8n (optional automation; manual ritual can start without this)

- [ ] Decision: **manual mode** (export + analysis in Cursor) vs **orchestrated** (n8n calls an LLM).
- [ ] If LLM in n8n: credential (OpenAI, Azure OpenAI, etc.) with **cost cap** and agreed model.
- [ ] If n8n: instance, per-node credentials, **execution logs** policy (no sensitive data retention beyond need).
- [ ] Baseline workflow (even skeleton): trigger -> Jira -> Git -> optional Datadog snapshot -> **human approval** before any Datadog `write`.

### A.7 Cursor (prompt development and review)

- [ ] Docs/prompt repo on workspace (this hub or internal repo).
- [ ] If using Jira MCP in Cursor: same data constraints as A.1.
- [ ] Agreed output schema: use `artefatos/en/02_PROMPT_RELEASE_ANALYSIS_V1.md`.

### A.8 “Sample release” validation (Phase 1 exit criteria)

- [ ] A fictitious or past release was assembled in **under 30 minutes** using only Part B.
- [ ] Gaps logged (missing fields, slow APIs, task-PR mapping) with corrective actions.

---

## Part B - Per-release collection (before AI analysis)

### B.1 Release identity

- [ ] Release name/version: `<RELEASE_ID>`
- [ ] Go-live window: `<WINDOW>`
- [ ] Target environment: `<prod|stage|...>`
- [ ] Release owner contact: `<NAME>`

### B.2 Task list

- [ ] Keys (e.g. `ABC-123`): `<LIST>`
- [ ] PII-free export/summary (link to export): `<SOURCE>`
- [ ] Confirmation: no secrets pasted into raw prompts.

### B.3 Code change linkage

- [ ] Per task: linked PR(s)/commits (or “no PR” with justification).
- [ ] Release branch or tag: `<BRANCH_OR_TAG>`
- [ ] Aggregated diff or changed file list available (link or export).

### B.4 Pipeline / artefact (optional)

- [ ] Pipeline run links/IDs: `<PIPELINE>`
- [ ] Artefact/image version: `<ARTEFACT>`
- [ ] Status: green / known failures with documented waivers.

### B.5 Datadog context (optional)

- [ ] Impacted services: `<SERVICES>`
- [ ] Current dashboard URLs: `<URLS>`
- [ ] Existing monitors/SLOs that must **not** be duplicated: `<NOTES>`

### B.6 Handoff to analysis

- [ ] Package stored in a single place (internal markdown, “release intel” ticket, etc.).
- [ ] Analysis executed with `02_PROMPT_RELEASE_ANALYSIS_V1.md` (Cursor and/or n8n+LLM).
- [ ] Output reviewed by a human before go/no-go.

---

## Part C - After analysis (gate and observability)

- [ ] **Go/No-Go** recorded with name and date.
- [ ] “Create monitor/dashboard” items **approved** or declined with rationale.
- [ ] If applicable: new Datadog monitor/dashboard IDs: `<IDS>`
- [ ] Final package attached per A.3 convention.

---

## Revision history

| Version | Date | Notes |
|---------|------|-------|
| 0.1 | 2026-05-14 | Initial (setup + intake + post) |
