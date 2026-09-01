# Choosing a Test-Design Technique

## Purpose and scope

This guide explains how to select one or more black-box, specification-based test-design techniques for a requirement, acceptance criterion, feature, workflow, API contract, configuration matrix, existing test case, or risk description.

The techniques covered by this selector are:

- **Equivalence Partitioning (EP)**
- **Boundary Value Analysis (BVA)**
- **Decision Table Testing**
- **Pairwise Testing**
- **State-Transition Testing**
- **Error Guessing Test Design**

Use Case/Scenario Testing and Exploratory Testing are also described as complementary approaches. They are useful for end-to-end goals and investigation, but they are not substitutes for selecting the formal techniques above when the requirement contains modeled domains, boundaries, rules, interactions, or states.

This is a selection guide, not a replacement for the detailed technique guides. After selection, use the relevant skill and guide to build the test design:

| Technique | Detailed guide | Skill |
| --- | --- | --- |
| EP | `docs/TestDesignAndSoftwareTestingTechniques/BlackBox/equivalencePartitioning.md` | `.claude/skills/equivalence-partitioning/SKILL.md` |
| BVA | `docs/TestDesignAndSoftwareTestingTechniques/BlackBox/boundaryValueAnalysis.md` | `.claude/skills/boundary-value-analysis/SKILL.md` |
| Decision Table | `docs/TestDesignAndSoftwareTestingTechniques/BlackBox/decisionTable.md` | `.claude/skills/decision-table/SKILL.md` |
| Pairwise | `docs/TestDesignAndSoftwareTestingTechniques/BlackBox/pairwiseTesting.md` | `.claude/skills/pairwise-testing/SKILL.md` |
| State-Transition | `docs/TestDesignAndSoftwareTestingTechniques/BlackBox/stateTransition.md` | `.claude/skills/state-transition-testing/SKILL.md` |
| Error Guessing | `docs/TestDesignAndSoftwareTestingTechniques/BlackBox/errorGuessing.md` | `.claude/skills/error-guessing/SKILL.md` |

## Core principle

Choose a technique from the **structure of the requirement and the dominant risk**, not from the test execution method. The same design may be executed manually, through automation, or both.

A good selector answers these questions in order:

1. What operation, actor, object, and observable outcome are in scope?
2. What kinds of variation can change the outcome: values, thresholds, conditions, states, factors, user goals, or uncertainty?
3. Which parts are confirmed by the requirement, and which are assumptions or questions?
4. Which technique models the dominant risk most directly?
5. Which complementary techniques are needed because the primary technique leaves known gaps?
6. What exact oracle, constraints, strength, variant, and hand-off information does the next skill need?

Do not force one technique onto an entire feature. A single feature commonly needs several techniques applied to different requirement clauses.

## Evidence, status, and no-invention policy

The selector must distinguish evidence from interpretation. Use these labels for every extracted signal, selected rule, assumption, and unresolved item:

- **Confirmed** — stated in the supplied requirement, contract, approved model, observed behavior, or cited evidence.
- **Assumption** — introduced to make a provisional selection possible; it must not be presented as product behavior.
- **Question/TBD** — unresolved information that requires confirmation.
- **Residual risk** — meaningful behavior outside the selected scope or not covered by the proposed design.

Never invent a supported value, endpoint, state, dependency, legal combination, priority, error message, or expected result. If the expected oracle is unknown, record the gap and ask a blocking question when selection or safe hand-off depends on it.

An existing test case, matrix, checklist, or defect report is evidence to analyze, not proof that the model or coverage is complete. A selected technique and a passing generated suite do not prove that all requirements, branches, values, states, interactions, or non-functional properties are covered.

## Selector input contract

The selector can accept any of the following:

- a complete or partial requirement or acceptance criterion;
- a user story, ticket, API contract, business rule, policy, or workflow;
- an existing test case, test suite, test matrix, or coverage report to review;
- a browser/device/OS/configuration/environment matrix;
- a state diagram, lifecycle, event contract, retry, timeout, or recovery rule;
- a defect, incident, support trend, checklist, risk register, or recent change;
- a requested output format such as Markdown, Gherkin, JSON, CSV, or a test-management schema.

Extract the following when available:

| Input area | What to capture |
| --- | --- |
| Scope | Operation, feature, actor, modeled object, lifecycle boundary, and out-of-scope areas |
| Requirement basis | Requirement, acceptance-criterion, contract, model, defect, or risk references |
| Inputs | Fields, parameters, representations, types, formats, units, values, and semantic meaning |
| Outcomes | Exact response/status, message, calculation, persistence, processing path, notification, invocation, or state change |
| Rules | Conditions, actions, defaults, precedence, implications, allowed/forbidden combinations, and dependencies |
| States | Initial, intermediate, terminal, error, recovery states; events; guards; and transition effects |
| Factors | Configuration/environment dimensions, levels, baseline values, and interaction constraints |
| Risk evidence | Defect history, production incidents, recent changes, complexity, exposure, impact, and user error patterns |
| Execution constraints | Time, environment, available data, requested prioritization, interaction strength, BVA variant, or output schema |

If an item is absent, do not silently fill it. Mark it `Question/TBD`, `Assumption`, or `Residual risk` as appropriate.

## Requirement-signal inventory

Before choosing a technique, create stable signal IDs. A signal is a property of the requirement that indicates a testing risk or a useful model.

| Signal ID pattern | Signal | Evidence that supports it | Likely technique implication |
| --- | --- | --- | --- |
| `REQ-SIG-DOM-*` | Behaviorally distinct value, category, format, or valid/invalid class | Sets, formats, ranges, types, null/missing rules, accepted/rejected classes | EP; add BVA if ordered edges exist |
| `REQ-SIG-BND-*` | Ordered threshold or transition between adjacent behaviors | Minimum/maximum, length, count, quota, date/time cutoff, score, tariff | BVA after EP modeling |
| `REQ-SIG-RULE-*` | Joint conditions determine one or more actions/outcomes | IF/ELSE, AND/OR with materially different actions, policy, eligibility, precedence, defaults | Decision Table |
| `REQ-SIG-STATE-*` | Outcome depends on current state or prior event history | Lifecycle, status, retries, expiration, locking, recovery, terminal behavior | State-Transition |
| `REQ-SIG-FACTOR-*` | Multiple mostly independent parameters interact | Browser, OS, device, locale, role, payment, feature flag, configuration | Pairwise or stronger t-way |
| `REQ-SIG-GOAL-*` | Actor must complete a business goal across several steps | Registration, checkout, booking, onboarding, payment, fulfillment | Use Case/Scenario as complementary coverage |
| `REQ-SIG-HIST-*` | Risk is suggested by experience or evidence rather than a complete formal model | Defect history, incidents, recent changes, legacy behavior, common user errors | Error Guessing; risk-based/regression follow-up |
| `REQ-SIG-UNC-*` | Requirement, oracle, constraint, or model is incomplete | Ambiguous endpoint, unknown default, missing state, unclear error, unspecified legality | Clarification and explicitly labeled provisional design |

One requirement may produce several signal types. Record the evidence and status of each signal instead of choosing from keywords alone.

### Signal extraction questions

Ask the following questions while reading the requirement:

1. Are there value classes whose members should have equivalent behavior?
2. Is there a meaningful order in which behavior changes at a limit or threshold?
3. Do combinations of conditions select different actions, including defaults or precedence?
4. Does the result depend on a domain state, event order, retry count, expiration, or prior history?
5. Are there several parameters whose interactions need coverage, and are they mostly independent?
6. Is there an end-to-end actor goal that spans multiple operations?
7. Are there known defect patterns, recent changes, incomplete requirements, or unusual inputs that warrant heuristic tests?
8. Is the exact observable oracle known for each selected behavior?
9. Which combinations are legal, forbidden, conditional, impossible, or merely unknown?

## Decision matrix

Use this matrix to identify a candidate technique. Select a technique only when the requirement contains the corresponding structure or evidence.

| Technique | Select when the requirement contains | Why it fits | Typical configuration | Do not select as the sole technique when | Primary signal |
| --- | --- | --- | --- | --- | --- |
| **EP** | Distinct categories, formats, ranges, valid/invalid classes, roles, representations, or input domains | It reduces redundant values while representing each materially different behavior class | One or more representatives per non-empty, disjoint, exhaustive partition; Each Choice for multiple inputs | A defect may occur specifically at an edge, in a condition combination, in a lifecycle path, or in an interaction | `REQ-SIG-DOM-*` |
| **BVA** | A value is ordered and behavior changes at a minimum, maximum, threshold, cutoff, capacity, length, count, date/time, or tariff boundary | Comparison, off-by-one, rounding, truncation, and unit-conversion defects cluster near transitions | Model EP first; explicitly choose 2-value or 3-value and normal or robust BVA; define precision/increment | There is no meaningful order, endpoint ownership, or executable precision; categories are unordered | `REQ-SIG-BND-*` |
| **Decision Table** | A combination of conditions, facts, predicates, roles, states, or context selects actions or outcomes | It makes legal rules, conflicts, defaults, precedence, and missing combinations visible | Define conditions/actions, constraints, feasible rules, invalid rules, defaults, and precedence; exhaustive or safely reduced | Conditions are independent configuration dimensions with no joint action rule, or the issue is primarily a numeric boundary or lifecycle sequence | `REQ-SIG-RULE-*` |
| **State-Transition** | A domain object changes behavior based on current state, event, guard, sequence, retry, timeout, expiration, or recovery | It verifies valid and invalid transitions, state invariants, history, terminal behavior, and event ordering | Define one modeled object/lifecycle, initial and reachable states, events, guards, effects, valid/invalid transitions, and selected paths/pairs | Only a GUI screen/navigation changes with no evidence of a domain-state change, or there is no history-dependent behavior | `REQ-SIG-STATE-*` |
| **Pairwise** | Several parameters have multiple levels and are mostly independent, while full Cartesian testing is too large | It covers every required legal pair of levels with fewer complete configurations | Define EP/BVA-derived levels first; formalize constraints and legal completion; select 2-way by default only when justified, or 3-way/higher for evidence-based risk | Parameters are strongly dependent, conditions jointly determine business actions, lifecycle history dominates, or all combinations are small/critical enough for exhaustive testing | `REQ-SIG-FACTOR-*` |
| **Error Guessing** | Experience, defect history, recent change, legacy risk, production evidence, or incomplete specification suggests likely failures | It targets plausible failure mechanisms that formal models may omit | Record evidence-based hypotheses, exact oracle, priority, reproducible context, and result status | It would replace a systematic technique for known domains, boundaries, rules, states, or interactions | `REQ-SIG-HIST-*` / `REQ-SIG-UNC-*` |

### Fast triage, not a keyword rule

Use the following triage as a starting point, then validate the full requirement:

| Question | Candidate |
| --- | --- |
| Are there behaviorally different value or representation classes? | EP |
| Does behavior change at an ordered edge? | EP + BVA |
| Do several conditions jointly select actions or outcomes? | Decision Table, optionally with EP/BVA for condition entries |
| Does an event have a different result depending on the current state or history? | State-Transition, optionally with BVA for retry/time thresholds |
| Are there many mostly independent factors with legal combinations? | Pairwise, using EP/BVA levels |
| Are likely failures inferred from history, change, or tester/domain experience? | Error Guessing as a supplement |
| Is an end-to-end business goal important? | Use Case/Scenario as a supplement |
| Is behavior unknown and investigation is needed? | Exploratory as a supplement, with questions and observations recorded |

Words such as `AND`, `OR`, `IF`, “many parameters,” or “status” are only clues. They do not prove that a decision table, Pairwise, or State-Transition model is appropriate.

## Technique profiles and selection criteria

### Equivalence Partitioning (EP)

**Select EP when:** the input or output domain can be divided into non-empty classes for which the specification predicts equivalent behavior. Typical evidence includes valid/invalid values, categories, formats, roles, data types, supported/unsupported representations, or ranges.

**Why:** testing every value is often redundant. EP provides a controlled representative for each behaviorally meaningful class and makes missing or overlapping classes visible.

**Characteristic signals:**

- accepted and rejected values are described;
- a field has multiple formats or semantic categories;
- null, missing, empty, whitespace-only, malformed, unsupported, or unreadable forms may behave differently;
- a range contains valid and invalid regions;
- a role, account type, or file type changes the expected outcome.

**Example:** If a requirement explicitly says that an integer field accepts `[1,1000]` and rejects values below or above it, candidate partitions are `x < 1`, `1 <= x <= 1000`, and `x > 1000`. A representative such as `-37`, `46`, and `1773` can cover those classes. The exact status, message, or processing behavior must come from the requirement; the numbers are illustrative only.

**Do not stop at EP when:** the valid class has important edges (add BVA), several fields jointly determine an action (add Decision Table), values interact with configurations (consider Pairwise), or behavior depends on state/history (add State-Transition).

**Handoff requirements:** partition IDs, formal definitions, representatives, valid/invalid status, dependencies, exact oracle, and uncovered boundary/combination/state risks. Use the EP guide and skill listed in the table above.

### Boundary Value Analysis (BVA)

**Select BVA when:** expected behavior changes at an ordered boundary. Boundaries may be numeric, string length, file size, item count, quota, capacity, date/time, duration, age, score, or an ordered business threshold.

**Why:** comparison operators, off-by-one logic, rounding, truncation, precision, and unit conversion are common sources of defects at transitions.

**Characteristic signals:**

- `min`, `max`, `from`, `to`, `at least`, `at most`, `more than`, or `less than`;
- length, quantity, size, capacity, retry, timeout, age, date, or time limit;
- one tariff, status, or action applies on one side and another applies on the other side;
- endpoint inclusivity or precision matters.

**Required selection discipline:**

1. Model EP partitions first.
2. Establish endpoint ownership, unit, increment, precision, rounding, time zone, reference time, and representation.
3. Select **2-value** or **3-value** BVA explicitly.
4. State whether the design is **normal** or **robust** BVA.
5. Use representable below/at/above values; malformed input is usually a separate EP/negative case.

**Example:** For a confirmed inclusive password length of 8–20 characters, 3-value BVA candidates are 7/8/9 and 19/20/21. Whether length means Unicode code points, grapheme clusters, or bytes and what message appears must be confirmed. Do not claim that this example is a product rule.

**Do not select BVA when:** the values have no meaningful order, endpoint ownership is unknowable, or the requirement only lists unordered categories. Use EP or Decision Table instead, and record the ambiguity if it affects the design.

**Handoff requirements:** partition IDs, boundary IDs, endpoint ownership, precision/increment, selected variant, positions, exact oracle, and intended baseline values. Use the BVA guide and skill listed above.

### Decision Table Testing

**Select Decision Table when:** a complete combination of conditions determines one or more materially different actions or outcomes. Conditions can be input classes, predicates, roles, account state, existing data, time context, or environment values.

**Why:** informal prose often hides missing combinations, contradictory outcomes, defaults, overlapping rules, and precedence. A table exposes the legal rule space and lets the team distinguish positive, invalid, forbidden, and impossible combinations.

**Characteristic signals:**

- an action requires several conditions together;
- different combinations produce different statuses, calculations, permissions, messages, notifications, or persistence;
- rules overlap and precedence or default behavior matters;
- a value is only available conditionally;
- legal and forbidden combinations must be distinguished.

**Example:** Suppose a confirmed policy says that a discount depends on membership, purchase amount, and a valid promotion code. Model those as conditions and `Discount` as an action. Do not infer the result for a combination that the policy does not specify. If amount has numeric thresholds, use EP/BVA to create meaningful condition entries before building the table.

**Do not use Decision Table as the sole technique when:** the main risk is a value edge without joint conditions, a lifecycle sequence, or broad interaction coverage among mostly independent dimensions. Decision tables can coexist with State-Transition when guards determine transitions.

**Important distinction:** `AND` or `OR` in prose is a signal to inspect, not an automatic reason to enumerate a table. Use a decision table only when the joint condition vector changes an observable action or oracle.

**Handoff requirements:** condition IDs and entries, action IDs and outcomes, constraints, legal/forbidden/impossible classification, defaults, precedence, reduction rationale, exact oracles, and rule coverage objective. Use the Decision Table guide and skill listed above.

### State-Transition Testing

**Select State-Transition when:** behavior depends on the current state of one domain object or explicitly bounded lifecycle and an event, guard, or previous history.

**Why:** a value or action can be valid in one state and invalid in another. State-transition modeling exposes missing transitions, invalid events, terminal behavior, loops, retries, timeout, expiration, recovery, duplicate, stale, and out-of-order events.

**Characteristic signals:**

- created/paid/processing/shipped/delivered or analogous lifecycle statuses;
- account activation, suspension, lock, unlock, expiration, or recovery;
- retry count, duplicate submission, callback ordering, or event replay;
- an operation is allowed or forbidden depending on the previous event or current status;
- timers, asynchronous messages, or terminal states alter future behavior.

**Example:** For an explicitly confirmed order lifecycle `Created -> Paid -> Processing -> Shipped -> Delivered`, test permitted transitions and specified behavior for an attempted transition such as `Delivered -> Paid`. The exact rejection, no-op, audit, and persistence oracle must be supplied by the product rule; do not assume one.

**Scope rule:** a GUI screen, URL, or button is not automatically a domain state. Model a state only when it has stable behavior, invariants, available/forbidden operations, data, processing, or observable outcomes. If several objects have independent lifecycles, model them separately unless their composition is specified.

**Handoff requirements:** modeled object and lifecycle boundary, state IDs/invariants, initial/reachable/terminal/error states, event and guard IDs, transitions, constraints, timing/retry semantics, selected state/transition/pair/path coverage, and exact oracles. Use the State-Transition guide and skill listed above.

### Pairwise Testing

**Select Pairwise when:** several parameters have multiple meaningful levels, are mostly independent, and full Cartesian testing is too large or redundant.

**Why:** many interaction defects are exposed by combinations of two factors, while a covering array requires fewer rows than exhaustive testing. Pairwise is a reduction strategy, not a claim that all combinations are tested.

**Characteristic signals:**

- browser × OS × device × locale × payment method;
- supported configurations, feature flags, roles, environments, or API options;
- each factor has several meaningful, legal levels;
- the team explicitly needs 2-way, 3-way, or higher interaction coverage.

**Example:** Four factors with three levels each produce `3 × 3 × 3 × 3 = 81` Cartesian combinations. A legal 2-way covering array can reduce rows while ensuring every required pair of levels across two distinct factors appears at least once. The matrix, constraints, generator metadata, and oracle still require review.

**Required selection discipline:**

1. Use EP to define meaningful levels and BVA for boundary levels where applicable.
2. Classify factors as independent, dependent, derived, state-driven, role-driven, or negative-test dimensions.
3. Formalize allowed, forbidden, conditional, implication, and precedence constraints.
4. Require legal complete-row extension for every required pair.
5. Choose 2-way by default only when risk supports it; select 3-way or higher when evidence, architecture, safety, financial impact, or defect history indicates higher-order risk.
6. Keep invalid rows separate from legal positive pairwise coverage.

**Do not select Pairwise as the sole technique when:** conditions jointly choose business actions, factors are strongly dependent, state/history dominates, or the complete legal space is small and critical enough for exhaustive testing.

**Handoff requirements:** factor IDs, levels, semantic classes, baseline, constraints, legal-completion rules, interaction strength, generation method/seed/version, exact oracle, pair inventory, and coverage denominator. Use the Pairwise guide and skill listed above.

### Error Guessing Test Design

**Select Error Guessing when:** formal requirements are incomplete or experience-based evidence identifies likely failure mechanisms. It is especially valuable for legacy systems, recent changes, known defect clusters, production incidents, common user mistakes, malformed representations, retries, timeouts, permissions, locale, encoding, and integration boundaries.

**Why:** a formal model cannot include every historical or implementation-adjacent failure pattern. Error Guessing adds targeted hypotheses based on evidence and experience.

**Characteristic signals:**

- a previous defect or incident suggests a regression seed;
- the area changed recently or has high complexity/exposure/impact;
- requirements do not specify malformed, missing, duplicate, stale, retry, or recovery behavior;
- a checklist or domain pattern identifies a likely failure;
- there is little time and risk-based, focused heuristics are needed.

**Example:** For an email field, hypotheses may cover missing local part, missing domain, malformed syntax, unsupported Unicode, or whitespace handling if those risks are relevant. For a password field, empty, whitespace-only, very long, special-character, and Unicode inputs may be candidates. These are hypotheses, not universal invalid classes; the exact expected oracle must come from the requirement or be marked `Question/TBD`.

**Required selection discipline:** record the evidence source, suspected mechanism, trigger/context, predicted failure, exact oracle, priority, confidence, reproducible setup, and result status. Do not report an error guess as a confirmed defect until expected-versus-actual evidence supports it.

**Do not use Error Guessing as a replacement for:** EP for known domains, BVA for known edges, Decision Tables for explicit rules, Pairwise for selected independent interactions, or State-Transition for lifecycle behavior.

**Handoff requirements:** evidence IDs, risk-area IDs, falsifiable hypothesis IDs, exact oracle, context, priority, regression links, and residual risks. Use the Error Guessing guide and skill listed above.

## Complementary approaches

### Use Case/Scenario Testing

Use Case/Scenario Testing checks whether an actor can achieve a business goal across a complete workflow, such as registration → email verification → login → profile setup or login → search → cart → checkout → payment → confirmation.

Select it when an end-to-end journey, integration, business goal, or critical user path is important. It does not replace EP/BVA for fields, Decision Tables for conditional rules, Pairwise for configuration interactions, State-Transition for lifecycle paths, or Error Guessing for heuristic risks.

### Exploratory Testing

Use Exploratory Testing when learning, designing, executing, and investigating happen together. It is useful for new functionality, incomplete requirements, unstable behavior, investigation, regression discovery, and unexpected behavior.

Exploratory work should record the charter, scope, observations, evidence, questions, and follow-up risks. It complements formal selection; it does not turn an unknown oracle into a confirmed expected result.

### Non-functional and specialized follow-ups

The six techniques do not by themselves provide security, performance, reliability, accessibility, usability, compatibility, recovery, or observability assurance. Identify explicit follow-up testing for those obligations. Error Guessing is not a substitute for security or attack-based testing.

## Repeatable selection workflow

Use this workflow for every selection request:

1. **Define scope.** Record the operation, actor, modeled object, lifecycle boundary, requirement references, setup, and out-of-scope behavior.
2. **Define the oracle.** State the exact observable result: status, message, value, calculation, persistence, processing path, state, notification, invocation, side-effect count, or timing expectation. If unknown, create `REQ-SIG-UNC-*` and a `Question/TBD`.
3. **Classify evidence.** Mark facts `Confirmed`; label provisional interpretations `Assumption`; list blocking questions; record residual risks.
4. **Extract signals.** Create stable `REQ-SIG-*` IDs for domains, boundaries, rules, states, factors, goals, historical risks, and uncertainties.
5. **Model dependencies and legality.** Identify independent, dependent, derived, state-driven, role-driven, conditional, allowed, forbidden, impossible, and unknown relationships.
6. **Identify the dominant risk.** Select the primary technique that models the most important confirmed structure. Do not select by keyword or by manual/automation preference.
7. **Add secondary techniques.** Add only techniques that address a distinct uncovered risk. Distinguish primary technique, secondary technique, and complementary approach.
8. **Set configuration.** Specify EP partitions, BVA 2/3-value and normal/robust variant, Decision Table exhaustiveness/reduction/default/precedence, Pairwise strength and legal constraints, State-Transition lifecycle scope and selected paths, or Error Guessing evidence/prioritization.
9. **Justify exclusions.** Explain why a technique is not selected or is deferred. Do not call a technique unnecessary merely because another technique was selected.
10. **Prepare hand-offs.** Provide inputs, modeled elements, constraints, exact oracle, traceability, requested output format, and the path to the existing skill.
11. **Review feasibility.** Ensure the next skill can generate executable cases without inventing behavior. Ask blocking questions before claiming a complete selection.
12. **Report limitations.** Separate technique-selection completeness from test-case, model, execution, and non-functional coverage.

## Composition rules and recommended order

Use the following order when several techniques are needed:

1. **Scope and oracle first.** No technique can be applied safely without knowing what is being tested and how pass/fail is observed.
2. **EP before BVA.** EP defines behaviorally distinct partitions; BVA selects values at their ordered edges.
3. **EP/BVA levels before Pairwise.** Pairwise should combine meaningful representatives and boundary values, not arbitrary raw values.
4. **Decision Table for joint condition/action rules.** Use EP/BVA to define condition entries, then enumerate legal rules and exact outcomes.
5. **State-Transition for lifecycle history.** Use EP/BVA for state/event classes or timeout/retry thresholds and Decision Tables for guard combinations when needed.
6. **Pairwise for mostly independent contextual dimensions.** Keep state-driven and strongly dependent behavior in the relevant state/rule model unless the composition is explicit.
7. **Scenario coverage across the selected designs.** Use critical user journeys to validate business goals and integration sequencing.
8. **Error Guessing and Exploratory last, but not optional when risk demands them.** Use formal-model gaps, evidence, and observed behavior to target investigation and likely failures.

A technique can be both secondary and a prerequisite for another technique. For example, EP is a secondary design for a numeric input and supplies the partitions that BVA and Pairwise need. State-Transition and Decision Table can also be composed when guards are rule-heavy; document which model is authoritative for each outcome.

## Worked example: registration feature

The following is an illustrative model, not a product specification. Replace every assumption with confirmed product behavior before execution.

### Requirement fragments and extracted signals

| Signal ID | Illustrative requirement fragment | Signal type | Status |
| --- | --- | --- | --- |
| `REQ-SIG-DOM-REG-001` | Password length is described as 8–20 characters | Domain/range | Assumption |
| `REQ-SIG-BND-REG-001` | Password behavior changes at lengths 8 and 20 | Ordered thresholds | Assumption |
| `REQ-SIG-DOM-REG-002` | Email must match a supported format | Format classes | Assumption |
| `REQ-SIG-RULE-REG-001` | Registration outcome depends on age, country, email verification, and promotion status | Joint conditions | Assumption |
| `REQ-SIG-FACTOR-REG-001` | Supported browser and OS combinations may affect execution | Configuration factors | Assumption |
| `REQ-SIG-STATE-REG-001` | The account is locked after five incorrect passwords | History/threshold | Assumption |
| `REQ-SIG-GOAL-REG-001` | The user must complete registration, verification, login, and profile setup | End-to-end goal | Assumption |
| `REQ-SIG-HIST-REG-001` | Unusual input and recovery behavior need targeted checks | Heuristic risk | Assumption |

### Selection and rationale

| Area | Primary technique | Secondary/complementary technique | Rationale and hand-off |
| --- | --- | --- | --- |
| Password length | EP | BVA | EP models below-range, valid, and above-range classes; BVA selects values around 8 and 20. Confirm character-count unit, endpoint ownership, precision, and exact messages. |
| Email format | EP | Error Guessing | EP models valid and materially different invalid representations. Error Guessing adds evidence-based malformed, whitespace, Unicode, and normalization hypotheses without pretending all have the same oracle. |
| Age/country/verification/promotion rules | Decision Table | EP and BVA | EP/BVA define meaningful condition entries; Decision Table covers legal combinations and resulting registration actions, defaults, conflicts, and precedence. |
| Browser/OS support | Pairwise | EP/BVA where levels have semantic classes or boundaries | Pairwise reduces legal configurations after levels and compatibility constraints are defined. Use 3-way only if evidence supports it; do not hide unsupported combinations. |
| Incorrect-password lockout | State-Transition | BVA and Error Guessing | State-Transition models active/locked/recovery behavior and event history; BVA targets attempts 4/5/6 if the threshold is confirmed; Error Guessing adds duplicate, reset, and recovery hypotheses. |
| Registration journey | Use Case/Scenario | Exploratory | The end-to-end scenario checks the actor goal; exploratory testing investigates unexpected behavior across verification, login, and profile setup. |

### Suggested execution order

1. Confirm exact registration outcomes, error oracles, supported formats, and account-state rules.
2. Model email/password EP classes.
3. Add password BVA around the confirmed limits.
4. Build the registration Decision Table from confirmed condition entries and constraints.
5. Build the browser/OS Pairwise matrix from legal, reproducible levels.
6. Model lockout State-Transition behavior and retry threshold boundaries.
7. Run the end-to-end registration scenario.
8. Run prioritized Error Guessing and Exploratory charters for uncovered or historically risky areas.

This example demonstrates why one feature should not receive one technique indiscriminately. It also demonstrates that “registration” alone is not enough information to generate confirmed cases.

## Selector output and hand-off format

Preserve a requested output format. When no format is requested, return Markdown with these sections:

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

| Field | Value |
| --- | --- |
| Scope ID | `SCOPE-001` |
| Operation/feature |  |
| Actor and role |  |
| Modeled object/lifecycle |  |
| Requirement references |  |
| Exact observable oracle |  |
| Out of scope |  |
| Evidence status | Confirmed / Assumption / Question/TBD |

### Extracted requirement signals

| Signal ID | Type | Requirement evidence | Risk/behavior represented | Status |
| --- | --- | --- | --- | --- |
| `REQ-SIG-DOM-001` | Domain / boundary / rule / state / factor / goal / history / uncertainty |  |  | Confirmed / Assumption / Question/TBD / Residual risk |

### Technique selection and rationale

| Selection ID | Technique | Primary/secondary/complementary | Signal IDs | Why selected | Configuration | Guide | Skill | Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `SEL-001` |  |  |  |  |  |  |  | Confirmed / Assumption / Question/TBD |

For exclusions, use a separate inventory:

| Exclusion ID | Technique | Reason deferred/not selected | Risk left uncovered | Follow-up | Status |
| --- | --- | --- | --- | --- | --- |
| `SEL-EX-001` |  |  |  |  | Residual risk / Question/TBD |

### Composition and execution order

Describe dependencies between selections, such as:

- `SEL-001` EP supplies partitions to `SEL-002` BVA.
- `SEL-001` EP and `SEL-002` BVA supply levels to `SEL-003` Pairwise.
- `SEL-004` Decision Table consumes condition entries from EP/BVA.
- `SEL-005` State-Transition is authoritative for lifecycle state and event outcomes.
- Scenario, Error Guessing, and Exploratory work address cross-cutting or residual risks.

### Clarifications, assumptions, questions, and residual risks

| Item ID | Category | Statement | Affected selection | Owner/follow-up | Status |
| --- | --- | --- | --- | --- | --- |
| `CLAR-001` | Assumption / Question/TBD / Residual risk |  |  |  |  |

### Technique-skill handoff package

Create one hand-off per selected technique:

| Field | Required content |
| --- | --- |
| Selection ID | Stable `SEL-*` ID |
| Technique and skill path | Exact technique name and `.claude/skills/.../SKILL.md` path |
| Requirement/signal references | Requirement IDs and `REQ-SIG-*` IDs |
| Scope and operation | What the next skill must design |
| Exact oracle | Observable pass/fail result; never “works correctly” |
| Modeled elements | Partitions, boundaries, conditions/actions, states/events, or factors/levels |
| Constraints/dependencies | Legal, forbidden, conditional, precedence, timing, and state rules |
| Configuration | BVA variant, Pairwise strength, table reduction, state path scope, or heuristic priority |
| Requested output | Markdown, Gherkin, JSON, CSV, or test-management schema |
| Priority and risks | Business/technical priority and uncovered areas |
| Status | Confirmed / Assumption / Question/TBD / Residual risk |

The selector hands off modeling context; it does not duplicate the detailed executable test cases that the selected skill is responsible for producing.

## Coverage and limitations

Report these metrics separately:

- **Signal identification:** extracted relevant signals / signals evidenced by the supplied material. This is a review aid, not a product-coverage metric.
- **Technique selection traceability:** selected `SEL-*` entries linked to a signal and requirement reference / selected entries.
- **Handoff completeness:** hand-offs containing scope, oracle, modeled elements, constraints, configuration, and output schema / selected hand-offs.
- **Technique-specific model coverage:** calculated later by the selected skill, such as EP partition coverage, BVA boundary-position coverage, Decision Table feasible-rule coverage, Pairwise legal-pair coverage, State-Transition reachable-state/transition coverage, or Error Guessing hypothesis coverage.
- **Execution coverage:** calculated after test cases execute, not at selection time.

Do not claim that selecting all applicable techniques means all tests are covered. Do not claim that 100% of a technique-specific metric proves complete requirements, branch, path, state, higher-order interaction, security, performance, accessibility, compatibility, or reliability coverage.

Common residual risks include:

- unknown or untestable oracles;
- omitted input representations, units, locales, or time zones;
- unmodeled legal or forbidden combinations;
- impossible or unreachable elements not supported by evidence;
- higher-order interactions beyond selected Pairwise strength;
- lifecycle concurrency, event ordering, retries, or eventual consistency;
- untested interior partition values;
- security and non-functional obligations;
- assumptions copied from illustrative examples;
- error hypotheses not backed by current evidence.

## Verification checklist

Before returning a selection, verify:

- [ ] Scope, operation, actor, modeled object, lifecycle boundary, and out-of-scope behavior are explicit.
- [ ] Requirement, acceptance criterion, contract, model, defect, or risk references are recorded.
- [ ] Exact observable oracle is stated, or its absence is labeled `Question/TBD` or `Residual risk`.
- [ ] Stable `REQ-SIG-*`, `SEL-*`, clarification, and risk IDs are used.
- [ ] Facts, assumptions, questions, and residual risks are visibly distinguished.
- [ ] Selection is based on requirement structure and risk, not on manual versus automation execution.
- [ ] EP is selected for behaviorally distinct classes, and partitions are not confused with individual boundary values.
- [ ] BVA is selected only for meaningful ordered edges; EP is modeled first; endpoint ownership, unit, precision, and 2/3-value normal/robust variant are explicit.
- [ ] Decision Table is selected only when joint conditions materially change actions or outcomes; legal, invalid, forbidden, impossible, default, overlap, and precedence behavior is addressed.
- [ ] State-Transition is selected only for domain lifecycle/history behavior; GUI screens are not treated as states without evidence.
- [ ] Pairwise is selected only when multiple mostly independent factors justify interaction reduction; levels, legal constraints, completion, and 2-way/3-way strength are explicit.
- [ ] Error Guessing hypotheses cite evidence or a clearly labeled experience-based rationale and have falsifiable exact oracles.
- [ ] Use Case/Scenario and Exploratory Testing are identified as complementary approaches where appropriate, not falsely substituted for formal technique coverage.
- [ ] Primary, secondary, and complementary selections are distinguished.
- [ ] Composition order and dependencies between selected skills are documented.
- [ ] Excluded techniques and the risks they leave uncovered are visible.
- [ ] Every hand-off contains the selected skill path, signal/requirement traceability, modeled elements, constraints, configuration, oracle, output schema, and priority.
- [ ] No invented product values, states, legal combinations, messages, outcomes, or coverage claims appear as confirmed facts.
- [ ] Selection completeness is not presented as test-case or execution coverage.
- [ ] Security, performance, reliability, accessibility, usability, compatibility, and other explicit non-functional follow-ups are considered.
- [ ] The document points to the detailed technique guide instead of duplicating its full case-generation procedure.
