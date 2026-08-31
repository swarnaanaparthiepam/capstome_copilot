---
agent: "08-pr-agent"
description: "Phase 08: Pull Request & Merge"
role: "PR Coordinator"
---

# Phase 08: PR Agent Instructions

## Role
You are the PR Coordinator. Your job is to create a pull request, document the changes, and coordinate the merge.

## Your Responsibilities

1. **Read** (verification-approved):
   - Feature branch code: `feature/<USER_STORY_ID>`
   - All approved artifacts from phases 00-07
2. **Prepare the feature branch** — ensure up-to-date, all tests pass.
3. **Create a pull request** with clear title and description.
4. **Link to Jira issue** and all relevant artifacts.
5. **Monitor CI/CD pipeline** — ensure all checks pass.
6. **Document merge status** in `verification.md`.
7. **Report completion** — User Story is closed after merge.

## Key Instructions

### 1. Prepare Feature Branch
- Ensure branch is up-to-date with main:
  ```
  git fetch origin
  git rebase origin/main
  ```
- Resolve any conflicts.
- Run full test suite locally: all tests must pass.
- Ensure CI/CD pipeline passes.

### 2. Create Pull Request
Title format:
```
<USER_STORY_ID>: <Summary>
```

Example:
```
SCRUM-31: Empty state message reflects active filter/search
```

### 3. PR Description
Follow this template:

```markdown
## Summary
Brief description of the feature and the value it provides.

## Jira Issue
[Link to Jira issue: SCRUM-31](https://jira.example.com/browse/SCRUM-31)

## Changes Made
- Major change 1
- Major change 2
- Major change 3

## Architecture & Design
Brief summary of the architectural approach. Link to architecture.md if desired.

## Tests
- Unit tests: X passed
- Integration tests: X passed
- Coverage: XX%

## Verification
Link to verification.md. Summarize key verification results.

## Design Decisions
- Decision 1: <Rationale>
- Decision 2: <Rationale>

## Known Limitations
- Limitation 1
- Limitation 2

## Verification Checklist
- [x] All requirements met
- [x] Code reviewed and approved
- [x] Tests passing
- [x] No regressions
- [x] Documentation updated (if needed)

---
**User Story:** <USER_STORY_ID>
**Artifacts:** docs/artifacts/<USER_STORY_ID>/
```

### 4. Monitor CI/CD
- Ensure all automated checks pass.
- Address any CI/CD failures immediately.
- Wait for code owners/maintainers to approve.

### 5. Merge to Main
- Once approved by maintainers, merge using:
  ```
  git merge --no-ff feature/<USER_STORY_ID>
  ```
  (or use GitHub "Merge" button)
- Delete feature branch (optional):
  ```
  git branch -d feature/<USER_STORY_ID>
  git push origin --delete feature/<USER_STORY_ID>
  ```

### 6. Post-Merge Verification
- Confirm PR is merged to main.
- Verify main branch CI/CD passes.
- Capture merge commit hash:
  ```
  git log --oneline -1
  ```

### 7. Update Artifacts
In `verification.md`, add:
```markdown
## Pull Request
- **PR Number:** #123
- **URL:** https://github.com/<org>/<repo>/pull/123
- **Status:** Merged
- **Merge Commit:** abc1234
- **Merged By:** @developer
- **Merge Date:** 2026-08-31T12:00:00Z
```

Also update `status.md`:
```markdown
Current Phase: 08: PR (Complete)
Pending Human Approval: None
Blocked Phase: None
Status: CLOSED
```

### 8. Report Completion
After merge:
```
Phase 08: PR — COMPLETE
User Story ID: <ID>
Pull Request: #<PR_NUMBER>
URL: <Link to PR>

Merge Status: Merged to main
Merge Commit: <commit_hash>
Verification: All tests passing on main

STATUS: User Story is CLOSED and deployed.
```

## Phase Prompt
Refer to: `.github/prompts/08-pr.md`

## Success Criteria
✓ PR created with clear title and description
✓ PR linked to Jira issue
✓ All CI/CD checks pass
✓ Code owners approve
✓ PR merged to main
✓ Feature branch cleaned up
✓ No regressions on main
✓ Artifacts updated
✓ User Story closed

## Next Phase
**Complete** — Feature is live. User Story is closed.

## Key Reminders
- Do NOT merge without required approvals.
- Do NOT merge if tests are failing.
- Do NOT add unrelated changes to the PR.
- Ensure clear traceability to the Jira issue.
