# Release Readiness Walkthrough — Status Report

**Prepared for the quarterly release-readiness review (Friday)**

> Note: this report was intended for a GitHub gist, but gist creation was blocked by the
> authenticated token's scope (`403 Resource not accessible by personal access token`).
> It is committed here instead so the review doc has a durable, linkable artifact.

---

## ⚠️ Setup deviations you should know about first

Two constraints in the authenticated token forced deviations from the original plan:

1. **Repository location/name.** Repo creation in the personal account (`HeziLiu`) failed with a persistent `403 Resource not accessible by personal access token`. An org (`mcpmark-eval-liuhezi`) is accessible, but the name `release-readiness-2026` was **already taken there by a live repo** (private, with an open "Sprint 0 launch checklist" tracker and sub-issues). To keep this walkthrough on genuinely clean ground, the repo was created as:
   - **`mcpmark-eval-liuhezi/release-readiness-2026-walkthrough`** (public, README-initialized)
   - https://github.com/mcpmark-eval-liuhezi/release-readiness-2026-walkthrough

2. **Two security/notification checks could not be performed** (same 403 token error) — see the relevant sections below.

---

## 1. Repository

- **Repo:** `mcpmark-eval-liuhezi/release-readiness-2026-walkthrough`
- **Visibility:** public
- **Default branch:** `main`
- **URL:** https://github.com/mcpmark-eval-liuhezi/release-readiness-2026-walkthrough
- **Contents:** README (initial), `.github/workflows/ci.yml`, `docs/release-checklist.md`, this report

## 2. CI workflow

`.github/workflows/ci.yml` on `main` — a minimal smoke check (checkout + `test -f README.md`).

- **Triggers:** `push` (every push, no filters) and `workflow_dispatch` (manual)
- **Pre-trigger verification:** file content confirmed to contain both triggers **and** the workflow was confirmed registered under the repo's Actions tab ("CI", workflow id `389535912`, state `active`) before anything was fired.

### Workflow runs (as returned by GitHub)

| # | Run ID | Event | Attempt | Status | Conclusion |
|---|--------|-------|---------|--------|------------|
| 1 | `39273465354` | `workflow_dispatch` (manual start) | 1 | `completed` | `success` |
| 2 | `39273465354` (same run, re-run) | `workflow_dispatch` | 2 | `completed` | `success` |

- Manual run link: https://github.com/mcpmark-eval-liuhezi/release-readiness-2026-walkthrough/actions/runs/39273465354
- The run was green on both the initial dispatch and the deliberate re-run (attempt 2) — **no flakiness, stable**. No fix/retry loop was needed because it passed first time.

### Related runs (context)

- `39273373939` — `push` run on `main` for the workflow-addition commit: `completed` / `success`
- `39274003222` — `push` run on the PR branch `add-release-checklist`: `completed` / `success`
- `39275598131` — `push` run on `main` for the squash-merge commit: `completed` / `success`

## 3. Pull request — release checklist

- **PR #1: "Add release readiness checklist"**
- https://github.com/mcpmark-eval-liuhezi/release-readiness-2026-walkthrough/pull/1
- **Branch:** `add-release-checklist` → `main`
- **File:** `docs/release-checklist.md` (32 lines added), covering:
  - **CI status** — green run, push-trigger coverage, `workflow_dispatch` works, re-run stability, no skipped/cancelled jobs
  - **Security-alert status** — secret-scanning, code-scanning, Dependabot alerts triaged before the cut
  - **Open follow-up items** — release-blocking issues have owners, deferred items tracked with dates, follow-ups linked in release notes
  - Sign-off section
- **Review comment** (submitted review, COMMENT event) on `docs/release-checklist.md` line 15 (RIGHT side, in the "Security-alert status" section):

  > "Good coverage here. One suggestion for the review walkthrough: under "Security-alert status", it would help to note the difference between secret-scanning, code-scanning, and Dependabot alerts explicitly on screen — the team tends to conflate them. Also worth calling out that a clean (empty) secret-scanning list still needs to be checked and recorded, not skipped."

  Comment URL: https://github.com/mcpmark-eval-liuhezi/release-readiness-2026-walkthrough/pull/1#discussion_r4315448303
- **Merge:** squash-merged on 2026-10-19, merge commit `cbabe5e2ec33edd8ec5cadec1aa7b526b67b4aa3`, "Pull Request successfully merged". Both the review comment and the merge are visible in the PR's review history / conversation for screen-sharing.

## 4. Follow-up issue + Copilot

- **Issue #2: "Automate dependency audit in CI"**
- https://github.com/mcpmark-eval-liuhezi/release-readiness-2026-walkthrough/issues/2
- Describes the manual dependency-audit step in the checklist that should be automated: a CI job that fails the build on open critical/high alerts, reports medium/low without failing, and replaces the manual audit note in `docs/release-checklist.md`.
- **Copilot assignment result (exactly as returned):**

  > `copilot isn't available as an assignee for this issue. Please inform the user to visit https://docs.github.com/en/copilot/using-github-copilot/using-copilot-coding-agent-to-work-on-tasks/about-assigning-tasks-to-copilot for more information.`

  The assignment was attempted twice with the identical result. **The issue is NOT assigned to Copilot** — Copilot is not available as an assignee for this repo/issue. Likely cause: the Copilot coding agent not enabled for this org/repo, or not entitled on this account. The issue remains open and unassigned, ready to hand off once access is sorted.

## 5. Secret-scanning alerts

**Could not be checked.** The API call failed with a persistent `403 Resource not accessible by personal access token`. So this is *not* a confirmed "empty list" — it is simply unverified. Please check manually at:
https://github.com/mcpmark-eval-liuhezi/release-readiness-2026-walkthrough/security/secret-scanning

## 6. Notifications

**Could not be checked or cleared.** Listing notifications also failed with `403 Resource not accessible by personal access token`, so no threads related to this repo or the Copilot work could be identified or marked as read. Please review manually at https://github.com/notifications.

---

## Summary

| Item | Status |
|------|--------|
| Repository created | ✅ (org, renamed — see deviations) |
| CI workflow with both triggers | ✅ verified before firing |
| Manual dispatch green | ✅ run `39273465354`, `success` |
| Re-run stable | ✅ attempt 2, `success` |
| Checklist PR + review comment + squash merge | ✅ PR #1, comment visible, merged |
| Follow-up issue | ✅ issue #2 created |
| Copilot assignment | ❌ failed — not an available assignee |
| Secret-scanning check | ❌ blocked by token scope (403) |
| Notifications review/clear | ❌ blocked by token scope (403) |
| Gist report | ❌ blocked by token scope (403) — committed to repo instead |

*Report generated 2026-10-19.*
