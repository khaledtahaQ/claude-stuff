# The 360° Epic Scope Decomposition Matrix

This framework ensures that when taking on or breaking down an Epic, no critical engineering, operational, or security scope is overlooked.

---

## The 6 Pillars of Epic Scope

```
                      ┌────────────────────────┐
                      │    EPIC INITIATIVE     │
                      └───────────┬────────────┘
         ┌────────────────────────┼────────────────────────┐
         ▼                        ▼                        ▼
┌──────────────────┐    ┌──────────────────┐    ┌──────────────────┐
│ 1. Core Feature  │    │ 2. Data & Schema │    │ 3. Observability │
│  - User stories  │    │  - Migrations    │    │  - Metrics       │
│  - Edge cases    │    │  - Backfill      │    │  - Dashboards    │
│  - Error states  │    │  - Idempotency   │    │  - Alerts        │
└──────────────────┘    └──────────────────┘    └──────────────────┘
         ▲                        ▲                        ▲
         ├────────────────────────┼────────────────────────┤
         ▼                        ▼                        ▼
┌──────────────────┐    ┌──────────────────┐    ┌──────────────────┐
│ 4. Infra & CI/CD │    │ 5. QA & Security │    │ 6. Ops & Handoff │
│  - Terraform/IaC │    │  - E2E Tests     │    │  - Runbooks      │
│  - Pipeline jobs │    │  - Load Tests    │    │  - Confluence    │
│  - Secrets/Env   │    │  - Pen test/SAST │    │  - Change Mgmt   │
└──────────────────┘    └──────────────────┘    └──────────────────┘
```

---

## Scope Decomposition Rubric

When analyzing an epic, run through each section and generate child Jira tickets for items marked **Required**:

### Pillar 1: Core Functional & Edge Cases
* **Happy Path Stories**: Primary user and service consumer workflows.
* **Edge Cases & Failure Modes**:
  * Downstream service timeouts / circuit breaking.
  * Empty / malformed payloads and boundary conditions.
  * Concurrency / race conditions (e.g., duplicate events).
* **Backwards Compatibility**: Ensuring existing API clients or database records continue to function during phased rollout.

### Pillar 2: Data, Schemas & Migrations
* **Schema Evolution**: Forward-compatible DB migrations (expand-and-contract pattern).
* **Data Backfill & Seeding**: Idempotent scripts to backfill existing production records.
* **Performance & Indexes**: Queries profiled, indices added to avoid table scans.
* **Rollback Script**: Explicit down-migration or data restore procedure tested.

### Pillar 3: Observability, Metrics & Alerts
* **Application Metrics**: Custom Prometheus / OpenTelemetry counters, gauges, and timers for the new business process.
* **Grafana Dashboard**: A dedicated dashboard panel visualizing request rates, error rates, and p95/p99 latency.
* **Alert Rules**: PagerDuty or Slack alerts on elevated error rate (>1%) or missing heartbeats.
* **Structured Logs**: Key correlation IDs (`traceId`, `ticketId`, `organizationId`) present on every log statement.

### Pillar 4: Infrastructure & CI/CD
* **IaC & Terraform**: Azure resources, storage accounts, queues, or service buses declared as code.
* **Secrets Management**: Secrets provisioned in Azure KeyVault (never plaintext or committed).
* **CI/CD Pipeline Jobs**: Automated build, test, lint, and deploy stages added to `.gitlab-ci.yml`.

### Pillar 5: Testing, QA & Security
* **Automated Test Matrix**: Unit tests (>80% coverage on new logic) + integration tests.
* **Load / Performance Testing**: Verified against peak throughput requirements.
* **Security & Auth**: Principle of least privilege for Service Principals and role-based access control (RBAC).

### Pillar 6: Documentation, Runbooks & Change Management (CM)
* **Confluence Architecture Page**: Architecture decision records (ADRs), component diagrams, and API docs.
* **On-Call Runbook**: Troubleshooting guide for on-call engineers when alerts trigger.
* **Change Management (CM) Ticket**: Formal CloudX change record scheduled for production deployment.
