# Template - CI/CD Pipeline Failure Analysis

## When to use
When a pipeline fails and rapid, evidence-based diagnosis is needed.

## Prompt
```text
Analyze the pipeline failure below in a CI/CD context.

Context:
- repository: <REPO>
- branch: <BRANCH>
- pipeline id: <PIPELINE_ID>
- failed stage: <FAILED_STAGE>
- relevant logs: <LOGS>
- recent commits/PRs: <RECENT_CHANGES>

Task:
Identify probable cause and recommend next manual steps.

Required format:
1) Failure summary
2) Probable cause
3) Log evidence
4) Recommended manual checks
5) Test improvements to prevent recurrence
6) Risk of releasing without fix

Rules:
- Do not execute any action automatically.
- Do not propose production changes without human gate/approval.
- If multiple possible causes exist, prioritize by likelihood.
```
