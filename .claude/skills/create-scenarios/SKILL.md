---
name: create-scenarios
description: Generate traceable functional test scenarios from supplied requirements, domain knowledge, repository evidence, and implementation behavior using six coverage lenses. Trigger when the user asks for test scenarios, functional cases, a feature-flow inventory, or a complete scenario suite.
version: 0.1.0
argument-hint: "[feature-name or blank for full suite]"
---

# Functional Scenario Designer

Use this skill to design functional test scenarios, not to invent product requirements or write automation code. `$ARGUMENTS` identifies a feature or flow. When it is blank, analyze the broadest scope supported by confirmed evidence and state what remains outside scope.

In direct/shared mode, the default output is `docs/test-scenarios.md`. In an orchestrated ticket run, use the explicit run-local destination supplied by `test-pipeline`; do not mutate the shared file. Preserve requested formats such as Markdown, Gherkin, JSON, CSV, or a test-management schema while retaining the same evidence and traceability fields.

## Evidence and source discovery

Inspect sources in this order and record what was actually available:

1. User-provided requirements, acceptance criteria, links, files, contracts, and domain instructions.
2. Repository specifications, documentation, configuration, fixtures, and task artifacts relevant to the requested scope.
3. Existing project-local skills and technique guides.
4. Implementation files discovered by repository search.
5. Existing tests, mocks, fixtures, and recorded defects discovered by repository search.

Do not assume an application layout or named domain. Search the repository for relevant files rather than presuming a particular backend, frontend, API, or test directory. Technique guides describe testing methods; they are not product requirements.

Create stable identifiers without renumbering existing records:

| ID type | Format | Meaning |
| --- | --- | --- |
| Source | `SRC-001` | Requirement, document, code, test, defect, or user-provided evidence |
| Flow | `FLOW-001` | Confirmed feature, operation, or user flow |
| Rule | `RULE-001` | Confirmed business or technical rule |
| Clarification | `CLAR-001` | Assumption or blocking question |
| Scenario | `TC-001` etc. | Executable functional scenario |
| Risk | `RISK-001` | Uncovered or weakly evidenced risk |

Use these evidence labels consistently:

- **Confirmed** — stated in a requirement, contract, approved document, observed implementation behavior, or cited evidence.
- **Assumption** — introduced only to make a provisional design explicit; never present it as product behavior.
- **Question/TBD** — unresolved information that blocks a safe case or exact oracle.
- **Residual risk** — meaningful behavior not established or not covered by this scope.

If no product/domain evidence is available, write a blocked result: zero executable scenarios, an inventory of missing inputs, and questions needed to continue. Do not create fictional flows, roles, endpoints, messages, values, states, priorities, or expected results.

Ask only questions that block safe scenario design or make the oracle unknowable. Otherwise proceed with clearly marked assumptions and risks.

## Six-lens analysis

For every confirmed `FLOW-*`, consider all six lenses. Create scenarios only when the lens is applicable to confirmed evidence; otherwise record `Not assessable` or `Not applicable` with a reason and a residual risk where appropriate.

| Category | ID range | Questions to investigate |
| --- | --- | --- |
| Happy Path | `TC-001`–`TC-099` | What successful goal, completion state, and observable outputs are required? |
| Business Rules | `TC-100`–`TC-199` | Which conditions, calculations, permissions, defaults, precedence rules, and side effects change the outcome? |
| Security | `TC-200`–`TC-299` | What authentication, authorization, ownership, input-manipulation, secrecy, audit, or data-exposure behavior is specified? |
| Negative/Error | `TC-300`–`TC-399` | What missing, malformed, unsupported, rejected, duplicate, wrong-state, timeout, or recovery behavior is defined? |
| Edge Cases | `TC-400`–`TC-499` | Which confirmed limits, thresholds, representations, timing, ordering, locale, precision, or capacity values are risky? |
| UI State | `TC-500`–`TC-599` | Which loading, empty, validation, disabled, error, conditional, responsive, keyboard, or accessibility states are evidenced? |

A category is not evidence by itself. For example, do not invent UI cases when no UI behavior is supplied, and do not infer a security policy from the existence of a login field.

## Scenario-generation workflow

1. Define the requested scope, actor, operation, object, lifecycle boundary, setup, and out-of-scope behavior.
2. Inventory evidence and assign `SRC-*`, `FLOW-*`, and `RULE-*` IDs. Record source path, location, version/date when available, and status.
3. Extract exact observable oracles: status, message, accepted/rejected value, calculation, persisted state, transition, event, notification, audit record, invocation, side-effect count, or recovery outcome.
4. Apply all six lenses and record applicability, covered IDs, unavailable evidence, and residual risks.
5. In direct/standalone use, route requirement structure to `choose-technique` when systematic design is needed. During a `test-pipeline` run, return requirement signals to the orchestrator, which performs the single technique-selection hand-off. Preserve signal/selection IDs and use the existing technique skills for EP, BVA, Decision Table, Pairwise, State-Transition, and Error Guessing details.
6. Generate only non-duplicate scenarios. Preserve existing `TC-*` IDs; allocate the next unused ID within the category range. A scenario may have one primary category and additional technique tags.
7. Prioritize using available impact, likelihood, exposure, change risk, defect history, security/safety impact, and cost evidence. If those inputs are unavailable, use `Question/TBD` or a clearly provisional priority rather than inventing P0/P1 behavior.
8. Review each case for reproducible setup, complete input/context, deterministic steps, exact oracle, cleanup, traceability, and status.
9. Report design coverage separately from execution coverage. A generated scenario is not an executed test and does not prove quality.
10. Write or update the explicit destination, retaining useful existing content, stable IDs, assumptions, questions, and residual risks. Use `docs/test-scenarios.md` only in direct/shared mode; an orchestrated run must use its run-local artifact path.

## Technique hand-offs

Do not duplicate the detailed procedures in the existing skills. Use these exact paths when a hand-off is applicable:

- `docs/TestDesignAndSoftwareTestingTechniques/chooseTechnique.md` → `.claude/skills/choose-technique/SKILL.md`
- `docs/TestDesignAndSoftwareTestingTechniques/BlackBox/equivalencePartitioning.md` → `.claude/skills/equivalence-partitioning/SKILL.md`
- `docs/TestDesignAndSoftwareTestingTechniques/BlackBox/boundaryValueAnalysis.md` → `.claude/skills/boundary-value-analysis/SKILL.md`
- `docs/TestDesignAndSoftwareTestingTechniques/BlackBox/decisionTable.md` → `.claude/skills/decision-table/SKILL.md`
- `docs/TestDesignAndSoftwareTestingTechniques/BlackBox/pairwiseTesting.md` → `.claude/skills/pairwise-testing/SKILL.md`
- `docs/TestDesignAndSoftwareTestingTechniques/BlackBox/stateTransition.md` → `.claude/skills/state-transition-testing/SKILL.md`
- `docs/TestDesignAndSoftwareTestingTechniques/BlackBox/errorGuessing.md` → `.claude/skills/error-guessing/SKILL.md`

Use technique tags to preserve the relationship. EP identifies behaviorally distinct classes; BVA targets confirmed ordered boundaries after EP; Decision Tables cover materially different condition/action combinations; Pairwise covers mostly independent factors with legal complete rows; State-Transition covers lifecycle/history; Error Guessing covers evidence-based hypotheses. Scenario/use-case, exploratory, security, performance, accessibility, compatibility, reliability, and usability work remains complementary.

## Required scenario record

Every executable scenario must contain all applicable fields below. If a field cannot be known, mark it `Question/TBD` and explain why.

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
**Expected Results**: <exact observable oracle, including state, persistence, messages, side effects, and counts>
**Business Rule**: <confirmed rule, or explicitly labelled Assumption/Question/TBD>
**Suggested Layer**: <Unit | API/Integration | Component | E2E | Question/TBD>
**Technique Tags**: <EP | BVA | Decision Table | Pairwise | State-Transition | Error Guessing | Scenario>
**Evidence to Capture**: <response, record, screenshot, event, log, or other permitted artifact>
**Cleanup/Repeatability**: <reset, teardown, seed, timing, and reproducibility notes>
**Assumptions/Residual Risks**: <explicit limitations>
```

The oracle must name what can be observed and compared. A generic success claim, an unspecified error, or a statement that a page behaves normally is not an oracle. If failure handling is unspecified, record `Question/TBD` instead of guessing.

## Output sections

When no format is requested, produce `docs/test-scenarios.md` with:

1. Scope and generation status.
2. Evidence/source inventory.
3. Flow and rule inventory.
4. Six-lens applicability and coverage.
5. Scenarios grouped by the six ID ranges.
6. Assumptions, blocking questions, gaps, and residual risks.
7. Design and execution coverage with explicit denominators.
8. Handoff to `test-strategy`.
9. Verification checklist.

At minimum report:

- confirmed flows and sources represented by scenarios;
- applicable and unavailable lenses;
- selected scenario IDs by category and priority;
- duplicate, malformed, untraceable, blocked, or unresolved records;
- formal technique follow-ups and non-functional follow-ups;
- untested requirements, risks, sources, and high-priority scenarios.

Do not claim that all flows, defects, values, branches, states, interactions, security properties, or non-functional characteristics are covered merely because the six sections exist or a selected-scenario percentage reaches 100%.

## Verification checklist

Before returning the result:

- [ ] Scope, actor, operation, flow, evidence basis, and out-of-scope behavior are explicit.
- [ ] Sources and rules have stable IDs and accurate status labels.
- [ ] All six lenses were considered for every confirmed flow, with unavailable applicability recorded.
- [ ] Every scenario uses the correct category range and a unique stable ID.
- [ ] Every case has complete setup/data/context, deterministic steps, an exact observable oracle, evidence capture, and cleanup.
- [ ] Requirements, rules, code behavior, technique selections, and assumptions are not confused.
- [ ] Suggested layers are proposals and do not replace `test-strategy` analysis.
- [ ] Unknown behavior is `Question/TBD` or residual risk, never fabricated.
- [ ] EP, BVA, Decision Table, Pairwise, State-Transition, and Error Guessing follow their referenced skills.
- [ ] Design, scenario, execution, risk, and non-functional coverage remain separate with explicit denominators.
- [ ] Blocked, unresolved, excluded, duplicate, and untested items are visible.
- [ ] Markdown and the requested output format are valid.
