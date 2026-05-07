# Template - Docker Image and ECR Troubleshooting

## When to use
When pipeline fails on Docker image build/push/pull to ECR.

## Prompt
```text
Act as a CI/CD specialist for Docker image and ECR troubleshooting.

Context:
- repository: <REPO>
- pipeline id: <PIPELINE_ID>
- stage: <BUILD|PUSH|PULL>
- image/tag: <IMAGE_TAG>
- registry/ECR: <ECR_URI>
- error logs: <ERROR_LOGS>
- environment: <ENVIRONMENT>

Task:
Explain probable cause and list manual validations.

Required format:
1) Diagnostic summary
2) Possible causes (ordered)
3) Evidence supporting each cause
4) Manual validation checklist:
   - ECR authentication
   - IAM permissions
   - tag/repository existence
   - Docker cache/layers
   - network/connectivity
5) Suggested fix (manual)
6) Recurrence prevention

Rules:
- Do not execute push/pull/delete automatically.
- Do not expose credentials in the output.
```
