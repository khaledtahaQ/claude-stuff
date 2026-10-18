# Jira Engineering & Epic Management Skills

A battle-tested skill suite for **Claude Code**, designed for engineers, leads, and DevOps engineers managing Jira tickets, strict Definition of Done (DoD) validation, and complete Epic breakdowns without missed scope.

---

## Directory Structure

```
/Users/khaledtaha/.claude/skills/khaled-skills/jira/
├── ticket-lifecycle/
│   └── SKILL.md                 # Intake, DoR triage, branch kickoff, WIP/standup updates
├── ticket-closer/
│   └── SKILL.md                 # 5-Gate DoD validation, test proof, resolution summary
├── epic-manager/
│   └── SKILL.md                 # 360° Epic decomposition, blindspot detection, zero-orphan closure
└── references/
    ├── dod-checklist.md         # Comprehensive Definition of Done checklist
    ├── ticket-templates.md      # Standard Jira comment & communication templates
    └── epic-scope-matrix.md     # 6-pillar breakdown rubric ensuring no scope is forgotten
```

---

## Skills Overview

### 1. `jira-ticket-lifecycle`
* **Path:** `ticket-lifecycle/SKILL.md`
* **Trigger Phrases:** `"handle ticket"`, `"start ticket"`, `"triage ticket"`, `"kickoff ticket"`, `"update ticket"`, `"ticket standup"`, `"ticket progress"`, `"sync ticket"`
* **Core Capabilities:**
  * **Definition of Ready (DoR) Gate:** Audits incoming tickets for clear user stories, measurable Acceptance Criteria (AC), and reproduction steps for bugs. Blocks work on vague tickets with a polite clarification comment.
  * **Kickoff Automation:** Computes standard git branch names (`feat/CLOUDX-1234-...` or `fix/...`), verifies status transitions to `In Progress`, and drafts a kick-off comment.
  * **Keep Updated (WIP / Standup):** Inspects local git diffs and commits to write crisp, high-signal progress updates with milestones, next steps, and explicit blocker flags.

---

### 2. `jira-ticket-closer`
* **Path:** `ticket-closer/SKILL.md`
* **Trigger Phrases:** `"close ticket"`, `"resolve ticket"`, `"finish ticket"`, `"complete ticket"`, `"ticket dod"`, `"definition of done"`, `"validate before close"`
* **Core Capabilities:**
  * **5-Gate DoD Audit:**
    1. *Code & Merge:* MR merged to trunk, pipeline green, reviewer comments resolved.
    2. *Acceptance Criteria:* 100% of Jira AC items verified.
    3. *Test Evidence:* Unit tests + manual/API cURL proof recorded.
    4. *Documentation:* Confluence / README / Secrets documented.
    5. *Change Management (CM):* Verified if production infrastructure is touched.
  * **Audit Scorecard:** Prints a clear Pass/Fail table before touching Jira.
  * **Resolution Summary Comment:** Posts a formal 4-part resolution record (Objective, Root Cause/Solution, Verification Evidence, Deployment Notes).
  * **Safety Confirmation Gate:** Never resolves or closes a ticket without explicit user confirmation.

---

### 3. `jira-epic-manager`
* **Path:** `epic-manager/SKILL.md`
* **Trigger Phrases:** `"handle epic"`, `"epic breakdown"`, `"decompose epic"`, `"epic health"`, `"epic progress"`, `"audit epic"`, `"epic missing stuff"`, `"close epic"`
* **Core Capabilities:**
  * **360° Scope Decomposition:** Prevents missed requirements by checking all 6 pillars (Core Features, Edge Cases, Data/Migrations, Observability/Alerts, Infra/CI/CD, Ops/Change Management).
  * **Blindspot & Risk Scanner:** Scans child tickets for missing monitoring, unassigned tasks, stalled tickets (>5 days), and circular blockers.
  * **Zero-Orphan Closure:** Enforces that all child tickets are properly resolved or explicitly triaged before an epic can be closed.
  * **Executive Health Dashboard:** Rollup completion %, risk level (On Track / At Risk / Blocked), and immediate recommended actions.

---

## Quick Usage Examples with Claude Code

### Starting a Ticket
> *"Claude, handle ticket CLOUDX-5426. Check the requirements and help me kickoff."*

### Posting a Progress Update
> *"Claude, update ticket CLOUDX-5426 with what we did today based on our git commits."*

### Closing a Ticket with Validation
> *"Claude, validate DoD and close ticket CLOUDX-5426."*

### Decomposing an Epic
> *"Claude, break down epic CLOUDX-5000 using the 360 scope matrix so we don't miss anything."*

### Auditing Epic Health
> *"Claude, audit epic CLOUDX-5000 for any blindspots, missing tickets, or blockers."*
