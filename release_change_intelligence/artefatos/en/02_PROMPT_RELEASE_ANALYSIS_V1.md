# Prompt template - Release change intelligence analysis V1

Prompt version: 1.0 | Date: 2026-05-14

## When to use

Before go-live, when you have a **task list**, **task text/summary**, and **links to PRs or a changed-file list**, with or without a Datadog metrics snapshot.

## Using in Cursor

- Reference repo files with `@` (diffs, migrations, configs).
- Paste only the `text` block below into the chat (or adapt for an LLM node in n8n).

## Prompt (copy from the next line through the end of the block)

```text
Act as a senior DevOps analyst. Assess PRODUCTION risk for a release by correlating task intent with actual code changes.

Release context:
- release_id: <RELEASE_ID>
- go_live_window: <DATE_OR_WINDOW>
- target_environment: <ENVIRONMENT>
- impacted_services: <SERVICES>
- task_keys: <ABC-123, ABC-124, ...>

Task text or summary (no PII; if context is missing, declare gaps):
<TASK_TEXT_OR_LINKS>

Code changes (summary or PR/commit list; you may cite files visible in context):
<CODE_CHANGES_OR_PRS>

Optional Datadog context (metrics, SLOs, dashboard links — only safe-to-share material):
<OPTIONAL_DATADOG_CONTEXT>

Optional CI/CD context (pipeline, artefact, version):
<OPTIONAL_CICD_CONTEXT>

Tasks:
1) For EACH task_key, produce a section correlating task -> actual change.
2) For each task, list plausible production risks (do not invent scenarios unrelated to text or diff).
3) For each risk: estimated severity (low/medium/high), confidence (low/medium/high), supporting evidence, contradicting evidence, data gaps.
4) Produce a post-release **observability package**: metrics/logs/traces/symptoms to watch, suggested monitor queries (text), suggested attention window (e.g. 24h/72h), and what would validate mitigation or rollback.
5) Explicit gap list: missing data that would reduce uncertainty.
6) Rollout: suggested order (if applicable), feature flags, cross-task dependencies.

Required response headings:
## Executive summary (advisory go/no-go; not a final decision)
## Task -> change map
## Risks by task
## Observability package (post-go-live)
## Data gaps
## Rollout and dependencies
## Mandatory human review items

Rules:
- Do not execute infrastructure actions; recommendations only.
- Do not expose or request secrets, tokens, or PII.
- When evidence is insufficient, write "UNDETERMINABLE" instead of guessing.
- Response language: English.
```

## Quality bar (human review)

- [ ] Each medium/high risk cites **evidence** from a task or code area.
- [ ] **Data gaps** is non-empty when context was partial.
- [ ] Observability package is **actionable** (not only "watch the system").

## Prompt revision history

| Version | Date | Notes |
|---------|------|-------|
| 1.0 | 2026-05-14 | Initial |
