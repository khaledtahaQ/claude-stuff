---
name: jira-ticket-closer
version: "1.0"
description: >
  Validates strict Definition of Done (DoD) requirements and executes ticket closure for Jira issues.
  Use when closing a ticket, resolving an issue, validating DoD, writing resolution summaries,
  or ensuring all test evidence, MRs, and documentation are verified before closing Jira tickets.
triggers:
  - "close ticket"
  - "resolve ticket"
  - "finish ticket"
  - "complete ticket"
  - "ticket dod"
  - "definition of done"
  - "validate before close"
  - "ready to close ticket"
  - "close jira"
allowed-tools:
  - "bash"
  - "read"
  - "grep"
  - "glob"
  - "mcp__atlassian__getJiraIssue"
  - "mcp__atlassian__search"
metadata:
  author: Khaled Taha
  category: jira-management
  team: Engineering / CloudX
---

# Jira Ticket Closer & DoD Validator

Enforces strict Definition of Done (DoD) verification gates and generates standardized resolution records before closing any Jira ticket.

---

## Operating Principles

1. **Zero-Bypass Verification**: Never mark a ticket as `Done` or `Closed` without verifying merge status, test evidence, and acceptance criteria.
2. **Documented Evidence**: Every closed ticket must contain concrete proof of validation (cURLs, test logs, screenshots, or pipeline links).
3. **Change Management Guardrail**: Infrastructure and production-impacting changes must have an approved Change Management (CM) record linked before closure.
4. **Clean Historical Record**: Generate a structured Resolution Summary comment that leaves an unambiguous audit trail for auditors and teammates.

---

## The 5 Definition of Done (DoD) Gates

Before closing ticket `[KEY]`, execute the following audit checklist:

### Gate 1: Code & Merge Request Verification
* [ ] Working branch has been merged into trunk (`main`, `master`, or designated release branch).
* [ ] GitLab Merge Request status is `Merged` (not open, draft, or closed without merge).
* [ ] CI/CD pipeline on default branch passed successfully (green build, lint, tests).
* [ ] All review discussions / threads in MR are resolved.

### Gate 2: Acceptance Criteria Sign-Off
* [ ] Read original Jira ticket description and Acceptance Criteria.
* [ ] Map every single AC bullet point to verified behavior.
* [ ] No partial deliveries or deferred scope unless explicitly split into a new Jira ticket.

### Gate 3: Automated & Manual Test Evidence
* [ ] Unit and integration tests added or updated.
* [ ] Manual / QA test evidence recorded:
  * API endpoints: Request payload + HTTP 200/201 response snippet or cURL.
  * DB / Data changes: Verification queries + record count before/after.
  * Infra / Kubernetes: `kubectl get`, `az` CLI output, or Grafana dashboard metric.

### Gate 4: Documentation & Knowledge Handoff
* [ ] Repository `README.md` or architecture diagram updated if architecture/config changed.
* [ ] Environment variables / secrets documented in keyvault or configuration docs.
* [ ] Confluence documentation updated (if user-facing or platform-wide capability).

### Gate 5: Production Readiness & Change Management (CM)
* [ ] If modifying CloudX / Production Infrastructure:
  * Change Management page created in Confluence (per R365 Change Management Policy).
  * Post-deployment verification plan documented.
  * Rollback plan tested or verified feasible.
* [ ] If purely internal code/dev work with no prod impact: Mark as `N/A (Standard CI/CD Deploy)`.

---

## Execution Workflow

### Step 1: Run DoD Audit Scorecard
Display the audit result to the user:

```
📋 DoD Audit Scorecard for [TICKET-KEY]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[PASS/FAIL] Gate 1: MR Merged & Pipeline Green (MR: !123, Pipeline #45678)
[PASS/FAIL] Gate 2: Acceptance Criteria Fully Met (3/3 items verified)
[PASS/FAIL] Gate 3: Test Evidence Provided (Unit tests + cURL logs attached)
[PASS/FAIL] Gate 4: Documentation & Readme Updated
[PASS/N/A]  Gate 5: Change Management (CM-452 linked / Not required)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Verdict: [READY TO CLOSE / BLOCKED - ACTION REQUIRED]
```

If any gate fails, do **not** close the ticket. Detail exactly what is missing and provide the corrective action.

---

### Step 2: Generate Formal Resolution Summary

Once all gates pass, compile the official Resolution Summary comment for Jira:

```markdown
h3. ✅ Definition of Done - Resolution Summary

* *Resolution Status:* Resolved / Completed
* *Resolved By:* [Full Name]
* *Completion Date:* [YYYY-MM-DD]

h4. 1. Objective & Scope
[Concise summary of what was delivered or fixed]

h4. 2. Root Cause / Implementation Details
* *Cause (if bug):* [Brief root cause explanation]
* *Solution:* [Architectural/code implementation details]
* *Key Repositories:* [e.g., compeat/development/r365/sync-service]

h4. 3. Verification & Evidence
* *Merge Request:* [GitLab MR link or MR !XXX] (Merged to [main/master])
* *Pipeline Status:* Green (Build #[ID])
* *Automated Tests:* [e.g., 14 unit tests added, 100% passing]
* *Manual Testing Proof:*
{code:json}
[Paste curl request/response or test output snippet here]
{code}

h4. 4. Operational & Deployment Notes
* *Target Environment:* [Production / Staging / All]
* *Change Management:* [Link to Confluence CM page or N/A]
* *Configuration / Secrets Required:* [None / Listed with KeyVault refs]
* *Rollback Plan:* [Revert MR !XXX / Run rollback script]
```

---

### Step 3: Transition Ticket & User Confirmation

1. Show the generated Resolution Summary to the user.
2. Prompt for confirmation:
   > *"Do you want me to post this resolution comment and transition [KEY] to Done/Closed in Jira?"*
3. Upon confirmation, execute transition to `Done` / `Resolved`.
