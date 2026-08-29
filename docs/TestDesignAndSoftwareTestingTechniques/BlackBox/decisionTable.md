# Decision Table Testing

## Scope, requirement basis, and exact oracle

Decision Table Testing is a black-box, specification-based test-design technique. It makes combinations of conditions explicit and maps each complete combination to the actions or outcomes required by a specification. This guide is a practical supplement using ISTQB-consistent terminology. It does not replace the current official ISTQB syllabus, product requirements, acceptance criteria, domain rules, or the test strategy.

A decision table is useful when prose contains several conditions whose combinations select different outcomes. It makes omissions, contradictory rules, overlap, hidden precedence, missing defaults, and infeasible combinations reviewable before or during implementation. A table is a model of the declared decision; it is not proof that every input value, implementation branch, state path, higher-order interaction, or non-functional property is correct.

An exact **test oracle** states what can be observed and compared: for example, HTTP status and error code, an exact validation message, an accepted or rejected result, a calculated amount to a stated precision, persisted values, a processing path, an invoked operation, a notification, or a state change. A vague statement such as “the system behaves as expected” is not an oracle.

When applying this guide to a real requirement, record:

- the operation and requirement or acceptance-criterion references;
- preconditions, setup data, role, account state, lifecycle state, locale, time zone, reference date, environment, and existing data;
- every condition and action that can change the decision;
- exact legal, invalid, forbidden, impossible, default, overlap, and precedence behavior;
- selected scope: exhaustive, reduced, negative, risk-focused, or a combination;
- the evidence and version of any generator or reduction tool.

## Clarifications, assumptions, questions, and residual risks

Use these labels consistently:

- **Confirmed** — stated directly in the supplied requirement, acceptance criterion, contract, or approved domain rule.
- **Assumption** — introduced to make a self-contained model or provisional example possible; it must not be presented as product fact.
- **Question/TBD** — unresolved information requiring confirmation.
- **Residual risk** — a meaningful risk outside the current model or not covered by the selected cases.

Ask a question when the answer changes the condition domain, legality, precedence, action, or oracle, or when a case cannot be executed safely without it. Prioritize:

- missing or ambiguous conditions, entries, actions, and outputs;
- endpoint ownership, range semantics, units, precision, rounding, or date/time context;
- unknown legal, forbidden, impossible, conditional, or mutually exclusive combinations;
- conflicting rules or overlap without confirmed precedence;
- unclear default/otherwise behavior;
- unknown response, message, calculation, persistence, processing-path, invocation, or state-change oracle;
- unclear role, account, lifecycle, locale, time-zone, environment, or existing-data preconditions;
- uncertainty about whether `-` is a genuine don't-care or an unmodeled value.

If clarification is not blocking, continue with explicit **Assumption** and record the resulting **Residual risk**. Never turn an invented condition, action, constraint, precedence, default, or oracle into **Confirmed** behavior.

## What a Decision Table is

A decision table has **condition stubs** (the causes or facts), **action stubs** (the effects), and **rules**. Each rule assigns a value to every modeled condition, or a justified `-` or `N/A`, and maps that condition vector to every materially relevant action outcome.

The conventional orientation places condition and action stubs vertically and rules horizontally as columns:

| Stub type | Rule R1 | Rule R2 |
| --- | --- | --- |
| `COND-1: customer is a member` | `Y` | `N` |
| `COND-2: cart reaches threshold` | `Y` | `Y` |
| `ACTION-1: discount` | `10%` | `0%` |
| `ACTION-2: shipping` | `FREE` | `STANDARD` |

A transposed representation may put rules in rows, but it must say that it is transposed and preserve stable rule IDs. A rule is not automatically an executable test case: add preconditions, setup, complete input data, steps, priority, an exact oracle, and traceability to make it executable.

Decision tables are especially good at showing whether a finite or partitioned decision space is complete and consistent. They are not a substitute for testing values that were never modeled, boundaries, state paths, complex Boolean logic, security, performance, accessibility, reliability, compatibility, or exploratory behavior.

## Terminology and notation

| Term | Definition and required interpretation |
| --- | --- |
| **Condition/cause** | An input, fact, predicate, context value, role, state, or environmental condition that can influence an outcome. |
| **Condition domain** | The complete declared set, enumeration, or partitioned range of values a condition may take. Include materially different valid, invalid, missing, blank, null, malformed, unsupported, or unreadable representations where their behavior differs. |
| **Condition entry/value** | The value assigned to a condition in one rule, such as `Y`, `N`, an enumeration, a range class, a date/time class, or a representative partition. |
| **Condition stub** | The labeled condition row or field that identifies a condition and its meaning. |
| **Action/effect** | An observable response, business outcome, calculation, state change, operation, processing path, or notification selected by a rule. |
| **Action entry/value** | The expected value or execution marker for an action, such as `X`, `Do not apply`, an amount, a status, a message, or a state. |
| **Action stub** | The labeled action row or field that identifies an action and its meaning. |
| **Rule** | One complete condition vector mapped to one consistent set of expected actions. |
| **Rule ID** | A stable identifier that traces a rule to its requirement, constraints, test cases, results, and coverage. |
| **Rule column/row** | The selected table orientation. Conventional tables use columns for rules; a transposed table uses rows only when the orientation is clearly labeled. |
| **Condition-entry notation** | The explicit notation for condition values: `Y`/`N`, limited entries, extended enumerations, ranges, partitions, `-`, or `N/A`. |
| **Action-entry notation** | The explicit notation for an action outcome, status, amount, message, operation, state, or non-execution. |
| **Complete/exhaustive decision table** | A table representing every declared feasible complete combination, subject to documented constraints and exclusions. |
| **Limited-entry table** | A table using a small set of entries, usually Boolean `Y` and `N`. |
| **Extended-entry table** | A table using more than two entries, such as enumerated values or formal ranges. |
| **Don't-care (`-`, `–`)** | The condition may take any permitted value because changing it cannot change any action or oracle for that rule and does not violate a dependency. It represents several concrete combinations. |
| **Not applicable (`N/A`)** | The condition is genuinely undefined or inapplicable in the stated context, such as coupon validity when no coupon exists. It is not a forgotten value. |
| **Unknown** | Information is not known. It must not silently be treated as `-` or `N/A`. |
| **Omitted** | A value was not supplied or is absent from input. It may have behavior different from a valid value. |
| **Untested** | A value or rule exists in the model but no selected test has exercised it. It is a coverage status, not a condition entry. |
| **Blank** | An empty representation, such as an empty string or empty cell, whose meaning must be defined separately from missing, null, or `N/A`. |
| **Legal/feasible rule** | A complete rule that can occur under the declared domain, preconditions, dependencies, and constraints. |
| **Invalid rule** | An intentionally modeled input or combination that should be rejected or handled by a defined error path. |
| **Forbidden rule** | A combination prohibited by a business or system rule. It may need a negative test if it can be supplied externally. |
| **Impossible/infeasible rule** | A combination that cannot occur under the model or dependency rules. Exclude it with a rationale rather than count it as an uncovered legal rule. |
| **Default/otherwise rule** | A deliberately defined fallback for all remaining combinations within the declared scope. It is not an implicit guess. |
| **Overlap** | Two or more rules match the same concrete condition combination. |
| **Precedence** | An explicit priority or ordering that decides which matching rule controls the outcome. Rule order is not precedence unless the requirement says so. |
| **Contradiction/conflict** | Matching rules require incompatible actions or oracles. Report it as a specification defect or resolve it with confirmed precedence. |
| **Completeness** | Every required legal combination has a rule or a justified default/otherwise rule. |
| **Consistency** | Matching rules produce compatible actions and exact oracles. |
| **Reduction/minimization** | Merging equivalent rules with `-` without changing any covered behavior, constraint behavior, or material oracle. |
| **Decision-table coverage** | Coverage reported against a declared unit: rule IDs, condition entries, action outcomes, feasible expansions, constraints, defaults, precedence relationships, or a legal Cartesian space. |
| **Test oracle** | The exact observable result expected for a case and the evidence used to compare actual behavior. |

Every condition and action must have a defined domain or outcome. Every rule must have all materially relevant action entries; an empty action cell is acceptable only when its meaning, such as “not executed,” is explicitly defined. A `-` is neither unknown, omitted, untested, nor a shortcut for a missing requirement. `N/A` is not a substitute for a value that was forgotten. A 100% metric is meaningful only relative to the declared domain, rule set, constraints, exclusions, and reduction.

## Distinguishing related techniques

| Technique | Main purpose | Relationship to Decision Table Testing |
| --- | --- | --- |
| **Equivalence Partitioning (EP)** | Identify behaviorally equivalent valid and invalid classes and representatives. | EP can supply condition entries. One representative does not prove every value in its partition. |
| **Boundary Value Analysis (BVA)** | Test values below, at, and above numeric, length, date/time, size, count, and threshold boundaries. | Use BVA to choose entries where endpoint behavior matters; a table does not replace boundary testing. |
| **Pairwise/all-pairs** | Sample interactions among mostly independent parameters. | Decision tables explicitly model business-condition combinations and actions, including constrained, overlapping, or prioritized rules. Pairwise does not guarantee complete rule coverage. |
| **Full Cartesian/exhaustive coverage** | Execute every legal complete combination. | A reduced table can represent several combinations with `-`, but must show why their outcomes are equivalent. |
| **State-transition testing** | Cover states, events, guards, and lifecycle paths. | A state can be a condition, but a decision table does not replace transition and path coverage. |
| **Condition/cause-effect coverage** | Analyze logical relationships among causes and effects. | Use it when complex Boolean structure needs dedicated logic coverage. A decision table is a practical rule representation. |
| **Use-case/scenario testing** | Cover actor goals and end-to-end flows. | Use cases provide flow context; decision tables provide combination-level decisions inside a flow. |
| **Risk-based, security, performance, accessibility, reliability, usability, and exploratory testing** | Address risks beyond ordinary condition/action rules. | Add these activities for risks not represented in the table. |

## Modeling conditions, actions, and constraints

### Condition model

Model conditions before constructing rules. For each condition, document:

- a stable condition ID and requirement reference;
- semantic meaning, representation, data type, format, and units;
- the complete relevant domain and behaviorally distinct entries;
- valid, invalid, missing, blank, null, malformed, unsupported, and unreadable representations where behavior differs;
- endpoint ownership, precision, rounding, time zone, reference date, and daylight-saving assumptions when relevant;
- role, account state, lifecycle state, locale, environment, existing data, and context;
- dependencies, implications, mutual exclusions, and expected influence on actions;
- **Confirmed**, **Assumption**, or **Question/TBD** status.

Use EP to identify meaningful behavioral classes and BVA to select representatives around thresholds. Do not choose arbitrary values merely to fill a table. A partition representative is evidence for that representative, not proof for every value in the partition.

Classify each condition as mostly independent, dependent/constrained, derived, state/role/context-driven, or an invalid/negative-test dimension. Conditional values must be visible. Use `N/A` only when the condition genuinely does not apply.

### Action model

Model actions independently. For each action, document:

- a stable action ID and requirement reference;
- semantic meaning and every materially distinct outcome;
- possible execution states, values, messages, calculations, statuses, persistence, processing paths, invocations, and state changes;
- exact observable oracle, including units and precision;
- dependencies and mutually exclusive actions;
- **Confirmed**, **Assumption**, or **Question/TBD** status.

An action such as “apply discount” is incomplete. State whether the result is `10%`, a calculated total rounded to cents, a persisted discount code, a response field, and/or an operation invocation.

### Dependencies and constraints

Formalize the rules that affect feasibility before enumerating or reducing the table:

- allowed and forbidden combinations;
- conditional availability of an entry;
- implications and mutual exclusions;
- role, account, lifecycle, locale, time-zone, reference-date, environment, and existing-data dependencies;
- invalid input and rejection behavior;
- default/otherwise behavior;
- overlap and confirmed precedence.

Classify each candidate as legal/feasible, invalid, forbidden, or impossible/infeasible. An impossible combination is not an uncovered legal rule. An externally constructible forbidden payload may deserve a separate negative test with a violated constraint ID and exact error oracle.

### Table semantics, completeness, and reduction

Review the decision-table contract before testing:

1. Every stub has a defined domain or an explicit status.
2. Every rule has a stable ID, complete condition interpretation, and all material action outcomes.
3. Legal rules are feasible under all constraints.
4. Gaps have a rule, an explicit default, or a Question/TBD status.
5. Duplicate rules are merged or explained.
6. Overlap is eliminated, shown to have equivalent actions, or resolved by confirmed precedence.
7. Conflicting actions are reported or resolved; rule order alone does not resolve conflict.
8. A `-` is used only when every concrete expansion has the same actions, exact oracles, and applicable constraints.
9. A reduced rule's expansion is documented and checked independently.
10. Negative rules remain separate from legal positive coverage.

Reduction is a modeling decision, not just a formatting operation. Do not remove high-risk combinations or merge rules with different outcomes to make the table smaller.

## Repeatable Decision Table workflow

1. **Define scope and oracle.** Identify the operation, requirement references, preconditions, and exact observable outcomes.
2. **Extract conditions.** List every input, cause, predicate, representation, role, state, context value, and environmental factor that can affect the decision.
3. **Extract actions.** List every materially distinct action, calculation, status, message, persistence result, processing path, operation, and state change.
4. **Define condition domains.** Use finite values or behaviorally meaningful partitions; formalize ranges, endpoints, precision, formats, units, and context.
5. **Define action entries.** Specify exact values, execution markers, mutually exclusive outcomes, and observable oracles.
6. **Classify dependencies.** Mark independent, dependent, derived, state-driven, role-driven, and context-driven conditions and actions.
7. **Formalize constraints.** Record allowed, forbidden, conditional, implication, mutual-exclusion, precedence, default, and invalid-input rules.
8. **Enumerate the rule space.** Calculate the Cartesian candidate combinations, then classify feasible, invalid, forbidden, and impossible combinations.
9. **Construct the table.** Create stable condition, action, constraint, and rule IDs and map feasible rules to outcomes.
10. **Check completeness.** Find missing legal combinations, gaps, missing defaults, and uncovered condition entries. State intentional exclusions.
11. **Check consistency and overlap.** Detect duplicates, intersections, conflicting actions, hidden precedence, and contradictions.
12. **Reduce safely.** Introduce `-` only when all concrete expansions have identical actions, oracles, and applicable constraints. Record expansion and proof.
13. **Separate negative cases.** Keep invalid or intentionally forbidden cases out of legal positive coverage; identify violated constraints and exact rejection oracles.
14. **Derive executable cases.** Add setup, complete inputs, steps, priority, exact oracle, and traceability.
15. **Execute and compare.** Compare status, message, calculation, persistence, operation, processing path, and state against the rule oracle.
16. **Verify coverage independently.** Recalculate rule IDs, condition entries, action entries, feasible combinations, reductions, exclusions, constraints, defaults, and precedence without relying only on a generator.
17. **Report gaps and risks.** List uncovered feasible rules, excluded/impossible rules, untested constraints, ambiguous rules, assumptions, Question/TBD items, and residual risks.
18. **Revise from evidence.** If supposedly equivalent entries or merged rules behave differently, split the domain or rule, document the defect, and update the model.

## Generators, reduction, and reproducibility

A generator may enumerate, format, or minimize candidate rules. It cannot decide whether the requirement, domains, actions, constraints, precedence, defaults, or oracles are correct. Independently review:

- complete and meaningful condition domains;
- feasibility and invalid/forbidden classifications;
- conditional entries and `N/A` semantics;
- duplicate, missing, illegal, and accidentally coerced values;
- overlap, conflict, defaults, and precedence;
- every reduction and don't-care expansion;
- exact oracles for every rule or outcome class.

Record the tool and version, model, input data, constraints, priorities, reduction settings, seed, and other metadata needed to reproduce the result. A smaller table is not automatically safer, clearer, or easier to diagnose.

## Worked Example 1: Two binary conditions and four complete rules

### Assumed requirement and model

**Assumption E1:** An insurance quote operation calculates an annual premium in USD. `EXPERIENCE` is `Y` when the driver has at least 5 completed years of driving experience on the quote date and `N` when they have fewer than 5. `RECENT_CLAIM` is `Y` when the driver has at least one at-fault claim in the previous 3 years and `N` when there is no at-fault claim in that period. The period is measured by calendar date in UTC. All four combinations are legal. The premium is annual, not monthly.

**Assumption E2:** A successful quote returns HTTP `200`, an exact `annualPremiumUsd` value, and persists that value with the quote. No rounding beyond cents is permitted.

Conditions:

| Condition ID | Meaning | Entries | Status |
| --- | --- | --- | --- |
| `COND-1` | At least 5 completed driving years on the quote date | `Y` / `N` | Assumption E1 |
| `COND-2` | At least one at-fault claim in the previous 3 calendar years | `Y` / `N` | Assumption E1 |

Action:

| Action ID | Meaning | Exact outcomes | Status |
| --- | --- | --- | --- |
| `ACTION-1` | Annual insurance premium | `USD 1,200.00`, `USD 900.00`, `USD 700.00`, or `USD 500.00` according to the rule | Assumption E1/E2 |

### Complete decision table

Rules are columns in the conventional orientation. `Y` and `N` are limited-entry values; no don't-care is used.

| Stub | `R1` | `R2` | `R3` | `R4` |
| --- | --- | --- | --- | --- |
| `COND-1: EXPERIENCE` | `N` | `N` | `Y` | `Y` |
| `COND-2: RECENT_CLAIM` | `Y` | `N` | `Y` | `N` |
| `ACTION-1: annualPremiumUsd` | `1200.00` | `900.00` | `700.00` | `500.00` |
| Feasibility | Legal | Legal | Legal | Legal |
| Test case | `DT1-001` | `DT1-002` | `DT1-003` | `DT1-004` |

### Executable test cases and traceability

Common preconditions: the quote service is available; an authorized test user and a new quote ID exist; the driver history is reset to the data shown; the quote date and claim period are fixed; the database is queryable.

| Test case ID | Rule ID | Complete input data | Action | Exact expected oracle | Priority |
| --- | --- | --- | --- | --- | --- |
| `DT1-001` | `R1` | `completedYears=4`; one at-fault claim dated within the previous 3 years | Submit an annual quote | HTTP `200`; response `annualPremiumUsd=1200.00`; persisted quote premium is `1200.00 USD` | High |
| `DT1-002` | `R2` | `completedYears=4`; no at-fault claim in the previous 3 years | Submit an annual quote | HTTP `200`; response `annualPremiumUsd=900.00`; persisted quote premium is `900.00 USD` | High |
| `DT1-003` | `R3` | `completedYears=5`; one at-fault claim dated within the previous 3 years | Submit an annual quote | HTTP `200`; response `annualPremiumUsd=700.00`; persisted quote premium is `700.00 USD` | High |
| `DT1-004` | `R4` | `completedYears=5`; no at-fault claim in the previous 3 years | Submit an annual quote | HTTP `200`; response `annualPremiumUsd=500.00`; persisted quote premium is `500.00 USD` | High |

Each case traces the assumed requirement to `COND-1`/`COND-2`, an entry vector, `ACTION-1`, a stable rule ID, and a persisted result. A boundary check for exactly 5 completed years, claim-date endpoints, and histories outside the partition requires BVA and additional assumptions; it is not silently proved by these representatives.

### Independently checked coverage

The candidate Cartesian size is `2 × 2 = 4`. All combinations are feasible, so the required legal-rule denominator is `4`. Four distinct rule IDs are executed.

| Metric | Numerator | Denominator | Calculation |
| --- | ---: | ---: | ---: |
| Feasible-rule coverage | 4 executed rule IDs | 4 legal rule IDs | `4 / 4 × 100% = 100%` |
| `COND-1` entry coverage | 2 entries (`Y`, `N`) | 2 entries | `2 / 2 × 100% = 100%` |
| `COND-2` entry coverage | 2 entries (`Y`, `N`) | 2 entries | `2 / 2 × 100% = 100%` |
| `ACTION-1` outcome coverage | 4 premium outcomes | 4 modeled outcomes | `4 / 4 × 100% = 100%` |
| Full legal Cartesian coverage | 4 concrete combinations | 4 feasible combinations | `4 / 4 × 100% = 100%` |

**Residual risk:** This is 100% coverage of the declared two-condition model only. It does not prove every driving-history value, the exact five-year and claim-period boundaries, fraud, missing or malformed data, state/context behavior, implementation branches, or non-functional properties.

## Worked Example 2: Multi-condition pricing with safe reduction

### Assumed requirement and domains

**Assumption E2-1:** `POST /orders/preview` accepts an order whose spend for the current calendar month is measured in USD to cents. The buyback percentage is measured to two decimal places. The service returns a preview and does not persist an order. Every declared combination is legal.

| Condition ID | Meaning and formal entries | Status |
| --- | --- | --- |
| `COND-21` `SPEND_TIER` | `S1 < USD 100`; `S2 [USD 100, USD 499.99]`; `S3 [USD 500, USD 999.99]`; `S4 >= USD 1,000` | Assumption E2-1 |
| `COND-22` `BUYBACK_TIER` | `B1 < 5%`; `B2 [5%, 29.99%]`; `B3 [30%, 79.99%]`; `B4 >= 80%` | Assumption E2-1 |
| `COND-23` `LOYALTY_MEMBER` | `Y` or `N` for the customer at preview time | Assumption E2-1 |

Intervals are mutually exclusive and exhaustive for the declared spend and percentage domains. Endpoints are owned exactly as shown; the reference period is the current UTC calendar month.

Actions and exact values:

| Action ID | Action | Exact entry/oracle |
| --- | --- | --- |
| `ACTION-21` | Discount percentage | `S1/S2 → 0%`; `S3 + N → 5%`; `S3 + Y → 10%`; `S4 + N → 10%`; `S4 + Y → 15%` |
| `ACTION-22` | Free item quantity | `B1 → 0`; `B2 → 2`; `B3 → 5`; `B4 → 10` |
| `ACTION-23` | Shipping method | `STANDARD` when free items `<5`; `FREE_STANDARD` when free items `>=5` |

The exact positive oracle is HTTP `200`; JSON fields `discountPercent`, `freeItemQuantity`, and `shippingMethod` equal the action entries; the preview total is calculated from the submitted order, discount, and tax rules to cents; and no order record is persisted.

### Candidate space and reduction

The unchecked candidate Cartesian size is `4 × 4 × 2 = 32`. **Assumption E2-2:** all 32 candidates are feasible. Reduction is safe only for `S1` and `S2`: for every buyback tier and either loyalty value, the complete action vector is identical (`0%`, the buyback quantity, and the derived shipping method). Therefore loyalty is `-` only in those eight concrete expansions. Loyalty remains explicit for `S3` and `S4` because it changes `ACTION-21`; buyback remains explicit because it changes `ACTION-22` and possibly `ACTION-23`.

The reduced inventory contains 24 rules: 4 for `S1` with loyalty `-`, 4 for `S2` with loyalty `-`, 8 singleton rules for `S3`, and 8 singleton rules for `S4`.

| Rule IDs | `SPEND_TIER` | `BUYBACK_TIER` | `LOYALTY_MEMBER` | Expansion count |
| --- | --- | --- | --- | ---: |
| `R2-01`–`R2-04` | `S1` | `B1`–`B4` | `-` | 2 each |
| `R2-05`–`R2-08` | `S2` | `B1`–`B4` | `-` | 2 each |
| `R2-09`–`R2-12` | `S3` | `B1`–`B4` | `N` | 1 each |
| `R2-13`–`R2-16` | `S3` | `B1`–`B4` | `Y` | 1 each |
| `R2-17`–`R2-20` | `S4` | `B1`–`B4` | `N` | 1 each |
| `R2-21`–`R2-24` | `S4` | `B1`–`B4` | `Y` | 1 each |

The range notation in the grouped rows is an inventory shorthand; each stable ID identifies one buyback tier. For example, `R2-01 = S1,B1,-`, `R2-02 = S1,B2,-`, and `R2-24 = S4,B4,Y`.

Expansion proof for `R2-01`:

| Concrete expansion | Discount | Free items | Shipping | Why equivalent |
| --- | ---: | ---: | --- | --- |
| `S1,B1,N` | 0% | 0 | `STANDARD` | `S1` fixes discount; `B1` fixes quantity and shipping |
| `S1,B1,Y` | 0% | 0 | `STANDARD` | Loyalty does not affect any action in `S1` |

The same proof applies to `R2-02` through `R2-08`: each expands over `LOYALTY_MEMBER=N/Y` while retaining the same complete action vector. No diagonal row is treated as coverage for unrelated buyback or spend values.

### Executable reduced-rule cases

Common preconditions: the preview endpoint is available; an authorized customer and a new order payload exist; monthly spend and loyalty status are established before the call; no order is persisted by preview. In every row, choose a concrete amount inside the stated tier and a concrete percentage inside the stated buyback tier, not an unverified endpoint.

| Test case ID | Rule ID | Complete representative inputs (`spend`, `buyback`, `loyalty`) | Exact expected result |
| --- | --- | --- | --- |
| `DT2-001` | `R2-01` | `USD 50.00`, `2.00%`, `N` | HTTP `200`; `discountPercent=0`; `freeItemQuantity=0`; `shippingMethod=STANDARD`; total is exact to cents; no persistence |
| `DT2-002` | `R2-02` | `USD 50.00`, `10.00%`, `N` | HTTP `200`; `discountPercent=0`; `freeItemQuantity=2`; `shippingMethod=STANDARD`; total exact; no persistence |
| `DT2-003` | `R2-03` | `USD 50.00`, `50.00%`, `Y` | HTTP `200`; `discountPercent=0`; `freeItemQuantity=5`; `shippingMethod=FREE_STANDARD`; total exact; no persistence |
| `DT2-004` | `R2-04` | `USD 50.00`, `90.00%`, `Y` | HTTP `200`; `discountPercent=0`; `freeItemQuantity=10`; `shippingMethod=FREE_STANDARD`; total exact; no persistence |
| `DT2-005` | `R2-05` | `USD 250.00`, `2.00%`, `N` | HTTP `200`; `discountPercent=0`; `freeItemQuantity=0`; `shippingMethod=STANDARD`; total exact; no persistence |
| `DT2-006` | `R2-06` | `USD 250.00`, `10.00%`, `N` | HTTP `200`; `discountPercent=0`; `freeItemQuantity=2`; `shippingMethod=STANDARD`; total exact; no persistence |
| `DT2-007` | `R2-07` | `USD 250.00`, `50.00%`, `Y` | HTTP `200`; `discountPercent=0`; `freeItemQuantity=5`; `shippingMethod=FREE_STANDARD`; total exact; no persistence |
| `DT2-008` | `R2-08` | `USD 250.00`, `90.00%`, `Y` | HTTP `200`; `discountPercent=0`; `freeItemQuantity=10`; `shippingMethod=FREE_STANDARD`; total exact; no persistence |
| `DT2-009` | `R2-09` | `USD 750.00`, `2.00%`, `N` | HTTP `200`; `discountPercent=5`; `freeItemQuantity=0`; `shippingMethod=STANDARD`; total exact to cents; no persistence |
| `DT2-010` | `R2-10` | `USD 750.00`, `10.00%`, `N` | HTTP `200`; `discountPercent=5`; `freeItemQuantity=2`; `shippingMethod=STANDARD`; total exact to cents; no persistence |
| `DT2-011` | `R2-11` | `USD 750.00`, `50.00%`, `N` | HTTP `200`; `discountPercent=5`; `freeItemQuantity=5`; `shippingMethod=FREE_STANDARD`; total exact to cents; no persistence |
| `DT2-012` | `R2-12` | `USD 750.00`, `90.00%`, `N` | HTTP `200`; `discountPercent=5`; `freeItemQuantity=10`; `shippingMethod=FREE_STANDARD`; total exact to cents; no persistence |
| `DT2-013` | `R2-13` | `USD 750.00`, `2.00%`, `Y` | HTTP `200`; `discountPercent=10`; `freeItemQuantity=0`; `shippingMethod=STANDARD`; total exact to cents; no persistence |
| `DT2-014` | `R2-14` | `USD 750.00`, `10.00%`, `Y` | HTTP `200`; `discountPercent=10`; `freeItemQuantity=2`; `shippingMethod=STANDARD`; total exact to cents; no persistence |
| `DT2-015` | `R2-15` | `USD 750.00`, `50.00%`, `Y` | HTTP `200`; `discountPercent=10`; `freeItemQuantity=5`; `shippingMethod=FREE_STANDARD`; total exact to cents; no persistence |
| `DT2-016` | `R2-16` | `USD 750.00`, `90.00%`, `Y` | HTTP `200`; `discountPercent=10`; `freeItemQuantity=10`; `shippingMethod=FREE_STANDARD`; total exact to cents; no persistence |
| `DT2-017` | `R2-17` | `USD 1,500.00`, `2.00%`, `N` | HTTP `200`; `discountPercent=10`; `freeItemQuantity=0`; `shippingMethod=STANDARD`; total exact to cents; no persistence |
| `DT2-018` | `R2-18` | `USD 1,500.00`, `10.00%`, `N` | HTTP `200`; `discountPercent=10`; `freeItemQuantity=2`; `shippingMethod=STANDARD`; total exact to cents; no persistence |
| `DT2-019` | `R2-19` | `USD 1,500.00`, `50.00%`, `N` | HTTP `200`; `discountPercent=10`; `freeItemQuantity=5`; `shippingMethod=FREE_STANDARD`; total exact to cents; no persistence |
| `DT2-020` | `R2-20` | `USD 1,500.00`, `90.00%`, `N` | HTTP `200`; `discountPercent=10`; `freeItemQuantity=10`; `shippingMethod=FREE_STANDARD`; total exact to cents; no persistence |
| `DT2-021` | `R2-21` | `USD 1,500.00`, `2.00%`, `Y` | HTTP `200`; `discountPercent=15`; `freeItemQuantity=0`; `shippingMethod=STANDARD`; total exact to cents; no persistence |
| `DT2-022` | `R2-22` | `USD 1,500.00`, `10.00%`, `Y` | HTTP `200`; `discountPercent=15`; `freeItemQuantity=2`; `shippingMethod=STANDARD`; total exact to cents; no persistence |
| `DT2-023` | `R2-23` | `USD 1,500.00`, `50.00%`, `Y` | HTTP `200`; `discountPercent=15`; `freeItemQuantity=5`; `shippingMethod=FREE_STANDARD`; total exact to cents; no persistence |
| `DT2-024` | `R2-24` | `USD 1,500.00`, `90.00%`, `Y` | HTTP `200`; `discountPercent=15`; `freeItemQuantity=10`; `shippingMethod=FREE_STANDARD`; total exact to cents; no persistence |

These cases identify every stable reduced rule ID with complete concrete `B` inputs and exact action outcomes; the first eight rows also show the `-` expansion representatives. Run the unselected loyalty expansions when concrete exhaustive execution is required.

### Coverage arithmetic and residual risk

| Metric | Numerator | Denominator | Calculation |
| --- | ---: | ---: | --- |
| Candidate Cartesian size | 32 candidate vectors | 32 | `4 × 4 × 2 = 32` |
| Feasible-rule denominator | 32 feasible vectors | 32 | All candidates legal by E2-2 |
| Reduced rule-ID coverage | 24 executed reduced IDs | 24 reduced IDs | `24 / 24 × 100% = 100%` |
| Concrete representative execution | 24 representatives | 32 feasible vectors | `24 / 32 × 100% = 75%` |
| Reduced expansion representation | 32 represented expansions | 32 feasible vectors | `32 / 32 × 100%` |
| Spend entry coverage | 4 entries | 4 | `4 / 4 × 100%` |
| Buyback entry coverage | 4 entries | 4 | `4 / 4 × 100%` |
| Loyalty entry coverage | 2 entries | 2 | `2 / 2 × 100%` |
| Discount outcome coverage | `0%, 5%, 10%, 15%` | 4 outcomes | `4 / 4 × 100%` |
| Free-item outcome coverage | `0, 2, 5, 10` | 4 outcomes | `4 / 4 × 100%` |
| Shipping outcome coverage | `STANDARD`, `FREE_STANDARD` | 2 outcomes | `2 / 2 × 100%` |

Three loyalty expansions of `S1` and three of `S2` are not concrete executions if only one representative is run per reduced rule; list them as unexecuted concrete vectors, not as missing reduced rules. **Residual risk:** endpoints, malformed representations, calculations not modeled as conditions, persistence after final order creation, and higher-order or non-functional behavior need complementary tests.

## Worked Example 3: Constraints, overlap, precedence, default, and negative behavior

### Assumed coupon decision

**Assumption E3-1:** A checkout authorization operation receives a coupon field and customer context. `COUPON_VALID` is evaluated only when a coupon is supplied. `N/A` is the only valid entry when no coupon is supplied. The member tier is `Basic`, `Premium`, or `VIP`; the cart may or may not meet the coupon threshold; and the account may or may not be suspended.

| Condition ID | Meaning and entries | Status |
| --- | --- | --- |
| `COND-31` `COUPON_SUPPLIED` | `Y` / `N` | Assumption E3-1 |
| `COND-32` `COUPON_VALID` | `Y` / `N` when supplied; `N/A` when not supplied | Assumption E3-1 |
| `COND-33` `MEMBER_TIER` | `Basic` / `Premium` / `VIP` | Assumption E3-1 |
| `COND-34` `CART_MEETS_THRESHOLD` | `Y` / `N` | Assumption E3-1 |
| `COND-35` `ACCOUNT_SUSPENDED` | `Y` / `N` | Assumption E3-1 |

Constraints and precedence:

| Constraint ID | Formal rule | Classification and consequence |
| --- | --- | --- |
| `C-CON-1` | `COUPON_SUPPLIED=N ⇒ COUPON_VALID=N/A` | `N + Y` and `N + N` are impossible internal states. If an external payload supplies them, reject it as an invalid payload. |
| `C-CON-2` | `ACCOUNT_SUSPENDED=Y` takes precedence over all eligibility decisions | Deny with HTTP `403`, code `ACCOUNT_SUSPENDED`; do not mutate the order. |
| `C-CON-3` | A VIP-specific valid-threshold rule takes precedence over the broad valid-threshold rule | VIP receives the VIP outcome, not the broad outcome. |
| `C-CON-4` | `COUPON_SUPPLIED=N` with `COUPON_VALID=N/A` is the default coupon path when not suspended | Accept with no discount and standard shipping. |

**Assumption E3-2:** The exact oracles are:

- suspended: HTTP `403`, code `ACCOUNT_SUSPENDED`, exact message `Account is suspended`, no discount, no order mutation;
- supplied but invalid coupon: HTTP `422`, code `INVALID_COUPON`, exact message `Coupon is invalid`, no discount, no order mutation;
- no-coupon default: HTTP `200`, `discountPercent=0`, `shippingMethod=STANDARD`, no coupon persisted;
- valid coupon below threshold: HTTP `200`, `discountPercent=0`, `shippingMethod=STANDARD`, coupon accepted and persisted;
- valid coupon at threshold: HTTP `200`, coupon persisted, total recalculated to cents, and discount is `10%` for Basic, `15%` for Premium, or `20%` for VIP.

### Feasibility, overlap, and canonical rules

For a supplied coupon there are two validity entries (`Y`, `N`); for no coupon there is one conditional entry (`N/A`). The legal condition-vector count is therefore `(2 + 1) × 3 × 2 × 2 = 36`.

Two externally constructible invalid payloads are intentionally kept outside that denominator:

- `COUPON_SUPPLIED=N, COUPON_VALID=Y` violates `C-CON-1`;
- `COUPON_SUPPLIED=N, COUPON_VALID=N` violates `C-CON-1`.

The source rules below demonstrate an overlap. `OVL-1` says “valid coupon, threshold met, non-suspended, any member tier → 10%.” `OVL-2` says “valid coupon, threshold met, non-suspended, VIP → 20%.” A VIP vector matches both. Confirmed `C-CON-3` makes `OVL-2` authoritative. The canonical executable rules normalize that intersection into disjoint Basic, Premium, and VIP rules; they do not hide the conflict.

| Rule ID | `COUPON_SUPPLIED` | `COUPON_VALID` | `MEMBER_TIER` | `CART_MEETS_THRESHOLD` | `ACCOUNT_SUSPENDED` | Outcome/expansions |
| --- | --- | --- | --- | --- | --- | ---: |
| `R3-SUSP` | `-` | `-` subject to conditional domain | `-` | `-` | `Y` | HTTP `403`; 18 legal expansions |
| `R3-DEFAULT` | `N` | `N/A` | `-` | `-` | `N` | HTTP `200`, 0%, standard; 6 expansions |
| `R3-INVALID` | `Y` | `N` | `-` | `-` | `N` | HTTP `422 INVALID_COUPON`; 6 expansions |
| `R3-BELOW` | `Y` | `Y` | `-` | `N` | `N` | HTTP `200`, 0%, standard; 3 expansions |
| `R3-BASIC` | `Y` | `Y` | `Basic` | `Y` | `N` | HTTP `200`, 10%; 1 expansion |
| `R3-PREMIUM` | `Y` | `Y` | `Premium` | `Y` | `N` | HTTP `200`, 15%; 1 expansion |
| `R3-VIP` | `Y` | `Y` | `VIP` | `Y` | `N` | HTTP `200`, 20%; 1 expansion |

Here `-` in `R3-SUSP` means any value from a legal expansion, not an arbitrary invalid payload. Its 18 expansions are `(3 × 2 × 2) × 1` across coupon state, tier, and threshold. `R3-DEFAULT` expands to `3 × 2 = 6` tier/threshold combinations; `R3-INVALID` also expands to 6; `R3-BELOW` expands to 3. The remaining three threshold-valid tier vectors are singletons. The expansion counts sum to `18 + 6 + 6 + 3 + 1 + 1 + 1 = 36`.

The default rule does not overlap coupon rules because it requires `COUPON_SUPPLIED=N` and `COUPON_VALID=N/A`, while coupon validity rules require `COUPON_SUPPLIED=Y`. The suspension rule intentionally overlaps every legal business vector, but `C-CON-2` supplies confirmed precedence and its denial oracle is compatible with no other action.

### Executable positive and negative cases

Common preconditions: checkout is available; the test account is authorized; the order ID is new; the database and audit log can be inspected; member tier, cart threshold, suspension state, and coupon payload are set before submission.

| Test case ID | Rule ID | Complete input data | Steps/actions | Exact oracle | Priority |
| --- | --- | --- | --- | --- | --- |
| `DT3-001` | `R3-SUSP` | `supplied=Y`, `valid=Y`, `tier=VIP`, `threshold=Y`, `suspended=Y` | Submit checkout authorization | HTTP `403`; code `ACCOUNT_SUSPENDED`; message `Account is suspended`; no discount and no order mutation | Critical |
| `DT3-002` | `R3-DEFAULT` | `supplied=N`, `valid=N/A`, `tier=Premium`, `threshold=Y`, `suspended=N` | Submit checkout authorization | HTTP `200`; `discountPercent=0`; `shippingMethod=STANDARD`; no coupon persisted and order remains unchanged except accepted checkout result | High |
| `DT3-003` | `R3-INVALID` | `supplied=Y`, `valid=N`, `tier=Basic`, `threshold=Y`, `suspended=N` | Submit checkout authorization | HTTP `422`; code `INVALID_COUPON`; message `Coupon is invalid`; no discount and no order mutation | High |
| `DT3-004` | `R3-BELOW` | `supplied=Y`, `valid=Y`, `tier=Basic`, `threshold=N`, `suspended=N` | Submit checkout authorization | HTTP `200`; `discountPercent=0`; `shippingMethod=STANDARD`; supplied coupon is persisted as accepted; total is unchanged to cents | High |
| `DT3-005` | `R3-BASIC` | `supplied=Y`, `valid=Y`, `tier=Basic`, `threshold=Y`, `suspended=N` | Submit checkout authorization | HTTP `200`; `discountPercent=10`; coupon persisted; total recalculated exactly to cents | High |
| `DT3-006` | `R3-PREMIUM` | `supplied=Y`, `valid=Y`, `tier=Premium`, `threshold=Y`, `suspended=N` | Submit checkout authorization | HTTP `200`; `discountPercent=15`; coupon persisted; total recalculated exactly to cents | High |
| `DT3-007` | `R3-VIP` | `supplied=Y`, `valid=Y`, `tier=VIP`, `threshold=Y`, `suspended=N` | Submit checkout authorization | HTTP `200`; `discountPercent=20`; VIP precedence applies; coupon persisted; total recalculated exactly to cents | Critical |
| `DT3-NEG-01` | `INVALID-C-CON-1` | External payload `supplied=N`, `valid=Y`, `tier=Basic`, `threshold=N`, `suspended=N` | Submit the payload without client-side normalization | HTTP `400`; code `INVALID_CONDITIONAL_FIELD`; exact message `Coupon validity is not applicable when no coupon is supplied`; no checkout session or order mutation; violated `C-CON-1` | High |
| `DT3-NEG-02` | `INVALID-C-CON-1` | External payload `supplied=N`, `valid=N`, `tier=Basic`, `threshold=N`, `suspended=N` | Submit the payload without client-side normalization | HTTP `400`; code `INVALID_CONDITIONAL_FIELD`; exact message `Coupon validity is not applicable when no coupon is supplied`; no checkout session or order mutation; violated `C-CON-1` | High |

The positive cases are representatives of reduced legal rules. Their seven executions cover 7 reduced IDs but only 7 concrete legal vectors. If all 36 legal expansions must execute, derive additional cases from each expansion and report that separately.

### Coverage and gaps

| Coverage ID | Metric | Numerator | Denominator | Calculation/notes |
| --- | --- | ---: | ---: | --- |
| `COV3-1` | Legal rule representation | 36 represented expansions | 36 legal vectors | `36 / 36 × 100%`; table representation, not execution |
| `COV3-2` | Reduced rule-ID execution | 7 executed IDs | 7 reduced IDs | `7 / 7 × 100%` |
| `COV3-3` | Concrete positive execution | 7 representatives | 36 legal vectors | `7 / 36 × 100% = 19.4%` (rounded to one decimal) |
| `COV3-4` | Condition entries | `2 + 3 + 3 + 2 + 2 = 12` entries | 12 modeled entries | `12 / 12 × 100%` across `COND-31` to `COND-35` |
| `COV3-5` | Action outcomes | 7 distinct outcome classes | 7 modeled outcome classes | `7 / 7 × 100%` across denial/default/below/tier actions |
| `COV3-6` | Selected invalid payloads | 2 negative cases | 2 selected invalid cases | `2 / 2 × 100%`, separate from legal coverage |
| `COV3-7` | Constraints/default/precedence | `C-CON-1`–`C-CON-4` exercised | 4 important rules | `4 / 4 × 100%` |

The two `C-CON-1` payloads are externally invalid; the corresponding internal combinations are impossible and are listed with rationale rather than counted as uncovered legal rules. Uncovered concrete legal expansions include unsuspended combinations not selected as representatives; they are not missing reduced rules. **Residual risk:** coupon format and expiry, exact threshold boundaries, tier changes during checkout, replay/idempotency, authorization paths beyond suspension, state transitions, security abuse, and non-functional behavior require complementary coverage.

## Coverage definitions and reporting

Use deduplicated stable IDs and state each denominator. At minimum report:

1. **Condition-entry/value coverage:** required condition entries exercised at least once divided by all required entries.
2. **Action-entry/outcome coverage:** required action values, execution states, and observable outcomes exercised at least once divided by all modeled outcomes.
3. **Required feasible-rule coverage:** `covered feasible rule IDs / total required feasible rule IDs × 100%`.
4. **Selected invalid/forbidden-rule coverage:** intentionally exercised invalid or forbidden cases divided by selected negative cases, reported separately from legal positive coverage.
5. **Full legal Cartesian coverage:** executed legal complete combinations divided by all declared feasible complete combinations, reported only when exhaustive execution was intended.
6. **Reduced-rule expansion coverage:** concrete combinations represented by justified generalized rules divided by the feasible combinations those rules are intended to represent.
7. **Constraint/default/precedence coverage:** selected rules exercising each important constraint, default, and precedence relationship divided by the declared relationships.
8. **Row/rule count and metadata:** total candidates, feasible rules, reduced rules, executed rows, excluded rules, tool/version, model, constraints, priority, and reduction method.

Always list:

- uncovered feasible rules and concrete expansions;
- excluded, impossible, or forbidden combinations and their rationale;
- untested constraints, defaults, and precedence relationships;
- each reduction and its expansion/equivalence rationale;
- assumptions and Question/TBD items;
- residual risks and complementary tests.

Do not imply that 100% decision-table rule coverage proves all values, all unmodeled combinations, all requirements, implementation branches, state paths, higher-order interactions, security, performance, accessibility, usability, reliability, compatibility, or exploratory behavior.

## When to use Decision Table Testing

Use decision tables when:

- several conditions jointly select different actions or outcomes;
- discounts, eligibility, authorization, pricing, routing, limits, notifications, or order rules are difficult to interpret as prose;
- omissions, contradictions, overlap, precedence, exceptions, or defaults are plausible;
- domains are finite or can be partitioned into behaviorally distinct classes;
- a reviewable trace from requirements to executable tests is needed.

Use EP and BVA to establish meaningful entries first. Split a table when unrelated decisions have different scope or oracles, or when the table is too dense to review. Do not force a decision table onto a purely numeric boundary problem, a lifecycle graph, an unordered input with no condition/action rule, or many mostly independent configuration dimensions better handled by Pairwise testing.

## Limitations and common mistakes

Decision Tables alone are insufficient for:

- values and representations not modeled as entries;
- malformed, null, missing, blank, unsupported, unreadable, or overflow input unless explicitly modeled;
- numeric, length, date/time, file-size, count, and quota boundaries unless BVA representatives are selected;
- lifecycle paths and state transitions;
- complex Boolean logic requiring dedicated condition/cause-effect analysis;
- broad mostly independent interactions where Pairwise or stronger combinatorial testing is more appropriate;
- implementation branches not represented by requirements;
- security, performance, reliability, usability, accessibility, compatibility, and exploratory risks.

Avoid these mistakes:

1. Treating a rule as an automatically executable test without setup, complete data, steps, priority, oracle, and traceability.
2. Leaving rule orientation ambiguous or losing IDs when transposing a table.
3. Omitting condition entries or material action outcomes.
4. Calling unknown, omitted, or untested values `-` or `N/A`.
5. Using `N/A` for missing information rather than genuine inapplicability.
6. Counting impossible combinations as uncovered legal rules.
7. Mixing invalid negative cases into legal positive coverage.
8. Allowing overlap without confirmed precedence.
9. Merging rules with different outcomes or without checking every expansion.
10. Leaving default behavior implicit.
11. Choosing arbitrary values instead of requirement entries, EP classes, BVA representatives, or explicit assumptions.
12. Assuming a fixed number of rules or tests is universally sufficient.
13. Using vague expected results.
14. Confusing rule coverage with full Cartesian, requirement, branch, state, or non-functional coverage.
15. Trusting a generator or visual table without independent review.
16. Keeping one huge table for unrelated decisions.
17. Removing high-risk cases solely to minimize the row count.

## Complementary techniques

Combine Decision Table Testing with:

- **Equivalence Partitioning** for valid, invalid, and behaviorally distinct condition entries;
- **Boundary Value Analysis** for lower, upper, threshold, and transition representatives;
- **Pairwise testing** for many mostly independent parameters when exhaustive rule modeling is too large;
- **State-transition testing** for states, events, guards, and lifecycle paths;
- **Condition/cause-effect coverage** for complex logical relationships;
- **Use-case/scenario testing** for actor goals and end-to-end flows;
- **Error guessing** for malformed, unusual, coercion, locale, historical, and likely-user-error cases;
- **Risk-based testing** for safety, financial, authorization, security, compatibility, and high-impact rules;
- **Exploratory testing** for behavior outside the formal model.

A practical sequence is: model partitions with EP, choose threshold entries with BVA, express explicit business combinations in a decision table, validate constraints and precedence, use Pairwise for mostly independent dimensions, use state-transition testing for lifecycle behavior, and add risk-based and exploratory tests for residual risks.

## Reusable templates

Copy and adapt these templates. Replace every blank with a real value or an explicit status; do not leave a placeholder as if it were product behavior.

### Condition model template

| Condition ID | Meaning | Representation/type | Domain and entries | Endpoint/context assumptions | Expected influence/oracle | Dependencies | Requirement reference | Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `COND-1` |  |  |  |  |  |  |  | Confirmed / Assumption / Question/TBD |

### Action model template

| Action ID | Meaning | Possible entries/outcomes | Exact observable oracle | Dependencies/mutual exclusions | Requirement reference | Status |
| --- | --- | --- | --- | --- | --- | --- |
| `ACTION-1` |  |  |  |  |  | Confirmed / Assumption / Question/TBD |

### Constraint and precedence model

| Constraint ID | Formal rule | Allowed/forbidden or conditional behavior | Affected conditions/actions | Default/precedence/conflict impact | Observable consequence | Requirement reference | Status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `CONSTRAINT-1` |  |  |  |  |  |  | Confirmed / Assumption / Question/TBD |

### Decision-rule matrix

State the orientation before the table. The following execution-friendly form uses one row per rule:

| Rule ID | `COND-1` | `COND-2` | `COND-3` | `ACTION-1` | `ACTION-2` | Feasibility/status | Constraint IDs | Covered cases | Requirement reference | Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `RULE-1` |  |  |  |  |  | Legal / invalid / impossible / default |  |  |  | Confirmed / Assumption / Question/TBD |

For a conventional column-oriented table, keep condition and action stubs vertical and give every rule column a stable ID. For an intentional invalid case, include violated constraint IDs and an exact rejection/error outcome. For an impossible case, record exclusion rationale rather than inventing a positive test.

### Executable Decision Table test case

| Test case ID | Rule ID | Title/objective | Requirement reference | Priority | Preconditions/setup | Complete input data | Steps/actions | Exact expected result/oracle | Covered condition IDs/entries | Covered action IDs/outcomes | Violated constraint IDs | Technique tags | Assumptions/notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `DT-001` | `RULE-1` |  |  | High/Medium/Low |  |  | 1.  2.  3.  |  |  |  |  | Decision Table / EP / BVA / negative |  |

Every positive case must use a feasible complete rule or documented expansion of a reduced rule. Every intentional invalid case must identify its violated constraint and exact rejection/status oracle.

### Coverage and gap template

| Coverage ID | Metric | Required denominator | Exercised numerator | Percentage | Exclusions/uncovered items | Evidence/notes |
| --- | --- | --- | --- | --- | --- | --- |
| `COV-1` | Feasible-rule coverage |  |  |  |  |  |

Also record candidate Cartesian size, feasible denominator, reduced-rule count, concrete expansion count, condition-entry and action-outcome denominators, selected negative denominator, constraint/default/precedence coverage, tool/version, model, seed, constraints, priorities, and reduction metadata when applicable.

### Risk and residual-risk inventory

| Risk ID | Uncovered or weakly modeled area | Reason not covered | Impact/priority | Complementary technique or follow-up | Status |
| --- | --- | --- | --- | --- | --- |
| `RISK-1` |  |  |  |  | Residual risk / Question/TBD |

## Verification checklist

Before approving a Decision Table design, confirm:

- [ ] Scope, operation, requirement basis, preconditions, and exact observable oracle are documented.
- [ ] Every condition has a stable ID, semantic meaning, representation, and declared domain.
- [ ] Every action has a stable ID, possible outcomes, and exact oracle.
- [ ] Endpoint ownership, units, precision, formats, locale, time zone, state, role, and context assumptions are explicit where relevant.
- [ ] Valid, invalid, missing, blank, null, malformed, unsupported, and unreadable entries are considered when behavior differs.
- [ ] Entries are traceable to the requirement, EP, BVA, or an explicit assumption.
- [ ] Action entries are complete, compatible, and traceable.
- [ ] Table orientation is explicit and rule IDs remain stable if transposed.
- [ ] `Y`, `N`, enumerations, ranges, `-`, `N/A`, unknown, omitted, blank, and untested have distinct meanings.
- [ ] Dependencies, allowed/forbidden combinations, conditional entries, implications, and mutual exclusions are formalized.
- [ ] Every required legal rule is feasible and has a complete condition/action interpretation.
- [ ] Impossible combinations have exclusion rationales and are not counted as uncovered legal rules.
- [ ] Intentional invalid rules have violated constraint IDs and exact rejection/status oracles.
- [ ] The legal space is complete or has an explicit default/otherwise rule and visible gaps.
- [ ] Duplicate, overlapping, and conflicting rules are resolved or documented with confirmed precedence.
- [ ] Any `-` reduction proves identical actions, oracles, and applicable constraints for every expansion.
- [ ] Reduction rationale and expansion coverage are traceable.
- [ ] Each selected rule maps to an executable case with setup, complete input data, steps, priority, exact oracle, and traceability.
- [ ] Condition-entry, action-outcome, feasible-rule, invalid/forbidden, reduced expansion, default, constraint, and precedence coverage are reported separately.
- [ ] Full legal Cartesian coverage is reported separately and only when intentionally attempted.
- [ ] Coverage arithmetic uses deduplicated stable IDs and correct denominators.
- [ ] Uncovered feasible rules, excluded/impossible rules, untested constraints, assumptions, Question/TBD items, and residual risks are visible.
- [ ] Generator and reduction metadata are recorded where tools or optimization are used.
- [ ] EP, BVA, Pairwise, state-transition, condition/cause-effect, error-guessing, risk-based, and exploratory follow-ups are identified where appropriate.
- [ ] No claim says Decision Table coverage proves all values, combinations outside the model, requirements, branches, states, higher-order behavior, or non-functional properties.
- [ ] Markdown headings and tables render correctly, with no placeholders, duplicate headings, broken tables, or non-English text.
