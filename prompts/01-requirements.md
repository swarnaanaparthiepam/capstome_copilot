# Phase 01: Requirements Analysis

## Objective
Analyze the User Story and extract functional and non-functional requirements. Identify ambiguities and ask clarification questions ONE AT A TIME. Do not make assumptions.

## Input
- `docs/artifacts/<USER_STORY_ID>/user-story.md` (read-only)

## Process

### 1. Read the User Story
- Extract the title, description, and acceptance criteria.
- Identify explicit requirements and implicit assumptions.

### 2. Identify Ambiguities
- List all unclear, vague, or conflicting statements.
- Do NOT resolve ambiguities by guessing.

### 3. Ask Clarification Questions
- Present ONE clarification question at a time.
- Wait for human response before asking the next question.
- Record the human's answer in the artifact.
- Continue until all ambiguities are resolved.

### 4. Extract Requirements
- **Functional Requirements:** What the system must do.
  - User goals and interactions
  - System behavior
  - Input/output specifications
  - Edge cases
- **Non-Functional Requirements:** Quality attributes.
  - Performance (response time, throughput)
  - Scalability
  - Security
  - Reliability
  - Maintainability
  - Usability
  - Compliance

### 5. Record Decisions
- Document each clarification and its resolution.
- Maintain traceability to the original User Story.

### 6. Identify Out-of-Scope Items
- List requirements that are explicitly excluded.
- List dependencies on external systems or teams.

## Output
Create `docs/artifacts/<USER_STORY_ID>/requirements.md`:

```markdown
# Requirements — <USER_STORY_ID>

**Source:** docs/artifacts/<USER_STORY_ID>/user-story.md
**Analyzed:** <timestamp>

## Functional Requirements

### FR-1: <Requirement Title>
- **Description:** <Clear, unambiguous description>
- **Acceptance:** <How to verify this requirement is met>

### FR-2: ...

## Non-Functional Requirements

### NFR-1: <Requirement Title>
- **Description:** <Clear specification>
- **Metric:** <How to measure>

### NFR-2: ...

## Clarifications Requested and Resolved

### Clarification 1
- **Question:** <Original question>
- **Human Response:** <Recorded answer>
- **Resolution:** <How this affects the requirements>

### Clarification 2: ...

## Out-of-Scope
- <Item 1>
- <Item 2>

## Dependencies
- <External system or team>
- <Data source>

## Traceability
- All requirements trace to User Story <USER_STORY_ID>.
- Acceptance criteria from the User Story are incorporated as functional requirements.

---

**Status:** Ready for human approval.
```

## Approval Requirement
- **YES** — Human must review and approve the extracted requirements before Phase 02 can begin.

## Halt Conditions
**Stop and report if:**
- User Story artifact is missing.
- User Story content is unintelligible.
- Clarification cannot be obtained (human does not respond).

**Report:**
- Missing or invalid artifact
- List of outstanding clarifications
- Recommended recovery

## No-Assumption Rule
- Do NOT guess the answer to an ambiguous requirement.
- Do NOT proceed without clarification.
- Do NOT fill gaps with standard assumptions (e.g., "assume REST API").

## Key Questions to Ask

Typical clarifications:
1. What are the primary users and their goals?
2. What are the success criteria for this feature?
3. Are there performance or scalability constraints?
4. What data must be persistent, and for how long?
5. What security or compliance requirements apply?
6. What are the integration points with other systems?
7. What happens in error scenarios?

Ask only the questions needed to resolve ambiguities in the User Story.

## Next Phase
Phase 02: Architecture Design (awaiting human approval)
