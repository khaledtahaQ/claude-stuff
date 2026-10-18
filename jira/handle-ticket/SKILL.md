---
name: jira-handle-ticket
version: "1.0"
description: >
  Manages Jira ticket intake, Definition of Ready triage, work kickoff, and daily progress
  updates. Use when a user asks to handle a ticket, start or kickoff a ticket, triage ticket
  requirements, post progress/standup updates, or track blockers on a Jira ticket.
triggers:
  - "handle ticket"
  - "jira-ticket-lifecycle"
  - "start ticket"
  - "triage ticket"
  - "kickoff ticket"
  - "update ticket"
  - "ticket standup"
  - "ticket progress"
  - "post ticket update"
  - "sync ticket"
  - "check ticket status"
  - "jira ticket"
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

# Jira Ticket Lifecycle Manager

Orchestrates Jira ticket intake, Definition of Ready (DoR) validation, branch kickoff, and continuous progress tracking.

---

## Operating Principles

1. **Never start work on an ambiguous ticket**: Missing acceptance criteria or reproduction steps must be flagged before coding.
2. **Traceable Git Workflow**: Branch names and commit messages must consistently reference the Jira ticket key.
3. **High-Signal Updates**: Status comments must capture measurable progress, active blockers, and immediate next steps—not vague "in progress" text.
4. **Read Before Writing**: Always fetch the latest Jira ticket state before proposing comments or status transitions.

---

## Phase 1: Ticket Intake & Definition of Ready (DoR) Gate

When handed a Jira ticket (e.g., `CLOUDX-1234` or issue URL):

### 1.1 Fetch & Parse Ticket State
Inspect the ticket via Atlassian MCP or Jira API:
* **Key Fields**: Summary, Issue Type, Status, Assignee, Priority, Components, Labels, Fix Version, Epic Link.
* **Content**: Description, Acceptance Criteria, linked GitLab MRs, recent comments.

### 1.2 Validate Against Definition of Ready (DoR)
Check all four criteria:
- [ ] **Problem & Scope**: Clear problem statement or user story (*"As a... I want... So that..."*).
- [ ] **Acceptance Criteria (AC)**: Clear, testable pass/fail conditions (Given/When/Then or unambiguous checklist).
- [ ] **Reproduction / Context (Bugs only)**: Environment (Dev/Stage/Prod), steps to reproduce, expected vs actual behavior, log snippets or stack traces.
- [ ] **Dependencies & Permissions**: Required credentials, service dependencies, or external team approvals identified.

### 1.3 Action on DoR Failure
If any critical requirement is missing, do **not** proceed silently. Generate a structured clarification request for the ticket author:

```markdown
h4. ⚠️ Definition of Ready (DoR) - Missing Information
Hi [Author], before starting work on this ticket, we need clarification on:
* *Missing Acceptance Criteria:* [Specific question on expected behavior]
* *Environment/Scope:* [Specific clarification needed]
* *Dependencies:* [Uncertain dependency]
```

---

## Phase 2: Work Kickoff

Once DoR passes or the user instructs to proceed:

### 2.1 Branch Creation & Alignment
Check local repository state and compute the standard branch name:
* **Feature:** `feat/{ticket-key-lower}-{short-kebab-summary}`
* **Bugfix:** `fix/{ticket-key-lower}-{short-kebab-summary}`
* **Infra / Chore:** `chore/{ticket-key-lower}-{short-kebab-summary}` or `{ticket-key-lower}/{short-kebab-summary}`

*Example:* `CLOUDX-5426` → `CLOUDX-5426/sync-service-upgrade` or `chore/cloudx-5426-sync-service-upgrade`

Verify local workspace:
```bash
git status
git checkout -b <branch-name>
```

### 2.2 Kickoff Status Update
Draft a starter comment for Jira:
```markdown
h4. 🚀 Work Commenced
* *Owner:* [User Name]
* *Working Branch:* [branch-name]
* *Planned Approach:*
# [Key technical step 1]
# [Key technical step 2]
# [Verification plan]
* *Target Completion:* [Estimated date/sprint]
```
*(Ask for user confirmation before posting or updating Jira status to `In Progress`)*.

---

## Phase 3: Daily Progress & WIP Updates (Keeping Tickets Updated)

When updating ticket progress, generating standup notes, or encountering blockers:

### 3.1 Inspect Git & Work State
Collect real context from git:
```bash
git status -s
git log -n 5 --oneline
git diff --stat
```

### 3.2 Draft Standardized Progress Comment
Draft a crisp, factual update:
```markdown
h4. 🔄 Progress Update - [YYYY-MM-DD]
* *Current Milestone:* [Completed subtask or refactored component]
* *Changes Made:*
  * [Key file or service updated] ([commit-sha-short])
  * [Unit tests added/updated]
* *Next Steps:*
  * [Next immediate task]
* *Blockers / External Dependencies:* [None OR explicit blocker details]
```

### 3.3 Blocker Handling Protocol
If blocked by external dependencies, missing secrets, or upstream APIs:
1. Explicitly mark the status as **BLOCKED** in the update.
2. Tag or mention the blocking owner or external team.
3. Document:
   * What is needed to unblock.
   * Business/sprint impact if delayed.
   * Workaround attempted (if any).

---

## Reference & Templates
Detailed comment templates and edge cases are located in [references/ticket-templates.md](file:///Users/khaledtaha/.claude/skills/khaled-skills/jira/references/ticket-templates.md).
