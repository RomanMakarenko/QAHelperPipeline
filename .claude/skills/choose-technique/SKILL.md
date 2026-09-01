---
name: choose-technique
description: Select one or more ISTQB-aligned black-box test-design techniques for a supplied requirement, acceptance criterion, feature, workflow, test case, configuration matrix, or risk description. Trigger when the user asks which technique to use, how to combine techniques, or how to route a testing task to the appropriate technique skill.
version: 0.1.0
---

# Test-Design Technique Selector

Apply this skill when the user asks which test-design technique or combination of techniques should be used for a requirement, acceptance criterion, feature, API rule, workflow, existing test case, configuration matrix, defect history, or testing risk.

Use the project's selector guide as the primary reference:

`docs/TestDesignAndSoftwareTestingTechniques/chooseTechnique.md`

Use the detailed guide and skill for each selected technique when preparing the actual test design:

- `docs/TestDesignAndSoftwareTestingTechniques/BlackBox/equivalencePartitioning.md` → `.claude/skills/equivalence-partitioning/SKILL.md`
- `docs/TestDesignAndSoftwareTestingTechniques/BlackBox/boundaryValueAnalysis.md` → `.claude/skills/boundary-value-analysis/SKILL.md`
- `docs/TestDesignAndSoftwareTestingTechniques/BlackBox/decisionTable.md` → `.claude/skills/decision-table/SKILL.md`
- `docs/TestDesignAndSoftwareTestingTechniques/BlackBox/pairwiseTesting.md` → `.claude/skills/pairwise-testing/SKILL.md`
- `docs/TestDesignAndSoftwareTestingTechniques/BlackBox/stateTransition.md` → `.claude/skills/state-transition-testing/SKILL.md`
- `docs/TestDesignAndSoftwareTestingTechniques/BlackBox/errorGuessing.md` → `.claude/skills/error-guessing/SKILL.md`

If the selector guide is unavailable, use the fallback rules in this skill and the detailed technique skills. Do not invent product behavior, constraints, states, values, levels, priorities, or oracles.

## Scope and core principle

Select from the structure of the requirement and its dominant risk, not from whether the resulting cases will be manual or automated. A feature may require several techniques, each applied to a different requirement aspect.

This selector chooses and explains a route. It does not duplicate the detailed partition, boundary, rule, interaction, state, or hypothesis case-generation procedures of the selected skills.

## Input contract

Accept:

- a complete or partial requirement, acceptance criterion, user story, API contract, policy, or business rule;
- a feature, workflow, lifecycle, event rule, retry/timeout rule, or state diagram;
- an existing test case, suite, test matrix, coverage report, or checklist to review;
- a browser/OS/device/locale/configuration/environment matrix;
- a defect, incident, production symptom, support trend, risk report, recent change, or legacy concern;
- a requested output format such as Markdown, Gherkin, JSON, CSV, or test-management schema.

Extract when available:

- scope, operation, actor, role, modeled object, lifecycle boundary, and requirements;
- inputs, representations, types, formats, units, ranges, categories, and semantic classes;
- exact outcomes: statuses, messages, calculations, persistence, processing path, notifications, invocations, side effects, or state changes;
- conditions, actions, defaults, precedence, dependencies, legal/forbidden combinations, and timing;
- states, events, guards, transitions, retries, expiration, recovery, and event ordering;
- parameters, levels, baseline values, environment factors, interaction strength, and constraints;
- defect history, recent changes, complexity, exposure, impact, user-error patterns, and regression seeds;
- requested priority, BVA variant, Pairwise strength, generation method, and output schema.

An existing test case or matrix is evidence to assess, not proof of adequate modeling or coverage.

## Evidence and status rules

Use these labels consistently:

- **Confirmed** — stated in supplied requirements, contracts, approved models, observed behavior, or cited evidence.
- **Assumption** — introduced for a provisional selection; never present it as product behavior.
- **Question/TBD** — unresolved information that requires confirmation.
- **Residual risk** — meaningful behavior outside the selected scope or not covered by the hand-off.

Ask only questions that block safe selection or make the next skill's oracle or model unknowable. Otherwise proceed with clearly labeled assumptions and risks. Never turn an unknown endpoint, state, dependency, level, legal combination, message, or outcome into a confirmed fact.

## Selection workflow

1. **Define scope.** Record the operation, actor, role, modeled object, lifecycle boundary, requirement references, setup, and out-of-scope behavior.
2. **Define the oracle.** Capture exact observable pass/fail results. If unknown, record a `REQ-SIG-UNC-*` signal and a `Question/TBD`.
3. **Classify evidence.** Separate confirmed facts, assumptions, questions, and residual risks.
4. **Extract stable signals.** Create `REQ-SIG-DOM-*`, `REQ-SIG-BND-*`, `REQ-SIG-RULE-*`, `REQ-SIG-STATE-*`, `REQ-SIG-FACTOR-*`, `REQ-SIG-GOAL-*`, `REQ-SIG-HIST-*`, or `REQ-SIG-UNC-*` IDs as applicable.
5. **Model dependencies.** Classify inputs/factors as independent, dependent, derived, state-driven, role-driven, conditional, allowed, forbidden, impossible, or unknown.
6. **Select a primary technique.** Choose the technique that addresses the dominant confirmed risk. Do not use keyword-only mapping.
7. **Select secondary techniques.** Add a technique only when it addresses a distinct risk or supplies a model needed by another selection. Distinguish primary, secondary, and complementary approaches.
8. **Set configuration.** State EP partition scope; BVA 2-value/3-value and normal/robust variant; Decision Table rule scope, constraints, defaults, and precedence; Pairwise strength and legal completion; State-Transition lifecycle and path scope; or Error Guessing evidence and priority.
9. **Justify exclusions.** Explain deferred techniques and the risk they leave uncovered.
10. **Prepare hand-offs.** Give the selected skill the exact requirement references, signal IDs, modeled elements, constraints, oracle, priority, and requested output format.
11. **Review feasibility.** Ensure the hand-off can be executed without inventing behavior. Ask blocking questions before claiming a complete selection.
12. **Report limitations.** Separate selector traceability from technique-specific model coverage and execution coverage.

## Selection rules

Apply these fallback rules when the guide cannot be read, then confirm them against the detailed skills where possible:

- **Equivalence Partitioning (EP):** select for behaviorally distinct value, representation, category, range, role, valid, or invalid classes. Use one or more reproducible representatives per non-empty partition. EP does not cover boundary defects, combinations, lifecycle paths, or non-functional properties by itself.
- **Boundary Value Analysis (BVA):** select for ordered edges where behavior changes: numeric, length, size, count, quota, capacity, date/time, duration, retry, age, score, or tariff limits. Model EP first and explicitly define endpoint ownership, unit, increment, precision, and 2-value/3-value normal/robust variant. Do not force BVA onto unordered sets.
- **Decision Table Testing:** select when joint conditions determine actions or outcomes, including defaults, overlap, precedence, conditional availability, legal/forbidden combinations, or conflicting rules. Use EP/BVA to define meaningful condition entries. `AND`/`OR` alone is not sufficient; the joint vector must materially affect an observable result.
- **State-Transition Testing:** select when behavior depends on current domain state, event, guard, prior history, retry, timeout, expiration, recovery, duplicate, terminal, stale, or out-of-order behavior. A GUI screen is not automatically a state. Define one object or an explicitly bounded composition.
- **Pairwise Testing:** select when there are multiple mostly independent factors with meaningful levels and the Cartesian product is too large. Derive levels with EP/BVA, formalize constraints and legal complete-row extension, and state 2-way/3-way/higher strength. Do not use it as the sole technique for strongly dependent rules or lifecycle behavior.
- **Error Guessing:** select as an evidence- and experience-based supplement for incomplete specifications, legacy behavior, recent changes, defect history, production incidents, malformed/unusual inputs, or likely user and integration errors. Each hypothesis needs evidence, context, predicted failure, exact oracle, priority, and status. It does not replace systematic techniques for known models.
- **Use Case/Scenario Testing:** recommend as a complementary approach when a user or business goal must be validated across an end-to-end workflow.
- **Exploratory Testing:** recommend as a complementary approach when the system or requirement requires learning, investigation, or regression discovery. Record charter, evidence, questions, and follow-ups.

## Composition rules

Use this order unless requirement evidence supports another explicitly documented sequence:

1. Establish scope and oracle.
2. Model EP partitions.
3. Add BVA values at confirmed ordered edges.
4. Use EP/BVA entries to build Decision Tables for joint rules or to supply Pairwise levels.
5. Use State-Transition for lifecycle/history; use Decision Tables for complex guards and BVA for retry/time thresholds when applicable.
6. Use Pairwise for mostly independent contextual/configuration dimensions after constraints and legal completion are known.
7. Add critical Use Case/Scenario coverage across the feature.
8. Add prioritized Error Guessing and Exploratory work for evidence-based or unmodeled risks.

Document dependencies between selections, for example: `SEL-001` EP supplies partitions to `SEL-002` BVA; `SEL-001` and `SEL-002` supply levels to `SEL-003` Pairwise; or `SEL-004` Decision Table supplies guards for `SEL-005` State-Transition.

## Default output

Preserve any requested format. When none is requested, output Markdown with exactly these sections:

```markdown
## Scope and requirement basis

## Extracted requirement signals

## Technique selection and rationale

## Composition and execution order

## Clarifications, assumptions, questions, and residual risks

## Technique-skill handoff package

## Verification checklist
```

### Scope and requirement basis

Include operation, actor/role, modeled object/lifecycle, requirement references, exact oracle, setup/context, out-of-scope behavior, and evidence status.

### Extracted requirement signals

Use a table:

| Signal ID | Type | Requirement/evidence | Behavior or risk represented | Status |
| --- | --- | --- | --- | --- |
| `REQ-SIG-DOM-001` | Domain / boundary / rule / state / factor / goal / history / uncertainty |  |  | Confirmed / Assumption / Question/TBD / Residual risk |

### Technique selection and rationale

Use a table:

| Selection ID | Technique | Primary/secondary/complementary | Signal IDs | Why selected | Configuration | Guide path | Skill path | Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `SEL-001` |  |  |  |  |  |  |  | Confirmed / Assumption / Question/TBD |

For exclusions, list the deferred technique, reason, uncovered risk, and follow-up rather than silently omitting it.

### Composition and execution order

Describe prerequisites and which selection is authoritative for each modeled outcome. Keep scenario, Error Guessing, Exploratory, and non-functional follow-ups distinct from the six formal technique selections.

### Clarifications, assumptions, questions, and residual risks

Use stable IDs and state which selection is affected:

| Item ID | Category | Statement | Affected selection | Follow-up | Status |
| --- | --- | --- | --- | --- | --- |
| `CLAR-001` | Assumption / Question/TBD / Residual risk |  |  |  |  |

### Technique-skill handoff package

Create one hand-off per selected technique:

| Field | Required content |
| --- | --- |
| Selection ID | Stable `SEL-*` ID |
| Technique and skill path | Exact existing `.claude/skills/.../SKILL.md` path |
| Requirement/signal references | Requirement IDs and `REQ-SIG-*` IDs |
| Scope and operation | What the selected skill must design |
| Exact oracle | Observable pass/fail result; never “works correctly” |
| Modeled elements | Partitions/boundaries, conditions/actions, states/events, or factors/levels |
| Constraints/dependencies | Legal, forbidden, conditional, precedence, timing, and state rules |
| Configuration | BVA variant, Pairwise strength, table reduction, state path scope, or heuristic priority |
| Requested output | Markdown, Gherkin, JSON, CSV, or test-management schema |
| Priority and risks | Priority and uncovered areas |
| Status | Confirmed / Assumption / Question/TBD / Residual risk |

Do not duplicate the selected skill's detailed executable test cases in this selector response.

## Coverage and limitations

Report selection quality separately from downstream testing:

- **Signal traceability:** selected signals with requirement/evidence references / extracted signals.
- **Selection traceability:** selections linked to at least one signal and requirement reference / selections.
- **Handoff completeness:** hand-offs containing scope, oracle, model, constraints, configuration, priority, and output schema / selected hand-offs.
- **Technique-specific coverage:** calculated by the selected skill, such as EP partition, BVA boundary-position, Decision Table feasible-rule, Pairwise legal-pair, State-Transition reachable-state/transition, or Error Guessing hypothesis coverage.
- **Execution coverage:** calculated only after test cases execute with evidence.

Never imply that all signals were found, that selected techniques cover every requirement, or that 100% of a technique metric proves complete quality. Identify higher-order interactions, omitted values, unknown legality, unmodeled states, concurrency, non-functional obligations, and untestable oracles as residual risks.

## Verification checklist

Before returning the selection, verify:

- [ ] Scope, operation, actor, modeled object, lifecycle boundary, requirement basis, and out-of-scope behavior are explicit.
- [ ] Exact observable oracle is stated, or its absence is labeled `Question/TBD` or `Residual risk`.
- [ ] Stable `REQ-SIG-*`, `SEL-*`, clarification, and risk IDs are used.
- [ ] Confirmed facts, assumptions, questions, and residual risks are distinct.
- [ ] Selection follows requirement structure and risk, not manual versus automation execution.
- [ ] EP is used for behaviorally distinct classes and does not silently absorb boundary, combination, state, or format-specific behavior.
- [ ] BVA has a modeled EP basis, explicit endpoint ownership, unit, precision, increment, and variant.
- [ ] Decision Table selection is supported by materially different joint condition/action outcomes, with constraints, defaults, overlap, and precedence considered.
- [ ] State-Transition selection has a domain object/lifecycle scope; GUI screens are not treated as states without evidence.
- [ ] Pairwise selection has mostly independent factors, meaningful levels, legal constraints, legal completion, and explicit interaction strength.
- [ ] Error Guessing hypotheses have evidence/rationale, falsifiable predictions, exact oracles, priority, and status.
- [ ] Use Case/Scenario and Exploratory Testing are correctly presented as complementary approaches.
- [ ] Primary, secondary, and complementary selections and their composition order are explicit.
- [ ] Exclusions and their uncovered risks are visible.
- [ ] Every hand-off has skill path, requirement/signal traceability, modeled elements, constraints, configuration, oracle, output schema, and priority.
- [ ] No product behavior, values, states, combinations, messages, or coverage is invented or presented as confirmed.
- [ ] Selection, model, and execution coverage are reported separately.
- [ ] Security, performance, reliability, accessibility, usability, compatibility, and other specialized follow-ups are considered.
- [ ] The response routes to existing technique skills instead of duplicating their detailed case-generation procedures.
