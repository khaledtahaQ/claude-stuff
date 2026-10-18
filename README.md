# Khaled's Claude Skills (`claude-stuff`)

Custom skills, prompts, and automation workflows for **Claude Code** engineered for Restaurant365 engineering, CloudX DevOps, and agile management.

---

## Repository Structure

```
.
├── README.md
└── jira/
    ├── README.md
    ├── handle-ticket/
    │   └── SKILL.md             # jira-handle-ticket (Intake, DoR, Kickoff, Standup updates)
    ├── ticket-closer/
    │   └── SKILL.md             # jira-ticket-closer (5-Gate DoD verification & resolution notes)
    ├── epic-manager/
    │   └── SKILL.md             # jira-epic-manager (360° Scope Matrix, blindspot scanner, zero-orphan closure)
    └── references/
        ├── dod-checklist.md     # Definition of Done reference checklist
        ├── ticket-templates.md  # Standard Jira comments & communication templates
        └── epic-scope-matrix.md # 6-pillar breakdown rubric ensuring no scope is forgotten
```

---

## Skills Catalog

### 1. [`jira-handle-ticket`](jira/handle-ticket/SKILL.md)
* **Triggers:** `"handle ticket"`, `"start ticket"`, `"triage ticket"`, `"kickoff ticket"`, `"update ticket"`, `"ticket standup"`, `"ticket progress"`, `"sync ticket"`
* **Capabilities:**
  * **Definition of Ready (DoR) Gate:** Audits incoming tickets for clear user stories, testable Acceptance Criteria, and reproduction steps for bugs.
  * **Work Kickoff:** Verifies assignment, transitions status to `In Progress`, creates git branch following standard naming (`feat/CLOUDX-1234-...` or `fix/...`), and drafts kick-off comment.
  * **Keep Updated (WIP / Standups):** Collects local git diffs/commits to generate concise, high-signal progress updates and explicit blocker escalations.

### 2. [`jira-ticket-closer`](jira/ticket-closer/SKILL.md)
* **Triggers:** `"close ticket"`, `"resolve ticket"`, `"finish ticket"`, `"complete ticket"`, `"ticket dod"`, `"definition of done"`, `"validate before close"`
* **Capabilities:**
  * **5-Gate Definition of Done (DoD) Audit:**
    1. MR merged to trunk & CI/CD pipeline green.
    2. 100% of Acceptance Criteria verified.
    3. Unit tests passing + manual verification evidence (cURLs/logs) recorded.
    4. Documentation, secrets, and repository README updated.
    5. Change Management (CM) Confluence page created/verified for production infra changes.
  * **Resolution Summary Generator:** Drafts formal 4-part resolution record (Objective, Solution, Verification Evidence, Operational Notes) before requesting confirmation to close.

### 3. [`jira-epic-manager`](jira/epic-manager/SKILL.md)
* **Triggers:** `"handle epic"`, `"epic breakdown"`, `"decompose epic"`, `"epic health"`, `"epic progress"`, `"audit epic"`, `"epic missing stuff"`, `"close epic"`
* **Capabilities:**
  * **360° Scope Decomposition:** Decomposes epics into stories across 6 required pillars (Core Features, Edge Cases, Data/Migrations, Observability/Alerts, Infra/CI/CD, Ops/Change Management).
  * **Blindspot & Risk Scanner:** Scans child tickets for missing monitoring, unassigned tasks, stalled tickets (>5 days), and circular blockers.
  * **Zero-Orphan Closure:** Enforces that 100% of child tickets are resolved or formally triaged before closing the parent epic.

---

## Global Claude Code Setup

In `~/.claude/skills/`, symlinks are established to enable auto-discovery:
* `jira-handle-ticket` & `jira-handle-ticket.md`
* `jira-ticket-closer` & `jira-ticket-closer.md`
* `jira-epic-manager` & `jira-epic-manager.md`
