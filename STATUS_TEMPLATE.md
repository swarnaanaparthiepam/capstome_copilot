# Agentic SDLC Framework — Status Template

Use this template to track a User Story as it progresses through the SDLC phases.

**Location:** `docs/artifacts/<USER_STORY_ID>/status.md`

---

# User Story: <USER_STORY_ID>

**Title:** <Summary from user-story.md>

**Jira URL:** [<USER_STORY_ID>](<link to Jira issue>)

## Current Status

**Current Phase:** <PHASE_NUMBER>: <PHASE_NAME>

**Status:** In Progress (or Complete, or Blocked)

## Phase Completion Checklist

- [ ] 00: Input — Retrieve User Story from Jira
- [ ] 01: Requirements — Extract and clarify requirements
- [ ] 02: Architecture — Design high-level solution
- [ ] 03: Design Review — Validate architecture
- [ ] 04: Planning — Create implementation plan
- [ ] 05: Implementation — Write code and tests
- [ ] 06: Review — Peer review code
- [ ] 07: Verification — Test and validate
- [ ] 08: PR — Create and merge pull request

## Approval Gates

**Pending Human Approval:** 
- [ ] Phase 00: Input
- [ ] Phase 01: Requirements
- [ ] Phase 02: Architecture
- [ ] Phase 03: Design Review
- [ ] Phase 04: Planning
- [ ] Phase 06: Code Review
- [ ] Phase 07: Verification

(Mark which phase is awaiting approval, if any)

## Blocked Phases

**Blocked Phase:** None

(If a phase is blocked, document:)
- **Phase:** <PHASE_NUMBER>: <PHASE_NAME>
- **Reason:** <Why the phase is blocked>
- **Recommended Recovery:** <Steps to unblock>

## Important Dates

- **User Story Created:** <Date in Jira>
- **Input Phase Started:** <Timestamp>
- **Requirements Approved:** <Timestamp>
- **Architecture Approved:** <Timestamp>
- **Implementation Started:** <Timestamp>
- **Code Merged:** <Timestamp>
- **Expected Release:** <Date or milestone>

## Artifacts

| Phase | Artifact | Status | Last Updated |
|-------|----------|--------|--------------|
| 00 | `user-story.md` | Created | <Timestamp> |
| 01 | `requirements.md` | <Ready/Pending Approval> | <Timestamp> |
| 02 | `architecture.md` | <Ready/Pending Approval> | <Timestamp> |
| 03 | `design-review.md` | <Ready/Pending Approval> | <Timestamp> |
| 04 | `impl-plan.md` | <Ready/Pending Approval> | <Timestamp> |
| 05 | Feature branch code | <In Progress/Complete> | <Timestamp> |
| 06 | `review.md` | <Ready/Pending Approval> | <Timestamp> |
| 07 | `verification.md` | <Ready/Pending Approval> | <Timestamp> |
| 08 | Pull Request #XXX | <Merged/Pending> | <Timestamp> |

## Progress Notes

### Phase 00: Input
- User Story retrieved from Jira successfully.
- [Any clarifications or notes]

### Phase 01: Requirements
- X clarification questions asked and resolved.
- [Any ambiguities that were resolved]

### Phase 02: Architecture
- Architecture designed with X components.
- Technology stack: [Language/Framework choices]

### Phase 03: Design Review
- X critical issues identified and resolved.
- [Key changes to architecture]

### Phase 04: Planning
- Implementation plan created with X tasks.
- Estimated effort: [Duration estimate]
- Critical path: [Task sequence]

### Phase 05: Implementation
- Feature branch: `feature/<USER_STORY_ID>`
- X tasks completed.
- Code coverage: XX%

### Phase 06: Code Review
- X issues found and addressed.
- Code approved by: [@reviewer]

### Phase 07: Verification
- All functional requirements verified: PASS
- All non-functional requirements verified: PASS
- Test coverage: XX%

### Phase 08: PR
- Pull Request: #XXX
- Merged by: [@merger]
- Merge commit: [commit hash]

## Known Issues or Limitations

- [Issue 1: Description]
- [Issue 2: Description]

(Document any known limitations or deferred work)

## Stakeholders

- **Product Owner:** [Name]
- **Architect:** [Name]
- **Lead Developer:** [Name]
- **QA/Tester:** [Name]

## Next Steps

[What needs to happen next, if any]

---

**Last Updated:** <TIMESTAMP>
**Next Review:** <DATE>
