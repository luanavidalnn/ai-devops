# Release Change Intelligence POC (DevOps AI — Point 1)

**Document type:** Confluence-ready (copy from Markdown or paste sections manually)  
**Audience:** DevOps, SRE, Release Management, Engineering leads, Security (review)  
**Repository reference:** `release_change_intelligence/` in the internal `ai-devops` documentation hub  
**Version:** 1.0 | **Date:** 2026-05-14

---

## Purpose

Establish a **repeatable, human-gated** practice where each **release** (with a **task list** from Jira and **actual code changes** from pull requests) is analysed **before production** to:

1. Correlate **task intent** with **implemented changes**  
2. Surface **plausible production risks** with explicit **evidence**, **confidence**, and **data gaps**  
3. Produce an **observability package** for the post-go-live window (what to watch, suggested signals, manual checks)

This initiative is **Point 1** of the broader DevOps AI adoption plan. It does **not** require CI/CD orchestration as a prerequisite. **No autonomous infrastructure actions** are in scope.

---

## Out of scope (initial POC)

- Fully automated Datadog monitor/dashboard creation without human approval  
- Sending secrets, tokens, or PII into AI prompts  
- Replacing human go/no-go decisions

---

## Principles (non-negotiable)

| Principle | Meaning |
|-----------|---------|
| AI advises; humans decide | No deploy, restart, scale, or monitor writes triggered autonomously by AI |
| Evidence or gap | Critical statements must cite task text or code context, or list missing data |
| Observability owns “watching” | AI proposes *what* to monitor; the team implements monitors/dashboards or active review |

---

## Deliverables (source of truth in Git)

| Artefact | Path (repo) |
|----------|-------------|
| Implementation plan | `release_change_intelligence/pt/01_PLANO_IMPLANTACAO_INTELIGENCIA_MUDANCA_RELEASE.md` (PT) / `en/01_RELEASE_CHANGE_INTELLIGENCE_IMPLEMENTATION_PLAN.md` (EN) |
| Setup + per-release checklist | `release_change_intelligence/artefatos/en/01_CHECKLIST_RELEASE_COLLECTION_AND_SETUP.md` |
| Analysis prompt V1 | `release_change_intelligence/artefatos/en/02_PROMPT_RELEASE_ANALYSIS_V1.md` |
| Post-release pilot note | `release_change_intelligence/artefatos/en/03_POST_RELEASE_PILOT_NOTE.md` |
| Jira bulk import (Epic + issues) | `data/jira/jira_release_change_intelligence_poc.csv` |

---

## Phased rollout (summary)

| Phase | Goal | Exit criteria (examples) |
|-------|------|---------------------------|
| 0 | Alignment | Owner, definition of “release”, minimum task fields agreed |
| 1 | Intake | Sample release package assembled in **under 30 minutes** |
| 2 | Prompt V1 | Two people produce structurally comparable outputs for the same test release |
| 3 | Pilot | N real releases (suggest 3–5); post-release note per release |
| 4 | Consolidation | Prompt/checklist V2; lightweight KPIs; retro documented |

---

## Tooling roles (realistic)

| Tool | Role |
|------|------|
| **Jira** | Task list, descriptions, links to PRs, release metadata |
| **Git / Bitbucket** | Authoritative **diff** and changed files |
| **Cursor** | Human-in-the-loop analysis, prompt development, `@` file context |
| **n8n** (optional later) | Orchestrate fetch + LLM + approval gate; never skip human review for writes |
| **Datadog** | Baseline dashboards/monitors; optional API apply **after** approval |

---

## Success metrics (POC)

- **Time to build** the release analysis package (target: downward trend after week 2)  
- **Usefulness** of observability suggestions (adopted vs declined with reason)  
- **Zero** critical policy violations (secrets/PII in prompts, unapproved autonomous changes)  
- **Calibration**: at least one checklist or prompt improvement per pilot cycle documented in post-release notes

---

## Security and compliance

- Follow internal AI / data policy; align with hub governance under `policies/en/`  
- Redact customer data; use issue keys, sanitized excerpts, and repo-relative paths  
- Log go/no-go with approver identity when attaching recommendations to the release record

---

## How to use this page in Confluence

1. Create a new page under your **DevOps** or **Platform** space.  
2. Paste this Markdown if your Confluence version supports Markdown import; otherwise paste section by section and apply heading styles (H1/H2/H3).  
3. Add links to your **mirrored** repo URL (Bitbucket/GitHub) for the paths above.  
4. Link the **Jira Epic** created from `jira_release_change_intelligence_poc.csv` in a “Related work” panel.

---

## Revision history

| Version | Date | Author | Notes |
|---------|------|--------|-------|
| 1.0 | 2026-05-14 | ai-devops hub | Initial Confluence export |
