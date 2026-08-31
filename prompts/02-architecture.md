# Phase 02: Architecture Design

## Objective
Design a high-level solution architecture that addresses all approved requirements. Identify components, responsibilities, data flows, external integrations, and technology choices.

## Input
- `docs/artifacts/<USER_STORY_ID>/requirements.md` (read-only, must be approved)
- `docs/artifacts/<USER_STORY_ID>/user-story.md` (read-only, for context)

## Process

### 1. Review Approved Requirements
- Read all functional and non-functional requirements.
- Identify critical constraints and dependencies.

### 2. Design High-Level Architecture
- Define the major components and their responsibilities.
- Map data flows and interactions.
- Identify external integrations (APIs, databases, services).
- Recommend technology stack where applicable.

### 3. Address Non-Functional Requirements
- How does the architecture support scalability?
- How are security and authentication handled?
- What is the data persistence strategy?
- How is error handling and recovery designed?
- What monitoring and observability are built in?

### 4. Identify Risks and Mitigations
- List architectural risks (single points of failure, performance bottlenecks, security gaps).
- Propose mitigation strategies.

### 5. Document the Architecture
- Use clear diagrams (ASCII or reference to visual format).
- Describe component interactions.
- Define interfaces and contracts.

## Output
Create `docs/artifacts/<USER_STORY_ID>/architecture.md`:

```markdown
# Architecture — <USER_STORY_ID>

**Based on:** docs/artifacts/<USER_STORY_ID>/requirements.md
**Designed:** <timestamp>

## High-Level Architecture Overview

[Diagram or clear text description of overall system structure]

## Components

### Component 1: <Name>
- **Responsibility:** <What it does>
- **Inputs:** <What it receives>
- **Outputs:** <What it produces>
- **Technology:** <Language, framework, library>

### Component 2: ...

## Data Flow

[Sequence or flow diagram describing how data moves through components]

## External Integrations

### Integration 1: <System Name>
- **Purpose:** <Why this integration is needed>
- **Protocol:** <REST, gRPC, message queue, etc.>
- **Data Exchanged:** <What flows in/out>

### Integration 2: ...

## Technology Recommendations

- **Backend:** <Language/framework>
- **Frontend:** <If applicable>
- **Database:** <Type and rationale>
- **Infrastructure:** <Deployment model>
- **Security:** <Auth mechanism, encryption>

## Non-Functional Requirements Addressed

- **Scalability:** <How the architecture scales>
- **Performance:** <Expected latency, throughput>
- **Reliability:** <Failover, redundancy, recovery>
- **Security:** <Authentication, authorization, encryption>
- **Maintainability:** <Modularity, testability, documentation>

## Identified Risks and Mitigations

| Risk | Severity | Mitigation |
|------|----------|-----------|
| <Risk description> | High/Medium/Low | <Mitigation strategy> |
| <Risk description> | ... | ... |

## Assumptions

- <Assumption 1>
- <Assumption 2>

## Open Questions or Decisions Pending
- <Any architectural decision deferred to design review>
- <Dependencies on external approvals>

---

**Status:** Ready for human review and design approval.
```

## Approval Requirement
- **YES** — Human must review and approve the architecture before Phase 03: Design Review.

## Halt Conditions
**Stop and report if:**
- Requirements artifact is missing or not approved.
- Architecture cannot address all approved requirements.
- Critical architectural decisions cannot be made without additional information.

**Report:**
- Missing or unapproved requirement
- Architectural gap or blocker
- Decision pending (e.g., "Awaiting decision on database: SQL vs NoSQL")

## Key Responsibilities

The architect MUST:
- Address every approved requirement.
- Justify technology choices.
- Identify and mitigate risks.
- Maintain clarity for the design review phase.

The architect MUST NOT:
- Add requirements not in the approved requirements.md.
- Assume design details that belong in later phases.
- Skip architectural components.

## Next Phase
Phase 03: Design Review (awaiting human approval)
