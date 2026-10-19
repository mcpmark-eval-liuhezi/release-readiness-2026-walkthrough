# Release Readiness Checklist

Use this checklist before every quarterly release cut. Each item must be verified
by the release shepherd on the day of the cut. This is a walkthrough/dry-run copy
for the release-readiness review, so all examples point at this sandbox repo.

## 1. CI status

- [ ] Verify the latest CI run on the release branch is green (`success`).
- [ ] Confirm the workflow runs on every push (no paths/branches filters that hide changes).
- [ ] Confirm a manual `workflow_dispatch` trigger exists and works.
- [ ] Re-run the release-blocking workflow once to confirm stability (no flakiness).
- [ ] No skipped or cancelled jobs in the release-blocking workflow.

## 2. Security-alert status

- [ ] Review open secret-scanning alerts; all must be resolved or explicitly accepted.
- [ ] Review open code-scanning alerts; triage each one (fix, defer with a ticket, or dismiss).
- [ ] Review open Dependabot alerts; patch critical/high severities before the cut.
- [ ] Confirm no unresolved alerts block the release gate.

## 3. Open follow-up items

- [ ] Enumerate open issues carrying the release-blocking label; confirm each has an owner.
- [ ] Confirm any deferred follow-up items are tracked as issues with target dates.
- [ ] Link the follow-up issues in the release notes so they are visible to reviewers.
- [ ] Confirm no dangling TODOs in docs that reference missing automation.

## 4. Sign-off

- [ ] Release shepherd has signed off on CI, security, and follow-ups above.
- [ ] Sign-off recorded in the release review doc with a timestamp.
