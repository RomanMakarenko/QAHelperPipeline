---
name: test-strategy
description: Analyze functional test scenarios and assign each to the lowest adequate Unit, API/Integration, Component, or E2E test-pyramid layer using repository evidence, risk, and traceability.
version: 0.1.0
disable-model-invocation: true
argument-hint: "[feature-name or blank for full analysis]"
---

# Test Strategy and Pyramid Analyst

Use this skill after scenarios have been produced, or when the user supplies an equivalent scenario list. `$ARGUMENTS` scopes analysis to a feature or flow; with no argument, analyze all available scenarios. In direct/shared mode, the primary input is `docs/test-scenarios.md` and the default output is `docs/test-strategy.md`. In an orchestrated ticket run, use the explicit run-local input and output supplied by `test-pipeline`; do not read or mutate unrelated shared documents. `disable-model-invocation: true` intentionally keeps this strategy analysis as an explicit, reviewable hand-off; the orchestrator applies this contract directly when coordinating a run.

This skill assigns test layers; it does not execute tests or rewrite scenario definitions. The `Suggested Layer` in a scenario is an input hypothesis and must be reviewed, not accepted automatically.

## Evidence and repository discovery

Read and inventory, in this order:

1. `docs/test-scenarios.md`.
2. User-provided requirements, acceptance criteria, contracts, and domain references.
3. Repository documentation and specifications relevant to the scope.
4. Existing project-local technique guides and skills, including `choose-technique`.
5. Implementation files discovered anywhere in the repository.
6. Test files, fixtures, harnesses, configuration, and recorded results discovered anywhere in the repository.

Search actual repository paths. Never copy an application-specific architecture into the strategy. A missing source is evidence unavailable, not permission to invent a function, endpoint, component, fixture, test path, or browser harness. If `docs/test-scenarios.md` is absent or contains no executable records, report the analysis as blocked or provisional.

Use these labels:

- **Confirmed** — directly supported by a scenario, requirement, source file, test, or other cited evidence.
- **Assumption** — provisional interpretation used for a recommendation.
- **Question/TBD** — information needed before a layer can be assigned safely.
- **Residual risk** — behavior or evidence not covered by this strategy.
- **Pending** — a layer decision awaits an oracle, source, or executable scenario.

## Layer decision model

Choose the lowest layer that adequately observes the behavior. “Lower” means faster and more isolated only when it still verifies the required contract and risk.

| Layer | Assign when it can verify | Do not assign as the sole layer when |
| --- | --- | --- |
| Unit | A deterministic pure function, parser, calculation, mapper, or rule with no I/O, persistence, clock, network, filesystem, framework, or external dependency is the behavior under test. | The required outcome depends on a real boundary, serialization, persistence, authorization context, or integration contract. |
| API/Integration | A service, handler, validator, serializer, persistence interaction, authorization boundary, event contract, or API status/body/side-effect is the observable behavior. | The scenario is only isolated presentation/state, or the required goal spans multiple user-facing pages and boundaries. |
| Component | An isolated UI component’s rendering, interaction, validation display, loading/empty/error state, accessibility behavior, or local state can be mounted with a controlled harness. | The scenario requires real routing, multiple pages, full application wiring, or external systems. |
| E2E | A confirmed multi-page, browser, full-stack, external-integration, or critical business journey must be verified across boundaries. | A lower layer can prove the same behavior and no cross-boundary wiring is at risk. |

Decision order:

1. Identify the scenario’s required oracle and boundaries, not its title or suggested layer.
2. Find the lowest layer with a real harness and source evidence that can observe that oracle.
3. Push validation, calculations, mappings, and contracts down when isolation is adequate.
4. Keep a small, risk-based E2E set for confirmed critical journeys and integration wiring.
5. Add defense-in-depth only when the higher layer verifies a distinct boundary or customer-critical wiring; explain the non-duplicate purpose.
6. If evidence cannot establish the required layer or oracle, assign `Pending` / `Question/TBD` rather than guessing. A provisional recommendation must be labelled as such.

Examples of contested decisions:

- Input rules normally belong at Unit or API/Integration; retain a Component case only when the UI-specific message, field state, or interaction is itself required.
- API statuses and response contracts belong at API/Integration; an E2E case may confirm that a critical journey surfaces the failure, but it should not be the only contract test.
- A calculation can be Unit-tested, while its persistence, authorization, and serialized representation require API/Integration defense-in-depth.
- A UI state belongs at Component when it can be mounted in isolation; a browser journey is E2E only when routing, rendering integration, or real browser behavior is part of the requirement.

Security, performance, reliability, accessibility, compatibility, usability, and exploratory objectives may require specialized follow-up. A Unit/API/Component/E2E assignment alone does not establish those properties.

## Assignment record

For every executable scenario, preserve its ID and record:

```markdown
| Scenario ID | Category | Priority | Assigned Layer | Primary/Defense-in-Depth | Requirement/Source References | Function/Endpoint/Component/Test Reference | Required Oracle and Boundary | Rationale | Lower-Layer Alternative | Status | Assumptions/Residual Risks |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
```

Use exact discovered paths and symbols when available. If none exists, write `Not found in repository` and identify the missing evidence. Do not turn a scenario’s category into a source reference. Preserve multiple assignments only when each has a distinct purpose.

Validate scenario input before assigning layers:

- IDs are unique and category ranges match.
- Required fields, source/rule references, status, priority, setup, steps, and exact oracle are present.
- Duplicate or conflicting scenarios are linked rather than silently merged.
- Unsupported suggested layers, missing sources, and unresolved oracles are visible.
- Scenario design coverage is not confused with layer distribution or execution coverage.

## Distribution and anti-patterns

Report a distribution table with explicit denominators:

| Layer | Assigned scenario count | Percentage of executable scenarios | Focus | Expected time | Evidence status |
| --- | ---: | ---: | --- | --- | --- |
| Unit |  |  |  | Known / Unknown |  |
| API/Integration |  |  |  | Known / Unknown |  |
| Component |  |  |  | Known / Unknown |  |
| E2E |  |  |  | Known / Unknown |  |

A percentage is `assigned scenarios / executable scenarios × 100%` and is `N/A` when the denominator is undefined or zero. Do not invent execution time; record a measured estimate, a documented team estimate, or `Unknown`. A broad lower-layer base and smaller E2E layer is a principle, not a universal ratio.

Inspect existing tests when they exist and report findings with evidence. Check for:

- input validation or pure logic covered only through E2E;
- API statuses/error bodies covered only through E2E;
- all scenarios assigned to E2E (an ice-cream-cone shape);
- no E2E confirmation for a confirmed critical cross-boundary journey;
- duplicate multi-layer cases without a distinct oracle or boundary;
- assignments unsupported by a discovered harness or source;
- lower-layer candidates left at a higher layer;
- critical rules lacking defense-in-depth where the risk warrants it.

If no tests are present, report anti-pattern analysis as `Not assessable`, not as proof that no anti-pattern exists. If no application source is present, report source analysis as unavailable.

## Empty-state behavior

When `docs/test-scenarios.md` has no confirmed executable scenarios:

- state `Blocked — no executable scenario records available`;
- list the sources and paths actually inspected;
- show no fabricated scenario-to-layer rows;
- show Unit, API/Integration, Component, and E2E counts as `0 assigned`, with denominator `0` and percentages `N/A`;
- set time to `Unknown`;
- record layer assignments as `Pending` only for supplied but unresolved records;
- list questions for requirements, oracles, implementation, harnesses, and critical journeys;
- mark E2E and defense-in-depth recommendations as conditional residual risks, not confirmed tests.

Zero assigned tests is an inventory fact, not evidence of a healthy pyramid, complete coverage, or absence of defects.

## Output sections

When no format is requested in direct/shared mode, write `docs/test-strategy.md` with the following sections. During an orchestrated run, write the same sections to the explicit run-local destination supplied by `test-pipeline`:

1. Scope and analysis status.
2. Scenario and source inventory.
3. Pyramid principles and decision rules.
4. Distribution table.
5. Scenario-to-layer assignments.
6. Contested assignment rationale.
7. Defense-in-depth recommendations.
8. Existing-test anti-pattern analysis.
9. Missing evidence, questions, gaps, assumptions, and residual risks.
10. Coverage and limitations.
11. Verification checklist.

Separate these metrics:

- scenario records validated;
- scenarios assigned to a layer;
- source-reference coverage;
- executable-layer distribution;
- defense-in-depth recommendations;
- execution coverage and outcomes, only when execution evidence exists;
- specialized non-functional follow-up coverage.

Do not claim that a pyramid percentage, test count, or 100% assignment proves complete requirements, branches, states, interactions, security, performance, reliability, accessibility, compatibility, usability, or defect absence.

## Verification checklist

Before returning the strategy:

- [ ] The requested scope and `docs/test-scenarios.md` input status are explicit.
- [ ] Actual repository evidence was discovered; absent sources are labelled unavailable.
- [ ] Scenario IDs, categories, references, statuses, priorities, and exact oracles were validated.
- [ ] Each confirmed assignment uses the lowest adequate layer and cites a real boundary or harness.
- [ ] Unit, API/Integration, Component, and E2E criteria are applied consistently.
- [ ] Suggested layers are reviewed rather than copied.
- [ ] Every contested assignment explains why lower layers are insufficient or preferred.
- [ ] Defense-in-depth cases have distinct purposes and do not hide duplicate coverage.
- [ ] Distribution counts, percentages, denominators, and timing claims are accurate or explicitly unknown.
- [ ] Existing-test anti-patterns are evidence-based; no tests means `Not assessable`.
- [ ] Blocking questions, assumptions, missing sources, exclusions, gaps, and residual risks are visible.
- [ ] Specialized security and non-functional follow-ups are not implied by ordinary layer assignment.
- [ ] Output is valid Markdown and preserves the requested format.
