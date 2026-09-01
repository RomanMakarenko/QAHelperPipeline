# Test Strategy

## Analysis status

**Status**: Blocked — no executable scenario records or confirmed application implementation are available in the current repository.

**Scope**: Question/TBD. Analyze the feature named by the `test-strategy` argument when supplied; otherwise analyze all confirmed scenario records.

This document is the generated strategy contract for `docs/test-scenarios.md`. It records strategy design separately from test execution. The current empty state must not be interpreted as a healthy pyramid, complete coverage, or absence of defects.

## Input and source inventory

| Source ID | Source type | Path or reference | Available evidence | Status |
| --- | --- | --- | --- | --- |
| `SRC-001` | Scenario input | `docs/test-scenarios.md` | Empty-state contract; no executable `TC-*` records | Confirmed |
| `SRC-002` | Technique selector | `.claude/skills/choose-technique/SKILL.md` | Routing and evidence conventions | Confirmed |
| `SRC-003` | Technique guides/skills | `docs/TestDesignAndSoftwareTestingTechniques/` and `.claude/skills/` | Test-design methods, not application architecture | Confirmed |
| `SRC-004` | Application implementation | No confirmed product source discovered | Functions, services, contracts, persistence, and UI harness unavailable | Question/TBD |
| `SRC-005` | Existing tests | No confirmed product test suite discovered | Existing assignments and anti-pattern evidence unavailable | Question/TBD |

## Pyramid principles

Assign the lowest layer that can observe the scenario’s required oracle and boundary:

| Layer | Primary focus | Typical evidence required |
| --- | --- | --- |
| Unit | Pure deterministic logic without I/O or external dependencies | Function/module and unit-test harness |
| API/Integration | Service rules, validators, serializers, persistence, authorization, events, and API contracts | Endpoint/service contract and integration harness |
| Component | Isolated rendering, interaction, validation display, loading/empty/error state, and local UI state | Component and rendering harness |
| E2E | Confirmed multi-page, full-stack, browser, external-integration, or critical journey wiring | Running application, environment, and browser/integration harness |

Push pure calculations and validation down when isolation still proves the oracle. Retain E2E only for confirmed cross-boundary behavior or critical user goals. Use defense-in-depth when a higher-level case verifies a distinct integration boundary rather than repeating a lower-level assertion.

## Distribution

No executable scenarios are currently available. Counts are inventory facts, not quality claims.

| Layer | Assigned scenario count | Percentage of executable scenarios | Main focus | Expected time | Evidence status |
| --- | ---: | ---: | --- | --- | --- |
| Unit | 0 | N/A (denominator 0) | Pure deterministic behavior | Unknown | No application source |
| API/Integration | 0 | N/A (denominator 0) | Service and contract behavior | Unknown | No application source |
| Component | 0 | N/A (denominator 0) | Isolated UI behavior | Unknown | No UI source or harness |
| E2E | 0 | N/A (denominator 0) | Cross-boundary journeys | Unknown | No application source or harness |

## Scenario-to-layer assignments

No confirmed executable scenarios can be assigned. Do not fabricate source references or layer decisions from scenario categories alone.

| Scenario ID | Category | Priority | Assigned Layer | Primary/Defense-in-Depth | Source/function/endpoint/component/test reference | Rationale | Lower-layer alternative | Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| — | — | — | Pending / Question-TBD | — | Not found in repository | No executable scenario records or exact oracles | Cannot assess | Pending |

For future records, preserve each `TC-*` ID and include the requirement/source references, exact oracle, actual discovered symbol/path, rationale, lower-layer alternative, status, and residual risk. The scenario’s Suggested Layer is not authoritative.

## Contested decisions

No contested decisions are assessable without scenario records. For each future contested assignment, state:

1. The required oracle and cross-boundary behavior.
2. The lowest candidate layer and its harness.
3. Why a lower layer is insufficient, if applicable.
4. Why a higher layer adds distinct risk coverage.
5. Whether the recommendation is Confirmed, Assumption, or Question/TBD.

## Defense-in-depth

No confirmed critical rules or flows are available, so no defense-in-depth pair is recommended. Once critical rules are evidenced, consider a lower-layer contract test plus a narrowly scoped higher-layer confirmation of wiring, persistence, or customer-visible behavior. Do not count identical assertions at multiple layers as independent coverage without a distinct purpose.

## Existing-test anti-pattern analysis

**Status**: Not assessable — no confirmed application test suite was discovered.

The following checks must be performed when tests become available:

- validation or pure logic tested only through E2E;
- API status and response contracts tested only through E2E;
- every scenario assigned to E2E;
- no E2E confirmation for a confirmed critical journey;
- duplicate multi-layer cases without distinct boundaries;
- assignments unsupported by a discovered harness or source;
- lower-layer candidates left at higher layers;
- critical rules missing justified defense-in-depth.

Absence of a discovered test suite does not prove that these anti-patterns are absent.

## Missing evidence and residual risks

| ID | Category | Missing evidence or risk | Required follow-up | Status |
| --- | --- | --- | --- | --- |
| `CLAR-001` | Question/TBD | Confirmed scenario records with exact oracles and source/rule references | Populate `docs/test-scenarios.md` from approved evidence | Open |
| `CLAR-002` | Question/TBD | Application modules, contracts, UI harnesses, test fixtures, and existing tests | Provide paths or add implementation sources | Open |
| `CLAR-003` | Question/TBD | Critical business journeys and rules requiring cross-layer confirmation | Identify impact and risk owners | Open |
| `RISK-001` | Residual risk | No layer suitability, pyramid balance, execution timing, or anti-pattern conclusion can be confirmed | Re-run after scenario and source inventory exists | Open |

## Coverage and limitations

Keep these denominators separate when the strategy is populated:

- validated scenario records / supplied scenario records;
- layer-assigned executable scenarios / executable scenarios;
- scenarios with discovered source references / executable scenarios;
- defense-in-depth recommendations / confirmed critical rules;
- executed scenarios by layer / assigned executable scenarios;
- specialized security and non-functional follow-up coverage / selected follow-ups.

A 100% assignment rate means only that every supplied executable record has a layer decision. It does not prove complete requirements, branch, state, interaction, security, performance, reliability, accessibility, compatibility, usability, or defect coverage.

## Verification checklist

- [ ] Scope and scenario input status are explicit.
- [ ] Actual repository evidence and unavailable sources are recorded.
- [ ] Scenario IDs, categories, references, statuses, priorities, and exact oracles are validated.
- [ ] Each confirmed assignment uses the lowest adequate layer.
- [ ] Unit, API/Integration, Component, and E2E decisions cite a real boundary or harness.
- [ ] Suggested layers are reviewed rather than copied.
- [ ] Contested decisions and lower-layer alternatives are explained.
- [ ] Defense-in-depth has distinct purposes.
- [ ] Distribution counts, percentages, denominators, and time estimates are accurate or explicitly unknown.
- [ ] Existing-test anti-patterns are evidence-based; no tests means Not assessable.
- [ ] Questions, assumptions, gaps, exclusions, and residual risks are visible.
- [ ] Specialized follow-ups are not implied by ordinary pyramid assignment.
