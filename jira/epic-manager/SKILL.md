---
name: jira-epic-manager
version: "1.0"
description: >
  Decomposes, audits, tracks, and closes Jira Epics without missing scope, edge cases, or dependencies.
  Use when breaking down an epic into user stories/tasks, auditing an epic for missing requirements or blindspots,
  tracking epic burndown/progress, analyzing blocker chains, or validating epic closure.
triggers:
  - "handle epic"
  - "epic breakdown"
  - "decompose epic"
  - "break down epic"
  - "epic health"
  - "epic progress"
  - "audit epic"
  - "epic missing stuff"
  - "close epic"
  - "epic sign off"
allowed-tools:
  - "bash"
  - "read"
  - "grep"
  - "glob"
  - "mcp__atlassian__getJiraIssue"
  - "mcp__atlassian__search"
  - "mcp__atlassian__searchConfluenceUsingCql"
  - "mcp__atlassian__getConfluencePage"
metadata:
  author: Khaled Taha
  category: jira-management
  team: Engineering / CloudX
---

# Jira Epic Manager & Blindspot Auditor

End-to-end framework for decomposing epics into exhaustive user stories, discovering hidden technical blindspots, tracking health, and managing airtight epic closure.

---

## Operating Principles

1. **MECE Breakdown**: Every epic must be decomposed into Mutually Exclusive, Collectively Exhaustive user stories. Zero forgotten tasks.
2. **The 360° Scope Matrix**: A feature is not complete just because code works. Observability, migrations, security, rollback, documentation, and change management are mandatory tickets.
3. **Continuous Dependency Auditing**: Identify critical path bottlenecks before work starts, not during sprint crunch.
4. **Zero-Orphan Closure**: Never close an epic if child tickets are left open, unassigned, or undocumented.

---

## Phase 1: Epic Decomposition (No Scope Missed)

When breaking down an Epic (e.g., from an initiative description, PRD, or architecture proposal):

### 1.1 Apply the 360° Scope Matrix
Every epic must be evaluated across all 6 dimensions. Create explicit Jira child tickets for each applicable area:

| # | Dimension | What to Check & Ticket |
|---|---|---|
| **1** | **Core Functional** | Happy paths, edge cases, input validation, error responses, backward compatibility. |
| **2** | **Infrastructure & Config** | Terraform/IaC updates, Kubernetes manifests, KeyVault secrets, firewall/network rules, service bus/queues. |
| **3** | **Data & Migrations** | DB schema migrations, data backfill scripts, indexing, idempotency, data retention. |
| **4** | **Observability & Alerting** | Grafana dashboards, OpenTelemetry traces, Prometheus metrics, Slack/PagerDuty alert thresholds. |
| **5** | **Testing & QA** | Unit test suites, end-to-end integration tests, load/stress testing, QA verification plan. |
| **6** | **Ops & Compliance** | Confluence runbooks, repository README updates, Change Management (CM) ticket for production. |

### 1.2 Structure of Generated Child Tickets
Format each child ticket strictly:
```markdown
*Summary:* [Service/Area] - [Actionable Verb] [Feature/Component]
*Issue Type:* Story / Task / Spike
*Epic Link:* [EPIC-KEY]
*Priority:* High / Medium / Low
*Dependencies:* Blocked by [KEY-X] / Blocks [KEY-Y]

*Description / User Story:*
As a [user role / service consumer]
I want [capability / change]
So that [business outcome / architectural benefit]

*Acceptance Criteria:*
- [ ] Given [precondition], when [action taken], then [expected outcome].
- [ ] [Boundary condition / Error case validation].
- [ ] [Telemetry/logging verified].

*Technical Notes & Repositories:*
- Target Repo: [e.g., compeat/development/r365/...]
- Key classes/files impacted: [...]
```

---

## Phase 2: Epic Health Check & Blindspot Audit

Run this periodically or midway through an epic to detect slippage and gaps:

### 2.1 Child Ticket Rollup
Fetch all tickets linked to the Epic and calculate:
* **Total Tickets:** Count
* **Status Distribution:** `Done` / `In Progress` / `In Review` / `Open` / `Blocked`
* **Progress Percentage:** `(Done / Total) * 100`

### 2.2 Blindspot & Gap Detection Checklist
Actively query the epic tree for common fatal omissions:
- [ ] **Observability Blindspot:** Are there tickets for dashboards and alerts? If not, flag as: `⚠️ Missing Observability Ticket`.
- [ ] **Database / Migration Blindspot:** If DB changes exist, is there a migration rollback test ticket?
- [ ] **Secret / Permission Blindspot:** Are there tickets for provisioning KeyVault access or service principals?
- [ ] **Stalled Tickets:** Any ticket `In Progress` for > 5 days with no comments?
- [ ] **Critical Path Blockers:** Are any unstarted stories blocked by already delayed stories?

### 2.3 Epic Health Dashboard Output
Generate the report:

```markdown
📊 Epic Health Dashboard: [EPIC-KEY] - [Summary]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Status: [ON TRACK / AT RISK / BLOCKED]
Completion: [XX]% ([N] of [Total] tickets closed)

🚨 Critical Blockers & Bottlenecks:
* [KEY-1] ([Assignee]) is BLOCKED by [KEY-2] - [Reason]

🔍 Discovered Blindspots / Missing Scope:
* ⚠️ Missing Observability: No tickets found for metrics/alerts.
* ⚠️ Missing Production Change Management: No CM ticket linked.

📋 Child Ticket Status Breakdown:
* ✅ Done (N): [List keys]
* 🔄 In Progress (N): [List keys]
* ⏳ Open / Unstarted (N): [List keys]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Recommended Next Actions:
1. [Action 1 to unblock or address gap]
2. [Action 2]
```

---

## Phase 3: Epic Closure & Sign-Off Gate

Before closing the parent Epic:

### 3.1 Zero-Orphan Verification
* [ ] Verify **every single child ticket** has reached a terminal state (`Done` or `Closed`).
* [ ] If any tickets remain open:
  * Decide with team: either complete them, or explicitly unlink them and move to backlog / follow-up epic with rationale documented.
* [ ] Zero unresolved blockers remaining.

### 3.2 Documentation & Confluence Sign-Off
* [ ] Confluence architecture overview published/updated.
* [ ] Operational runbook published for Support / On-Call engineers.

### 3.3 Epic Final Sign-Off Summary
Post the closing summary to the Epic:

```markdown
h2. 🏁 Epic Completion & Value Delivered Sign-Off

* *Epic:* [EPIC-KEY] - [Epic Title]
* *Epic Lead:* [Full Name]
* *Closure Date:* [YYYY-MM-DD]
* *Total Child Tickets Delivered:* [N] tickets

h3. 1. Value & Business Outcomes Achieved
[Summary of business capabilities or infrastructure improvements unlocked]

h3. 2. Major Components Delivered
* *Services / Repositories Modified:* [List repos]
* *Key Features / APIs:* [List endpoints/features]
* *Infrastructure / Pipelines:* [List Terraform / K8s / CI changes]

h3. 3. Documentation & Operational Hand-off
* *Confluence Architecture Page:* [Link]
* *Operational Runbook / Alerts:* [Link to Grafana / Runbook]
* *Change Management (CM) Record:* [Link]

h3. 4. Post-Launch Verification & Telemetry
* *Metrics Baseline:* [Healthy error rates, latency metrics]
* *Follow-Up / Future Enhancements:* [Linked follow-up tickets or None]
```
*(Require user confirmation before setting Epic status to Closed)*.
