# Test Scenarios

## Generation status

**Status**: Blocked — no confirmed application-domain flows or product requirements are available in the current repository.

**Scope**: Question/TBD. A feature name may be supplied to `create-scenarios`; a blank argument requests the broadest scope supported by confirmed evidence.

**Executable scenarios**: 0. This is an evidence-inventory state, not a claim that the application has no behavior or defects.

This document is the generated-artifact contract consumed by `test-strategy`. It is intentionally free of fictional test cases. Populate it only from user-provided requirements, approved specifications, observed implementation behavior, or other cited evidence.

## Source inventory

| Source ID | Source type | Path or reference | Observed content | Status |
| --- | --- | --- | --- | --- |
| `SRC-001` | Existing methodology | `docs/TestDesignAndSoftwareTestingTechniques/chooseTechnique.md` | Technique-selection guidance only; not application behavior | Confirmed |
| `SRC-002` | Existing project skill | `.claude/skills/choose-technique/SKILL.md` | Technique-routing and traceability conventions only | Confirmed |
| `SRC-003` | Application requirements | Not supplied | Product rules, actors, or acceptance criteria unavailable | Question/TBD |
| `SRC-004` | Application implementation | No confirmed product source discovered | Endpoints, functions, components, and persistence behavior unavailable | Question/TBD |
| `SRC-005` | Existing application tests | No confirmed product test suite discovered | Historical cases and execution evidence unavailable | Question/TBD |

Methodology sources must not be treated as product requirements. Add each future source with a stable ID, exact path/location, version or date when available, observed fact, and status.

## Flow and rule inventory

No confirmed `FLOW-*` or `RULE-*` records exist yet. Do not infer flows or rules from the names of testing techniques.

| Flow ID | Feature or operation | Source references | Scope/status |
| --- | --- | --- | --- |
| — | No confirmed application flow available | `SRC-003`, `SRC-004` | Question/TBD |

## Six-lens applicability

All six lenses must be considered for each confirmed flow. They are currently not assessable because no confirmed flow or product oracle is available.

| Category | Required ID range | Applicability | Covered scenarios | Evidence gap |
| --- | --- | --- | ---: | --- |
| Happy Path | `TC-001`–`TC-099` | Not assessable | 0 | No confirmed successful goal or completion oracle |
| Business Rules | `TC-100`–`TC-199` | Not assessable | 0 | No confirmed rules, conditions, calculations, or side effects |
| Security | `TC-200`–`TC-299` | Not assessable | 0 | No confirmed actors, permissions, data boundaries, or security contract |
| Negative/Error | `TC-300`–`TC-399` | Not assessable | 0 | No confirmed invalid inputs or failure/recovery oracle |
| Edge Cases | `TC-400`–`TC-499` | Not assessable | 0 | No confirmed limits, representations, timing, or capacity values |
| UI State | `TC-500`–`TC-599` | Not assessable | 0 | No confirmed UI, rendering, interaction, or accessibility behavior |

## Scenario records

No `TC-*` records are populated. Do not add a placeholder scenario merely to satisfy a numeric range.

Use the following record for future confirmed or explicitly provisional scenarios:

```markdown
### TC-<NNN>: <specific objective>
**Category**: <Happy Path | Business Rules | Security | Negative/Error | Edge Cases | UI State>
**Status**: <Confirmed | Assumption | Question/TBD | Residual risk>
**Priority**: <P0 | P1 | P2 | P3 | Question/TBD>
**Flow/Requirement References**: <FLOW-*, REQ-*, acceptance criterion, or source location>
**Source/Rule References**: <SRC-* and RULE-* IDs>
**Preconditions**: <actor, permissions, data, state, dependencies, and setup>
**Test Data/Context**: <complete inputs, environment, locale, clock, timezone, flags, and integration state>
**Steps**:
1. <deterministic action>
2. <deterministic action>
**Expected Results**: <exact observable oracle, including state, persistence, side effects, and counts>
**Business Rule**: <confirmed rule, or explicitly labelled Assumption/Question/TBD>
**Suggested Layer**: <Unit | API/Integration | Component | E2E | Question/TBD>
**Technique Tags**: <EP | BVA | Decision Table | Pairwise | State-Transition | Error Guessing | Scenario>
**Evidence to Capture**: <response, record, screenshot, event, log, or other permitted artifact>
**Cleanup/Repeatability**: <reset, teardown, seed, timing, and reproducibility notes>
**Assumptions/Residual Risks**: <explicit limitations>
```

Assign IDs by category without collisions:

- `TC-001`–`TC-099`: Happy Path
- `TC-100`–`TC-199`: Business Rules
- `TC-200`–`TC-299`: Security
- `TC-300`–`TC-399`: Negative
- `TC-400`–`TC-499`: Edge Cases
- `TC-500`–`TC-599`: UI State

An exact oracle must identify an observable status, value, message, state, persistence result, event, notification, audit record, invocation, side-effect count, or recovery outcome. If the expected outcome is not specified, use `Question/TBD`; do not replace it with a generic success claim.

## Assumptions, questions, and residual risks

| ID | Category | Item | Follow-up |
| --- | --- | --- | --- |
| `CLAR-001` | Question/TBD | What feature, requirements, acceptance criteria, actors, and business goal are in scope? | Supply approved product evidence |
| `CLAR-002` | Question/TBD | What exact observable outcomes and failure or recovery behavior are required? | Define status, message, state, persistence, and side-effect oracles |
| `CLAR-003` | Question/TBD | Which implementation, API, UI, data, environment, and test-harness sources should be inspected? | Provide paths or references, or approve repository discovery |
| `RISK-001` | Residual risk | Security, limits, lifecycle, integration, UI-state, and non-functional behavior cannot be assessed without product evidence | Reassess after sources are added |

## Coverage and limitations

Coverage denominators are undefined while there are no confirmed flows or executable scenario IDs:

| Metric | Numerator | Denominator | Result | Limitation |
| --- | ---: | ---: | --- | --- |
| Confirmed flow representation | 0 | Undefined | N/A | No confirmed flows |
| Executable scenario design coverage | 0 | Undefined | N/A | No executable scenario inventory |
| Six-lens coverage | 0 | Undefined | N/A | Lens applicability is not assessable |
| Execution coverage | 0 | Undefined | N/A | No execution evidence |

These metrics do not establish completeness, absence of defects, or coverage of values, branches, states, interactions, security, performance, accessibility, compatibility, reliability, usability, or exploratory behavior.

## Handoff to test-strategy

`test-strategy` should consume this document after confirmed scenario records are added. It must validate IDs and oracles, discover actual source and test harnesses, and assign the lowest adequate Unit, API/Integration, Component, or E2E layer. With the current empty state, assignments remain Pending/Question/TBD and execution time is Unknown.

## Verification checklist

- [ ] Every future scenario has a stable ID in the correct category range.
- [ ] Every scenario traces to a source, rule, requirement, observed behavior, or labelled assumption.
- [ ] All six lenses are considered for each confirmed flow.
- [ ] Preconditions, complete data/context, steps, exact oracle, priority, layer proposal, evidence, cleanup, and risks are present.
- [ ] Unknown behavior is not presented as confirmed.
- [ ] Design, execution, and specialized non-functional coverage remain separate.
- [ ] Duplicate, blocked, unresolved, excluded, and residual-risk items are visible.
