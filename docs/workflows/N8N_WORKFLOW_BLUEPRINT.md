# n8n Workflow Blueprint (Datadog AI + Bitbucket AI + Cursor)

## Purpose
Provide an implementation-ready blueprint for n8n orchestration in AI-assisted DevOps workflows.

**Hard rule:** no autonomous infrastructure actions from n8n.

---

## 1) Recommended Workflow Set

Design principle:
- Incident Monitoring workflow and CI/CD workflow are independent.
- Both publish signals to a shared Predictability workflow.

## WF-01 `incident-triage-orchestrator`
**Goal:** Orchestrate incident triage from alert to decision brief.

**Trigger options**
- Datadog webhook (monitor alert)
- Manual webhook trigger for test/simulation

**Main nodes (suggested)**
1. `Webhook Trigger` (Datadog event)
2. `Function` (normalize payload to standard schema)
3. `IF` (severity filter for pilot scope)
4. `HTTP Request` (Datadog context enrichment)
5. `HTTP Request` (Bitbucket recent PRs/commits)
6. `HTTP Request` (Cursor prompt endpoint / AI service bridge)
7. `Function` (assemble decision brief)
8. `Approval` (human validation checkpoint)
9. `Slack/Teams/Jira` (notify + create incident comment)
10. `Datastore/DB` (audit log)

**Output**
- Structured incident decision brief (status, hypotheses, evidence, next manual checks).

---

## WF-02 `cicd-failure-triage-orchestrator`
**Goal:** Analyze CI failure and route manual next steps.

**Trigger options**
- Bitbucket pipeline failure webhook

**Main nodes (suggested)**
1. `Webhook Trigger` (pipeline failed event)
2. `Function` (extract repo, commit, pipeline id)
3. `HTTP Request` (Bitbucket logs/artifacts metadata)
4. `HTTP Request` (Bitbucket AI analysis)
5. `Function` (validate branch and PR standards for development changes)
6. `IF` (release gate criteria met for staging?)
7. `HTTP Request` (Release Manager API trigger for staging)
8. `HTTP Request` (Datadog staging validation snapshot)
9. `Datastore/DB` (audit log)

**Output**
- Failure cause summary, CI/CD policy validation result, and staging trigger decision.

**Important**
- PR workflow is limited to development changes.
- Incident operational actions do not require PR.

---

## WF-03 `release-risk-briefing-gate`
**Goal:** Generate release risk brief and enforce Go/No-Go gate input quality.

**Trigger options**
- Release Manager event (pre-release checkpoint)
- Scheduled trigger before release windows

**Main nodes (suggested)**
1. `Schedule/Webhook Trigger`
2. `HTTP Request` (Datadog service health trends)
3. `HTTP Request` (Bitbucket change summary since last release)
4. `HTTP Request` (Cursor risk briefing prompt)
5. `Function` (risk score + rationale normalization)
6. `Approval` (release owner + manager)
7. `Release Manager API` (post recommendation and checklist status)
8. `Datastore/DB` (audit log)

**Output**
- Release risk brief with evidence, confidence, and manual recommendation.
- Optional automatic trigger to staging after gate approval.

---

## WF-04 `weekly-predictive-briefing`
**Goal:** Publish weekly predictive risk brief per service using independent inputs from Incident and CI/CD workflows.

**Trigger options**
- Weekly schedule (e.g., Mondays 09:00)

**Main nodes (suggested)**
1. `Schedule Trigger`
2. `Function` (iterate service list)
3. `HTTP Request` (Datadog trend extraction)
4. `HTTP Request` (Bitbucket change velocity and failure signals)
5. `HTTP Request` (ingest normalized outputs from WF-01 and WF-02)
5. `HTTP Request` (Cursor predictive briefing prompt)
6. `Jira/Confluence/Slack` (publish summary)
7. `Datastore/DB` (audit log)

**Output**
- Service-level risk level, drivers, warnings, preventive manual actions.

---

## WF-05 `postmortem-draft-assistant`
**Goal:** Auto-assemble a human-reviewed postmortem draft after incident closure.

**Trigger options**
- Incident status changed to resolved

**Main nodes (suggested)**
1. `Webhook Trigger`
2. `HTTP Request` (Datadog timeline/events)
3. `HTTP Request` (Bitbucket merged changes around incident window)
4. `HTTP Request` (Cursor postmortem draft prompt)
5. `Confluence/Jira` (create draft page/ticket section)
6. `Datastore/DB` (audit log)

**Output**
- Draft postmortem with timeline, root-cause hypotheses, actions.

---

## 2) Standard Event Payload (Canonical Schema)

Use this schema between workflows and tools.

```json
{
  "eventId": "evt-2026-05-06-0001",
  "eventType": "incident_alert",
  "source": "datadog",
  "timestamp": "2026-05-06T13:20:00Z",
  "service": {
    "name": "payments-api",
    "environment": "production",
    "ownerTeam": "devops-platform"
  },
  "severity": "sev2",
  "context": {
    "timeWindowMinutes": 60,
    "symptoms": [
      "5xx increase",
      "latency p95 spike"
    ],
    "telemetryLinks": [
      "https://app.datadoghq.com/dashboard/...",
      "https://app.datadoghq.com/logs?query=..."
    ],
    "changeWindow": {
      "from": "2026-05-06T11:00:00Z",
      "to": "2026-05-06T13:20:00Z"
    },
    "bitbucketRefs": {
      "workspace": "company",
      "repo": "payments-api",
      "pullRequests": [],
      "pipelines": []
    }
  },
  "governance": {
    "requiresHumanApproval": true,
    "autonomousInfraActionsAllowed": false,
    "containsSensitiveData": false
  },
  "requestedOutput": {
    "type": "incident_decision_brief",
    "sections": [
      "situation",
      "impact",
      "hypotheses",
      "evidence",
      "manual_next_checks",
      "risks"
    ]
  }
}
```

---

## 3) Decision Brief Output Contract

```json
{
  "briefId": "brief-2026-05-06-013",
  "service": "payments-api",
  "environment": "production",
  "summary": {
    "situation": "Elevated 5xx and latency regression in last 25 min",
    "impact": "Checkout failures for 12% of requests"
  },
  "hypotheses": [
    {
      "title": "DB pool saturation",
      "confidence": "high",
      "supportingEvidence": [
        "Connection timeout increase",
        "Pool utilization above 95%"
      ],
      "contradictingEvidence": []
    }
  ],
  "manualNextChecks": [
    "Validate connection pool metrics by pod",
    "Compare behavior before/after latest deploy"
  ],
  "recommendedManualActions": [
    "Consider rollback via standard release process"
  ],
  "governance": {
    "humanApprovalRequired": true,
    "approvedBy": null
  }
}
```

---

## 4) n8n Approval Gate Pattern

Use this reusable pattern in all prod-impacting branches:
1. Build recommendation packet
2. Route to approver channel (Slack/Teams/Jira)
3. Wait for explicit approve/reject action
4. If rejected -> log and notify
5. If approved -> continue to manual execution guidance only

Never continue automatically to deployment or infrastructure mutation.

---

## 5) Security Controls in n8n

- Use separate credentials per environment (dev/stage/prod)
- Default integrations to read-only permissions
- Keep secrets in official secret manager integrations only
- Mask or drop sensitive fields before AI calls
- Log all approvals, decisions, and outbound API requests
- Add retry limits and circuit-breaker behavior to avoid loops

---

## 6) Implementation Order (Suggested)

1. Implement `incident-triage-orchestrator`
2. Implement `cicd-failure-triage-orchestrator`
3. Add `release-risk-briefing-gate`
4. Add `weekly-predictive-briefing`
5. Add `postmortem-draft-assistant`

Start in non-production with replayed events, then promote gradually.

