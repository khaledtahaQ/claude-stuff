# Jira Ticket Comment & Communication Templates

Standardized templates for communicating ticket status, kickoffs, blockers, QA handoffs, and resolutions.

---

## 1. Ticket Kickoff Template
Use when transitioning a ticket to `In Progress`:

```markdown
h4. 🚀 Work Kickoff: [TICKET-KEY]
* *Engineer:* [Your Name]
* *Branch:* `{branch-name}`
* *Scope Summary:* [1-2 sentences on what will be changed]

h5. Planned Steps:
# [Step 1: Code/Infra change]
# [Step 2: Automated testing & local verification]
# [Step 3: MR creation and peer review]

* *Target PR Date:* [YYYY-MM-DD]
```

---

## 2. Daily Standup & WIP Progress Update
Use for daily updates or when keeping stakeholders informed:

```markdown
h4. 🔄 WIP Update - [YYYY-MM-DD]
* *Status:* On Track / Blocked / In Review
* *Completed Today:*
** [Milestone / Commit SHA: feat: implemented token cache]
** [Added unit tests covering cache expiry]
* *Next Up:*
** [Connect endpoint to downstream service]
* *Blockers / Waiting on:* [None / Waiting on DB migration approval from @username]
```

---

## 3. Blocker Escalation Template
Use when an issue is blocked by an external dependency or permission:

```markdown
h4. 🚨 WORK BLOCKED - [TICKET-KEY]
* *Blocked Since:* [YYYY-MM-DD]
* *Blocking Item:* [e.g., Missing Azure KeyVault read permissions in staging]
* *Action Required By:* [@Owner / @TeamName]
* *Impact:* Blocks deployment to staging environment and end-to-end testing.
* *Attempted Resolution:* [e.g., Requested access in IT ticket #12345]
```

---

## 4. QA / Verification Handoff Template
Use when handing off to QA or requesting peer verification:

```markdown
h4. 🧪 Ready for QA / Verification
* *MR Link:* [GitLab MR !XXX]
* *Deploy Environment:* [Dev / Staging / Ephemeral Env: `env-name`]

h5. Verification Steps:
# Send POST request to `/api/v1/sync` with payload:
{code:json}
{
  "syncId": "test-123",
  "mode": "incremental"
}
{code}
# Expect HTTP 200 with response `{ "status": "queued" }`.
# Check Grafana logs in namespace `r365-sync` for trace ID.

h5. Edge Cases Tested:
* Invalid payload returns HTTP 400.
* Token expiry automatically triggers refresh.
```

---

## 5. Resolution Summary (Definition of Done)
Use when closing or resolving a ticket:

```markdown
h3. ✅ Definition of Done - Resolution Summary
* *Resolved By:* [Your Name]
* *Resolution Date:* [YYYY-MM-DD]

h4. 1. Summary of Changes
[1-2 paragraphs detailing the root cause and the implemented solution]

h4. 2. Artifacts & Links
* *GitLab MR:* [Link to merged MR]
* *Default Branch Pipeline:* [Link to passing pipeline]
* *Confluence / CM:* [Link to documentation or Change Management page]

h4. 3. Evidence of Verification
* Automated test results: [e.g., 22 passed, 0 failed]
* Manual verification evidence attached/pasted above.
```
