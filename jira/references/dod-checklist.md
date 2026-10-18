# Definition of Done (DoD) Comprehensive Checklist

This reference document outlines the strict quality, testing, operational, and documentation criteria required before any engineering or DevOps task is marked as **Done** or **Closed**.

---

## 1. Code Quality & Version Control
- [ ] **Branch Merged**: All commits are merged to the designated target branch (`main`, `master`, or release branch) via a reviewed Pull/Merge Request.
- [ ] **Pipeline Green**: The CI/CD build, automated linting, security scans (SonarQube/Checkmarx/Trivy), and test suites completed with zero fatal failures.
- [ ] **Peer Review**: At least required approvals obtained; all reviewer threads and comments marked resolved.
- [ ] **Clean History**: Commit messages follow standard format and explicitly mention the Jira key (e.g. `feat(sync): CLOUDX-1234 add retry backoff logic`).

---

## 2. Automated & Manual Verification
- [ ] **Unit Tests**: Core business logic and regression edge cases covered with automated tests.
- [ ] **Integration Tests**: Service boundaries, database transactions, or external HTTP clients tested with mock or ephemeral dependencies.
- [ ] **Manual Testing Proof**:
  - Positive flow executed and documented (cURL, request/response JSON payload, UI screenshot).
  - Negative/error flow executed (400/404/500 handling, invalid input validation, timeout).
- [ ] **No Regressions**: Existing smoke test suite or regression pass succeeded.

---

## 3. Infrastructure, Config & Security
- [ ] **No Hardcoded Secrets**: Zero secrets, tokens, API keys, or database credentials committed to git.
- [ ] **Config / Secrets Documented**: Any new environment variable or KeyVault secret documented with description, expected format, and default value.
- [ ] **IaC Consistency**: Infrastructure changes implemented via Terraform / Helm / Kubernetes manifests, not manual click-ops.

---

## 4. Documentation & Operational Readiness
- [ ] **Readme / Confluence Updated**: If APIs, dependencies, or service behavior changed, the repository `README.md` or Confluence architecture doc is updated.
- [ ] **Observability & Alerts**: New endpoints or worker loops have logging, OpenTelemetry traces, and error alerts configured in Grafana / Datadog.
- [ ] **Change Management (CM)**: If modifying production infrastructure, a Change Management Confluence page is filed, approved, and linked.
- [ ] **Rollback Plan**: Rollback steps (revert MR, deploy previous image, run rollback migration) documented.
