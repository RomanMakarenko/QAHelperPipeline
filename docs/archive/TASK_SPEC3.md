# Task Specification 3: Decision Table Test Design

## Goal

Prepare a clear, structured, English-language documentation guide and a reusable project-local skill for **Decision Table Testing**.

The guide must follow the quality standard established by the project's Boundary Value Analysis, Equivalence Partitioning, and Pairwise Testing documentation. A junior QA engineer should be able to replace the example requirements with a real case, identify conditions and actions, construct and validate a decision table, derive executable test cases, handle impossible and overlapping rules, and report meaningful coverage. The accompanying skill must make the same workflow reusable when a future user supplies a requirement, acceptance criterion, existing decision table, policy, API rule, or business rule.

Decision Tables are a black-box, specification-based test-design technique for making combinations of conditions and their expected actions explicit. They are especially useful for discovering missing, contradictory, overlapping, or ambiguous business rules. They do not prove that all input values, implementation branches, state paths, higher-order behavior, or non-functional properties are correct.

The existing Decision Table file is source material, not a complete English guide. It is an informal Russian article with missing diagram/table content, inconsistent orientation descriptions, and examples that require clarification. The implementation must correct and replace that material rather than claim that it was already a complete English guide.

## Target files

The task has two future implementation targets:

1. **Decision Table guide**
   `docs/TestDesignAndSoftwareTestingTechniques/BlackBox/decisionTable.md`
2. **Project-local Decision Table skill**
   `.claude/skills/decision-table/SKILL.md`

The current task specification itself is:

- `TASK_SPEC3.md`

Do not modify unrelated guides, skills, source documents, or configuration files while implementing the future deliverables.

---

# Requirements for both deliverables

## 1. Language, structure, and audience

Both future deliverables must:

- be written entirely in English;
- use a consistent Markdown heading hierarchy;
- use correctly rendered Markdown tables;
- be readable by a junior QA engineer while maintaining senior-QA precision;
- explain the technique before presenting examples;
- state assumptions explicitly when an example requirement is incomplete;
- use stable IDs and preserve traceability from requirements to conditions, actions, rules, and test cases;
- avoid placeholders, duplicate headings, broken tables, unsupported claims, and vague expected results;
- avoid retaining article metadata, promotional prose, blank image/table scaffolds, or informal filler from the Russian source.

The guide may mention the source article's useful concepts, but it must not retain Russian or Ukrainian text, unexplained blank sections, or arbitrary placeholders such as `Do X`, `Do Y`, or `Do Z` as if they were product behavior.

## 2. Shared guide quality requirements

The Decision Table guide must explain:

- what a Decision Table is;
- why it is used;
- what risks it addresses;
- how to construct, validate, reduce, and review one step by step;
- when it is appropriate;
- when it is insufficient;
- how to combine it with other test-design techniques;
- how to turn a rule into an executable test case without losing setup, action, oracle, or traceability information.

Examples must be internally consistent and include:

- an explicit requirement assumption;
- identified conditions and their domains or entries;
- identified actions and exact outcomes;
- formal constraints, dependencies, and context where applicable;
- selected rules or a justified reduced table;
- preconditions, complete input data, and actions;
- exact observable result oracles;
- stable condition, action, rule, constraint, and test-case IDs;
- traceability to requirements, conditions, entries, actions, and rules;
- independently checked coverage arithmetic.

The guide must include:

- reusable condition, action, constraint, rule, coverage, and test-case templates that can be copied into a test-management or documentation tool;
- a practical checklist for reviewing a Decision Table design;
- limitations and common mistakes;
- explicit labels for **Confirmed**, **Assumption**, **Question/TBD**, and **Residual risk**;
- a warning that passing all selected rules does not prove that all input values, unmodeled combinations, requirements, branches, state paths, or higher-order interactions are defect-free;
- precise oracles such as response codes, validation messages, acceptance/rejection, calculated values, persisted values, processing paths, action invocations, or state changes.

---

# Decision Table Testing requirements

## 1. Definition and ISTQB alignment

Explain Decision Table Testing as a black-box, specification-based test-design technique that represents combinations of conditions and the actions or outcomes selected by each combination.

Use ISTQB-consistent principles and terminology without inventing a version-specific syllabus section or citation. Explain that the guide is a practical project supplement, not a replacement for the current official ISTQB syllabus, product requirements, or domain-specific rules.

Define all of the following terms:

- **Condition/cause** — an input, fact, predicate, context value, or business condition that can influence the outcome;
- **Condition domain** — the complete declared set or partitioned range of values that a condition may take;
- **Condition entry/value** — the value assigned to a condition in one rule, such as `Y`, `N`, an enumerated value, a range class, or a date/time class;
- **Condition stub** — the labeled condition row or field in the decision table;
- **Action/effect** — an observable system response, business outcome, calculation, state change, or operation selected by a rule;
- **Action entry/value** — the expected value or execution marker for an action in one rule, such as `X`, `Do not apply`, an amount, a status, or a message;
- **Action stub** — the labeled action row or field in the decision table;
- **Rule** — one complete condition vector mapped to one consistent set of expected actions;
- **Rule ID** — a stable identifier used to trace one rule to its test case, requirement, and coverage result;
- **Rule column/row** — the rule orientation used by the table; conventional tables commonly use columns for rules, while a transposed representation may use rows if it is clearly labeled;
- **Condition entry notation** — the explicit notation used for a condition, including `Y`/`N`, limited-entry values, extended-entry values, ranges, or domain classes;
- **Action entry notation** — the explicit notation used for an expected action, output, status, amount, message, or non-execution;
- **Complete/exhaustive decision table** — a table that represents every declared combination in its modeled condition domain, subject to documented feasibility rules;
- **Limited-entry table** — a table using a small, usually Boolean, set of condition entries such as `Y` and `N`;
- **Extended-entry table** — a table using more than two condition entries, such as enumerated values or formal ranges;
- **Don't-care (`-`, `–`)** — a value that does not affect the action outcome for that rule and may be either value or any permitted value, subject to documented constraints;
- **Not applicable (`N/A`)** — a condition that is genuinely undefined or inapplicable in the stated context;
- **Unknown, omitted, untested, and blank** — distinct statuses that must not be silently treated as a don't-care or `N/A`;
- **Legal/feasible rule** — a complete rule that can occur under the stated domain, preconditions, and constraints;
- **Invalid/forbidden rule** — an intentionally modeled combination that should be rejected or handled as an error;
- **Impossible/infeasible rule** — a combination that cannot occur under the model or dependency rules and should be excluded with a rationale rather than counted as an uncovered legal rule;
- **Default/otherwise rule** — a deliberately defined fallback for all remaining combinations within the declared scope;
- **Overlap** — two or more rules that match the same concrete condition combination;
- **Precedence** — an explicit ordering or priority that resolves overlapping rules;
- **Contradiction/conflict** — rules matching the same combination but requiring incompatible actions or outcomes;
- **Completeness** — whether every required legal combination has a rule or is covered by a justified default rule;
- **Consistency** — whether matching rules produce compatible actions and oracles;
- **Reduction/minimization** — merging equivalent rules with don't-care entries without changing any covered behavior;
- **Decision-table coverage** — the selected proportion of required rules, condition entries, actions, or other declared units exercised by test cases;
- **Test oracle** — the exact observable result expected for a test case.

State explicitly:

- A rule contains one value for every modeled condition, or a justified `-`/`N/A` entry.
- Each rule must map to all materially relevant action outcomes; an empty action entry must have defined meaning.
- Conventional Decision Tables commonly place conditions and actions vertically and rules horizontally. A transposed table is acceptable only when the orientation is labeled and rule identity remains unambiguous.
- A rule is not automatically an executable test case. It becomes executable only after preconditions, setup data, complete inputs, steps, priority, exact oracle, and traceability are defined.
- A `-` don't-care is not an unknown, omitted value, untested value, or generic shortcut for a missing requirement.
- `N/A` is not a substitute for a value that was forgotten or not tested.
- A 100% decision-table metric is meaningful only relative to the declared condition domain, rule set, reduction, and constraints.

## 2. Distinctions from related techniques

Clearly distinguish Decision Table Testing from:

- **Equivalence Partitioning (EP):** EP identifies behaviorally equivalent valid and invalid classes and representatives. Decision Tables combine condition entries and map them to actions; EP may supply the entries used in the table.
- **Boundary Value Analysis (BVA):** BVA targets values at or around numeric, length, date/time, size, count, and threshold boundaries. Use BVA to choose condition values when endpoint behavior matters; a Decision Table does not replace boundary testing.
- **Pairwise/all-pairs testing:** Pairwise samples interactions among mostly independent parameters. Decision Tables explicitly model business-condition combinations and action outcomes, including combinations that may be rare, constrained, overlapping, or higher priority. Pairwise does not guarantee complete rule coverage.
- **Full Cartesian/exhaustive coverage:** Exhaustive coverage executes every legal complete combination in the declared domain. A reduced Decision Table may represent several combinations using a don't-care entry, but it must show why the outcomes are equivalent.
- **State-transition testing:** State-transition testing covers states, events, guards, and lifecycle paths. A Decision Table can model a rule conditioned on state, but it does not replace transition and path coverage.
- **Condition/cause-effect coverage:** Cause-effect and Boolean logic techniques analyze logical relationships among causes and effects. Decision Tables are a practical representation for explicit condition/action rules; use condition/cause-effect analysis when complex logical structure needs dedicated coverage.
- **Use-case or scenario testing:** Use cases emphasize actor goals and flows. Decision Tables emphasize combinations of conditions and resulting actions within a decision.
- **Risk-based, error-guessing, security, performance, accessibility, and exploratory testing:** These address risks that are not represented by ordinary condition/action rules.

Decision Tables are best for explicit business rules with finite or partitionable conditions and observable outcomes. They are not a universal replacement for other test-design techniques.

## 3. Modeling conditions, actions, and dependencies

Require the guide and skill to model conditions and actions independently before constructing rules.

For every condition, document:

- stable condition ID;
- semantic meaning and requirement reference;
- representation and data type;
- complete relevant domain, entries, or behaviorally distinct partitions;
- valid, invalid, missing, blank, null, malformed, unsupported, and unreadable representations when their behavior differs;
- range notation, endpoint ownership, unit, precision, rounding, and time zone when applicable;
- role, account state, lifecycle state, locale, environment, existing data, or other context;
- dependencies on other conditions;
- expected influence on the action outcome;
- status as Confirmed, Assumption, or Question/TBD.

For every action, document:

- stable action ID;
- semantic meaning and requirement reference;
- possible values or execution states;
- exact status, message, calculation, persistence, processing path, operation, or state-change oracle;
- dependencies and mutually exclusive actions;
- status as Confirmed, Assumption, or Question/TBD.

Classify conditions as:

- mostly independent;
- dependent or constrained;
- derived from another condition;
- state-, role-, or context-driven;
- invalid or negative-test dimensions.

Use EP to identify behaviorally meaningful condition classes and BVA to choose representatives around relevant thresholds. Do not choose arbitrary values merely to fill a table. A condition entry may be a partition representative, but one representative does not prove all values in that partition behave correctly.

Formalize all relevant dependencies and constraints, including:

- allowed and forbidden combinations;
- conditional availability of a condition entry;
- implication and precedence rules;
- mutual exclusion;
- role, account, state, locale, time-zone, reference-date, environment, or existing-data dependencies;
- invalid input and rejection rules;
- default or otherwise behavior;
- whether a candidate rule is feasible and has a complete legal interpretation.

## 4. Table semantics, completeness, and reduction

The guide and skill must require a clear decision-table contract:

- Every condition stub must have a defined domain or explicitly documented status.
- Every action stub must have a defined outcome or an explicit non-execution value.
- Every rule must have a stable ID and a complete condition/action interpretation.
- Conditions and actions must be traceable to the requirement or an explicitly labeled assumption.
- A legal rule must be feasible under all documented constraints.
- An impossible or forbidden combination must be listed with its rationale; it must not silently appear as an uncovered legal rule.
- Intentional invalid or forbidden combinations must be separated from valid positive coverage and must have an exact rejection/error oracle.
- Overlapping rules must be eliminated, shown to have equivalent actions, or resolved with explicit precedence.
- Duplicate rules must be merged or explained; rules with different outcomes must not be merged.
- Gaps in the declared legal condition space must be covered by an explicit rule, default/otherwise rule, or Question/TBD status.
- Contradictory action entries must be reported as a specification defect or resolved by a confirmed precedence rule.
- `-` may be used only when changing that condition cannot change any action or oracle for the rule and does not violate a dependency.
- A reduced rule with `-` represents multiple concrete combinations. The implementation must explain the expansion and verify that no represented combination has a different outcome.
- Reduction must preserve materially distinct outcomes, negative behavior, constraint behavior, and high-risk combinations.
- Rule order must not be treated as precedence unless the requirement explicitly defines it.

Distinguish:

- **Exhaustive table coverage:** all declared feasible complete combinations are represented;
- **Reduced table coverage:** a set of generalized rules represents multiple combinations using justified don't-care entries;
- **Rule execution coverage:** the selected rule IDs were executed;
- **Condition-entry coverage:** each relevant condition entry was exercised;
- **Action coverage:** each relevant action outcome was exercised.

## 5. Input and clarification policy

Accept a complete requirement, acceptance criterion, existing decision table, policy, API validation rule, multi-field form rule, authorization rule, pricing rule, order rule, or business rule.

Extract, when available:

- operation under test and requirement reference;
- every condition/cause and its semantic meaning;
- condition domains, entries, partitions, representations, formats, units, and endpoints;
- every action/effect and exact observable outcome;
- roles, account state, lifecycle state, locale, time zone, reference date, environment, and existing data;
- dependencies, allowed combinations, forbidden combinations, and conditional entries;
- overlap, precedence, default/otherwise, and invalid-input rules;
- desired scope: exhaustive, reduced, negative, or risk-focused table;
- risk, severity, priority, and historically defective combinations;
- generation, reduction, or modeling tool and reproducibility metadata when applicable.

Ask only questions that block safe modeling or make the expected oracle unknowable. Prioritize:

- missing or ambiguous conditions, entries, or actions;
- unclear endpoint ownership, range semantics, units, precision, or date/time context;
- unknown legal, forbidden, impossible, or conditional combinations;
- conflicting or overlapping rules without confirmed precedence;
- unclear default/otherwise behavior;
- unknown status, message, calculation, persistence, processing path, action, or state-change oracle;
- unclear role, state, existing-data, locale, or environment preconditions;
- uncertainty about whether a `-` is a real don't-care or an unmodeled value.

If clarification is not essential, proceed using explicit labels:

- **Confirmed** — stated directly in the requirement;
- **Assumption** — introduced to make a provisional model possible;
- **Question/TBD** — unresolved and requiring confirmation;
- **Residual risk** — a meaningful area not covered by the generated design.

Never present invented conditions, actions, constraints, precedence, defaults, or behavior as Confirmed. If a blocking ambiguity remains, provide the clarification and only provisional cases that do not depend on the unknown behavior.

## 6. Repeatable Decision Table workflow

Use this workflow in both the guide and the skill:

1. **Define scope and oracle.** Identify the operation, requirement references, acceptance criteria, preconditions, and exact observable outcomes.
2. **Extract conditions.** List every input, cause, predicate, context variable, role, state, representation, and environmental factor that can affect the decision.
3. **Extract actions.** List every materially distinct action, calculation, status, message, persistence result, processing path, or state change.
4. **Define condition domains.** Use finite values or behaviorally meaningful partitions; formalize ranges, endpoints, precision, formats, units, and context.
5. **Define action entries.** Specify exact values, execution markers, mutually exclusive outcomes, and observable oracles.
6. **Classify dependencies.** Mark independent, dependent, derived, state-driven, role-driven, and context-driven conditions and actions.
7. **Formalize constraints.** Record allowed, forbidden, conditional, implication, mutual-exclusion, precedence, default, and invalid-input rules.
8. **Enumerate the rule space.** Determine the Cartesian candidate combinations for the declared condition domains before constraints and distinguish feasible, invalid, forbidden, and impossible combinations.
9. **Construct the table.** Create stable condition, action, constraint, and rule IDs and map each feasible rule to its expected actions.
10. **Check completeness.** Find missing legal combinations, gaps, missing defaults, and uncovered condition entries. State which combinations are intentionally excluded.
11. **Check consistency and overlap.** Detect duplicate rules, conflicting actions, overlapping rules, hidden precedence, and contradictions. Resolve or report each one.
12. **Reduce safely.** Introduce `-` only when all concrete combinations represented by the generalized rule have identical actions, oracles, and applicable constraints. Record the reduction rationale and expansion.
13. **Separate negative cases.** Keep invalid or intentionally forbidden cases distinct from legal positive rules and assign violated constraint IDs and exact rejection/error oracles.
14. **Derive executable cases.** Add preconditions, complete input data, setup, steps, priority, exact oracle, and rule/condition/action traceability.
15. **Execute and compare.** Compare actual status, message, calculation, persistence, action, processing path, and state against the rule oracle.
16. **Verify coverage independently.** Check rule IDs, condition entries, action entries, feasible combinations, reductions, exclusions, and arithmetic without relying only on a table generator.
17. **Report gaps and risks.** List uncovered feasible rules, excluded/impossible rules, untested constraints, ambiguous rules, assumptions, Question/TBD items, and residual risks.
18. **Revise the model from evidence.** If supposedly equivalent entries or merged rules behave differently, split the rule or domain, document the defect, and update the design.

## 7. Generator, reduction, and reproducibility rules

A tool may enumerate, format, minimize, or suggest decision-table rules, but it cannot decide whether the requirement, conditions, actions, constraints, precedence, or oracles are correct. The tester remains responsible for:

- selecting complete and meaningful condition domains;
- validating all action outcomes and their exact oracles;
- checking legal, forbidden, impossible, and invalid combinations;
- reviewing overlap, conflict, default, precedence, and rule completeness;
- verifying every reduction and don't-care expansion independently;
- checking generated rules for duplicates, missing entries, illegal combinations, and accidental type coercion;
- recording tool/version, model, reduction settings, constraints, priorities, and other metadata needed to reproduce the result.

Do not use a reduced row count as proof of quality. A smaller table is not automatically safer, clearer, or easier to diagnose. Do not remove critical risk-based rules merely to minimize the table.

## 8. Required worked examples

### Example 1: Two binary conditions with four complete rules

Use a clear, neutral version of the insurance-style example:

- `EXPERIENCE`: `Y` means the driver has at least 5 completed years of driving experience; `N` means fewer than 5 years;
- `RECENT_CLAIM`: `Y` means at least one at-fault claim in the last 3 years; `N` means no at-fault claim in the last 3 years;
- all four combinations are legal under the example assumption;
- the action is an exact premium amount selected by the two conditions.

The example must include:

- an explicit assumption defining `at least 5 years`, the claim period, currency, and whether the premium is monthly or annual;
- stable condition IDs, action IDs, rule IDs such as `R1` through `R4`, and test-case IDs;
- a complete 2 × 2 rule table with all four legal combinations;
- preconditions, setup, input data, submission action, and exact oracle for each premium, response, and persistence result;
- traceability from each requirement statement to conditions, actions, rules, and tests;
- independently checked arithmetic: `2 × 2 = 4` feasible combinations and `4 / 4 = 100%` rule coverage when all four rules are executed;
- separate condition-entry and action coverage;
- a statement that this covers the declared model only and does not prove every driving history, boundary, fraud case, state, or non-functional property.

Do not describe a vague “works correctly” result or retain conflicting wording such as “was in accidents” versus “often has accidents” without defining the condition.

### Example 2: Multi-condition, multi-action pricing, order, or access rule

Use a realistic rule with at least three conditions and multiple actions. A suitable model may include:

- `SPEND_TIER`: `<100`, `100–499`, `500–999`, `>=1000` in a stated currency and period;
- `BUYBACK_TIER`: `<5%`, `5–29%`, `30–79%`, `>=80%` with explicit endpoint ownership;
- a customer or account condition such as `LOYALTY_MEMBER`: `Y`/`N`, or an order condition such as `EXPRESS_ELIGIBLE`: `Y`/`N`;
- actions such as exact discount percentage, free-item quantity, shipping method, authorization result, or notification.

The example must:

- define every interval, endpoint, unit, reference period, and context assumption;
- state whether all candidate combinations are legal or identify constraints;
- show all required rules or a formally justified reduced table;
- demonstrate multiple actions and exact outcomes for every rule;
- show when actions are independent and when they are not, rather than implying that one diagonal row represents all combinations;
- use don't-care entries only where the action and oracle are genuinely unchanged for every expansion;
- calculate the candidate Cartesian size, feasible rule denominator, selected rule coverage, condition-entry coverage, and action coverage separately;
- map every selected rule to executable test cases with complete inputs, actions, exact result oracles, and stable IDs;
- identify untested or excluded combinations and explain whether they are legal gaps, impossible rules, or residual risks.

If a reduced rule represents multiple concrete combinations, show at least one expansion or a mapping table and prove that no expanded combination has a different outcome.

### Example 3: Constraints, overlap, precedence, default, and negative behavior

Use a constrained coupon, checkout, authorization, or access rule that demonstrates the difference between feasible, impossible, forbidden, and invalid combinations. A suitable model may include:

- `COUPON_SUPPLIED`: `Y`/`N`;
- `COUPON_VALID`: `Y`/`N` when a coupon is supplied and `N/A` when no coupon is supplied;
- `MEMBER_TIER`: enumerated levels;
- `CART_MEETS_THRESHOLD`: `Y`/`N`;
- an account or security condition such as `ACCOUNT_SUSPENDED`: `Y`/`N`.

The example must show:

- explicit conditional availability, for example `COUPON_SUPPLIED = N` with `COUPON_VALID = Y` or `N` is impossible rather than merely uncovered;
- at least one forbidden or invalid input combination intentionally exercised as a separate negative case;
- an exact rejection/status/message oracle and violated constraint ID for the negative case;
- at least one overlapping rule or a rule conflict;
- a confirmed precedence rule, such as suspended account or invalid coupon denial taking precedence over discount eligibility;
- a default/otherwise rule when required and an explanation of why it does not overlap a specific rule;
- safe reduction using a don't-care entry where the actions are identical, with an expansion or proof of equivalence;
- stable condition, action, constraint, rule, and test-case IDs;
- executable preconditions, setup, actions, complete inputs, exact oracles, and traceability;
- coverage arithmetic separating feasible-rule, condition-entry, action, and selected negative coverage;
- a clear statement of remaining residual risks and when state-transition, condition/cause-effect, Pairwise, EP, BVA, or another technique is more appropriate.

Every worked example must be self-contained, state assumptions, provide executable actions and exact oracles, and preserve traceability from requirement to condition, entry, action, rule, test case, and result. Do not claim that Decision Table Testing alone catches all higher-order, state, security, or non-functional defects.

## 9. Coverage requirements

For each declared coverage unit, define the denominator and report exclusions explicitly.

At minimum, require these separate metrics:

1. **Condition-entry/value coverage** — whether every required condition entry or semantic alternative is exercised at least once.
2. **Action-entry/outcome coverage** — whether every required action value, execution state, or observable outcome is exercised at least once.
3. **Required feasible-rule coverage** —
   `covered feasible rule IDs / total required feasible rule IDs × 100%`.
4. **Selected invalid/forbidden-rule coverage** — intentionally exercised invalid or forbidden combinations, reported separately from legal positive coverage.
5. **Full legal Cartesian coverage** — reported only when exhaustive coverage of all declared feasible complete combinations was intentionally attempted.
6. **Reduced-rule expansion coverage** — the concrete combinations represented by generalized don't-care rules, when reduction is used.
7. **Constraint/default/precedence coverage** — selected rules or cases exercising each important constraint, default branch, and precedence relationship.
8. **Row/rule count and metadata** — table size, reduced-rule count, tool/version, model, constraints, priorities, reduction method, and other reproducibility data when applicable.

Do not count impossible or infeasible combinations as uncovered required legal rules. List them separately with their rationale.

Always list:

- uncovered feasible rules;
- excluded, impossible, or forbidden combinations;
- untested constraints and precedence relationships;
- reductions and their expansion rationale;
- assumptions and Question/TBD items;
- residual risks and complementary tests.

Do not imply that 100% rule coverage equals coverage of all input values, all combinations outside the declared model, all requirements, branches, states, higher-order interactions, security, performance, accessibility, usability, reliability, compatibility, or exploratory behavior.

## 10. Recommended output structure

When no project-specific format is supplied, use:

1. Scope, requirement basis, and exact oracle
2. Clarifications, assumptions, questions, and residual risks
3. Condition model
4. Action model
5. Constraints, dependencies, defaults, and precedence
6. Decision Table and rule inventory
7. Executable test cases
8. Condition-entry, action, rule, negative, and optional Cartesian coverage
9. Uncovered risks and complementary techniques
10. Verification checklist

### Condition model table

| Condition ID | Meaning | Representation/type | Domain and entries | Endpoint/context assumptions | Expected influence/oracle | Dependencies | Requirement reference | Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `COND-1` |  |  |  |  |  |  |  | Confirmed / Assumption / Question/TBD |

### Action model table

| Action ID | Meaning | Possible entries/outcomes | Exact observable oracle | Dependencies/mutual exclusions | Requirement reference | Status |
| --- | --- | --- | --- | --- | --- | --- |
| `ACTION-1` |  |  |  |  |  | Confirmed / Assumption / Question/TBD |

### Constraint and precedence table

| Constraint ID | Formal rule | Allowed/forbidden or conditional behavior | Affected conditions/actions | Default/precedence/conflict impact | Observable consequence | Requirement reference | Status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `CONSTRAINT-1` |  |  |  |  |  |  | Confirmed / Assumption / Question/TBD |

### Decision-rule matrix

Use conventional columns for rules or clearly label a transposed form. Every condition and action entry must have defined notation.

| Rule ID | Condition entries | Action entries | Legal/invalid/impossible/default status | Precedence/overlap notes | Covered test cases | Requirement reference | Status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `RULE-1` |  |  |  |  |  |  | Confirmed / Assumption / Question/TBD |

A more execution-friendly matrix may expand conditions and actions into separate columns:

| Rule ID | `COND-1` | `COND-2` | `COND-3` | `ACTION-1` | `ACTION-2` | Feasibility/status | Constraint IDs | Covered cases |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `RULE-1` |  |  |  |  |  |  |  |  |

For an intentional invalid case, populate the violated constraint IDs and exact rejection/error outcome. For an impossible case, record the exclusion rationale rather than inventing an executable positive case.

### Executable Decision Table test-case table

| Test case ID | Rule ID | Title/objective | Requirement reference | Priority | Preconditions/setup | Complete input data | Steps/actions | Exact expected result/oracle | Covered condition IDs/entries | Covered action IDs/outcomes | Violated constraint IDs | Technique tags | Assumptions/notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `DT-001` | `RULE-1` |  |  | High/Medium/Low |  |  | 1.  2.  3.  |  |  |  |  | Decision Table / EP / BVA / negative |  |

Every positive case must use a feasible complete rule or a documented expansion of a reduced rule. Every intentional invalid case must identify its violated constraint and exact rejection/status oracle.

### Coverage and gap table

| Coverage ID | Metric | Required denominator | Exercised numerator | Percentage | Exclusions/uncovered items | Evidence/notes |
| --- | --- | --- | --- | --- | --- | --- |
| `COV-1` | Feasible-rule coverage |  |  |  |  |  |

## 11. When to use Decision Table Testing

Decision Tables are especially useful when:

- a requirement contains multiple conditions whose combinations select different actions;
- business rules include discounts, eligibility, authorization, pricing, routing, limits, or notifications;
- omissions, contradictions, overlap, and precedence are plausible risks;
- the condition domains are finite or can be partitioned into behaviorally distinct classes;
- the team needs a reviewable model connecting requirements to executable tests;
- a prose rule is difficult to interpret or maintain;
- a default, exception, or denial rule must be made explicit.

Use Decision Tables after the primary behavior and input meanings are understood. Use EP and BVA to establish meaningful condition entries before building the table. Split a table when it becomes too dense to review or when separate decisions have different scopes and oracles.

Do not force a Decision Table onto an unordered input with no condition/action rule, a purely numeric boundary problem, a lifecycle graph, or a large set of mostly independent configuration dimensions that is better handled by Pairwise or another technique.

## 12. Limitations and common mistakes

Decision Tables alone are insufficient for:

- values or representations not modeled as condition entries;
- malformed, null, missing, blank, unsupported, unreadable, or overflow input unless explicitly modeled;
- numeric, length, date/time, file-size, count, or quota boundaries unless entries are deliberately selected with BVA;
- lifecycle paths and state transitions;
- complex Boolean logic requiring dedicated condition/cause-effect analysis;
- broad interactions among many mostly independent parameters where Pairwise or stronger combinatorial testing is more practical;
- implementation branches not reflected in the requirement;
- security, performance, reliability, usability, accessibility, compatibility, and exploratory risks.

Avoid these mistakes:

1. **Treating a rule as an automatically executable test.** Add setup, complete inputs, steps, priority, exact oracle, and traceability.
2. **Leaving the table orientation ambiguous.** State whether rules are columns or rows and preserve stable rule IDs.
3. **Omitting condition entries or action outcomes.** Define the declared domain and every materially different behavior.
4. **Calling unknown or untested values don't-care.** Use `-` only when the action is invariant for all permitted expansions.
5. **Using `N/A` for missing information.** `N/A` means the condition is genuinely not applicable in that context.
6. **Counting impossible combinations as uncovered legal rules.** Exclude them explicitly with a feasibility rationale.
7. **Mixing invalid negative cases into legal positive rule coverage.** Give them separate constraint IDs and exact rejection/error oracles.
8. **Allowing overlapping rules without precedence.** Detect intersections and define a confirmed priority or fix the specification.
9. **Merging rules with different outcomes.** Reduction is safe only when all expanded combinations have equivalent actions and oracles.
10. **Leaving default behavior implicit.** Add a default/otherwise rule or record a requirement gap.
11. **Using arbitrary condition values.** Derive entries from the requirement, EP partitions, BVA representatives, or explicit assumptions.
12. **Assuming a fixed number of rules or tests.** The size depends on condition domains, constraints, action distinctions, reduction, and scope.
13. **Using vague oracles.** Replace “the system works” with exact status, message, calculation, persistence, action, processing path, or state outcome.
14. **Confusing rule coverage with full Cartesian, requirement, branch, or state coverage.** Report each metric separately.
15. **Trusting a generator or visual table without independent review.** Tools cannot validate the requirement, constraints, precedence, reduction, or oracle.
16. **Keeping one huge table for unrelated decisions.** Split by decision scope and link the tables when necessary.
17. **Ignoring maintenance cost.** Update rules and tests when the requirement, domain, action, or precedence changes.

## 13. Complementary techniques

Explain how Decision Table Testing works with:

- **Equivalence Partitioning** — derives valid, invalid, and behaviorally distinct condition entries;
- **Boundary Value Analysis** — selects lower, upper, threshold, and transition representatives before placing them in conditions;
- **Pairwise testing** — covers interactions among many mostly independent parameters when exhaustive decision rules are too large;
- **State-transition testing** — covers lifecycle states, events, guards, and important paths;
- **Condition/cause-effect coverage** — analyzes complex Boolean relationships and cause/effect logic;
- **Use-case/scenario testing** — covers actor goals and end-to-end flows;
- **Error guessing** — adds malformed, unusual, coercion, locale, historical, and likely-user-error cases;
- **Risk-based testing** — prioritizes safety, financial, authorization, security, compatibility, and high-impact rules;
- **Exploratory testing** — investigates behavior outside the formal model.

A practical sequence is to use EP and BVA to define condition entries, use a Decision Table for explicit condition/action rules, use constraints and precedence to validate feasibility and consistency, use Pairwise when many mostly independent dimensions are present, use state-transition testing for lifecycle behavior, and add risk-based or exploratory cases for residual risks.

## 14. Verification checklist

Before approving a Decision Table design, confirm:

- [ ] Scope, operation, requirement basis, preconditions, and exact observable oracle are documented.
- [ ] Every relevant condition/cause has a stable ID, semantic meaning, representation, and declared domain.
- [ ] Every relevant action/effect has a stable ID, possible outcome, and exact oracle.
- [ ] Endpoint ownership, units, precision, formats, locale, time zone, state, role, and context assumptions are explicit where relevant.
- [ ] Valid, invalid, missing, blank, null, malformed, unsupported, and unreadable entries are considered when their behavior differs.
- [ ] Condition entries are meaningful and traceable to the requirement, EP, BVA, or an explicit assumption.
- [ ] Action entries are complete, mutually compatible, and traceable to the requirement.
- [ ] Table orientation is explicit and rule IDs remain stable if the table is transposed.
- [ ] `Y`, `N`, enumerated entries, ranges, `-`, `N/A`, unknown, omitted, and untested have distinct meanings.
- [ ] Dependencies, allowed combinations, forbidden combinations, conditional entries, implications, and mutual exclusions are formalized.
- [ ] Every required legal rule is feasible and has a complete condition/action interpretation.
- [ ] Impossible or infeasible combinations are listed with exclusion rationale rather than counted as uncovered legal rules.
- [ ] Intentional invalid rules have violated constraint IDs and exact rejection/status oracles.
- [ ] The table is complete for the declared legal domain or has an explicit default/otherwise rule and visible gaps.
- [ ] Duplicate, overlapping, and conflicting rules are detected and resolved or documented with confirmed precedence.
- [ ] Any rule reduction uses `-` only when all expanded combinations have identical actions and oracles.
- [ ] Reduction rationale and expanded coverage are traceable.
- [ ] Each selected rule maps to an executable test case with preconditions, complete input data, steps/actions, priority, exact oracle, and traceability.
- [ ] Condition-entry, action-entry, feasible-rule, invalid/forbidden, default, and precedence coverage are reported separately.
- [ ] Full legal Cartesian coverage is reported separately and only when intentionally attempted.
- [ ] Coverage arithmetic uses deduplicated stable IDs and correct denominators.
- [ ] Uncovered feasible rules, excluded/impossible rules, untested constraints, assumptions, Question/TBD items, and residual risks are visible.
- [ ] Generation and reduction metadata are recorded where tools or optimization are used.
- [ ] EP, BVA, Pairwise, state-transition, condition/cause-effect, error-guessing, risk-based, and exploratory follow-ups are identified where appropriate.
- [ ] No claim says Decision Table coverage proves all values, combinations, requirements, branches, states, higher-order behavior, or non-functional properties.
- [ ] Markdown headings and tables render correctly, with no placeholders, duplicate headings, broken tables, or non-English text.

---

# Project-local Decision Table skill requirements

Create `.claude/skills/decision-table/SKILL.md` following the existing project skill convention.

## 1. Frontmatter and guide reference

Use YAML frontmatter with:

```yaml
---
name: decision-table
description: Apply ISTQB-aligned Decision Table Testing to a supplied requirement, acceptance criterion, existing decision table, policy, API rule, multi-field form, authorization rule, pricing rule, or business rule. Trigger when the user asks to model conditions and actions, enumerate or review business-rule combinations, analyze defaults or precedence, validate rule completeness, or prepare decision-table test cases.
version: 0.1.0
---
```

Reference the detailed project guide exactly as:

`docs/TestDesignAndSoftwareTestingTechniques/BlackBox/decisionTable.md`

The skill must state that the guide is the primary terminology, workflow, example, and template reference. If the guide is unavailable, the skill must provide a usable fallback based on the rules in the skill and ISTQB-consistent black-box specification-based terminology.

## 2. Skill behavior

The skill must:

- accept requirements, acceptance criteria, existing decision tables, policies, API validation rules, configuration rules, multi-field forms, and business rules;
- never invent conditions, actions, constraints, precedence, defaults, or expected behavior;
- ask only questions that block safe modeling or make the oracle unknowable;
- label information as Confirmed, Assumption, Question/TBD, or Residual risk;
- model conditions and actions independently with stable IDs;
- define domains, entries, representations, endpoints, context, dependencies, and exact oracles;
- distinguish `-`, `N/A`, unknown, omitted, blank, and untested;
- formalize legal, forbidden, conditional, impossible, invalid, precedence, overlap, default, and reduction rules;
- keep intentional invalid cases separate from legal positive coverage;
- require violated constraint IDs and exact rejection/error oracles for intentional invalid cases;
- validate completeness, consistency, overlap, contradiction, precedence, and feasibility;
- reduce only when a don't-care expansion preserves identical actions and oracles;
- derive executable cases with preconditions, complete inputs, steps, exact outcomes, priorities, and traceability;
- independently verify coverage arithmetic and list uncovered/excluded rules;
- preserve a user-requested output format such as Gherkin, JSON, CSV, or a test-management schema when one is supplied.

## 3. Default skill output

When no format is specified, the skill must produce Markdown with these sections:

```markdown
## Scope, requirement basis, and exact oracle

## Clarifications, assumptions, questions, and residual risks

## Condition model

## Action model

## Constraints, dependencies, defaults, and precedence

## Decision Table and rule inventory

## Executable test cases

## Coverage summary

## Uncovered risks and complementary techniques

## Verification checklist
```

The skill must include reusable templates for condition modeling, action modeling, constraints/precedence, decision rules, executable test cases, and coverage/gaps. The test-case template must include rule ID, requirement reference, priority, preconditions/setup, complete input data, steps/actions, exact oracle, covered condition/action IDs, violated constraint IDs, technique tags, and assumptions/notes.

## 4. Skill coverage and review rules

The skill must require separate reporting for:

- condition-entry/value coverage;
- action-entry/outcome coverage;
- required feasible-rule coverage;
- selected invalid/forbidden coverage;
- reduced-rule expansion coverage;
- optional full legal Cartesian coverage;
- default/precedence/constraint coverage;
- row/rule counts and generation/reduction metadata.

It must require explicit lists of uncovered feasible rules, excluded/impossible combinations, untested constraints, overlap/conflict findings, reduction assumptions, Question/TBD items, and residual risks.

It must warn that 100% Decision Table rule coverage does not prove every value, every unmodeled combination, every requirement, branch, state path, higher-order interaction, or non-functional property.

## 5. Skill manual checklist

Before presenting a result, the skill must verify:

- scope, requirement, operation, preconditions, and exact oracle;
- stable condition and action IDs with complete domains and outcomes;
- endpoint, representation, role, state, locale, time-zone, and dependency assumptions;
- explicit table orientation and stable rule IDs;
- distinct semantics for condition entries, action entries, `-`, `N/A`, unknown, omitted, and untested;
- formal constraints and feasible/impossible/invalid classification;
- complete and consistent rules, including default and precedence behavior;
- safe reduction rationale and expansion coverage;
- executable positive and negative test cases with complete data and exact oracles;
- separate condition, action, rule, negative, Cartesian, default, and precedence metrics;
- uncovered and excluded items, assumptions, Question/TBD items, and residual risks;
- appropriate EP, BVA, Pairwise, state-transition, condition/cause-effect, error-guessing, risk-based, and exploratory follow-ups;
- valid Markdown tables, no placeholders, no duplicate headings, and English-only output.

---

# Scope and acceptance criteria

The future implementation is complete only when all of the following are true:

1. `docs/TestDesignAndSoftwareTestingTechniques/BlackBox/decisionTable.md` is a standalone English Decision Table guide rather than a Russian article or a collection of blank diagram placeholders.
2. `.claude/skills/decision-table/SKILL.md` exists, uses the required frontmatter, references the exact project-relative guide path, and provides reusable Decision Table instructions.
3. Both deliverables are junior-readable while preserving precise specification-based test-design terminology.
4. Conditions, condition entries, actions, action entries, rules, rule IDs, conventional/transposed orientation, `-`, `N/A`, unknown, impossible, invalid, default, overlap, precedence, completeness, consistency, and reduction are explicitly defined.
5. The guide and skill formalize domains, endpoints, representations, roles, states, context, dependencies, constraints, legal rules, invalid rules, and impossible rules.
6. The workflow covers extraction, modeling, enumeration, feasibility, completeness, consistency, overlap/conflict review, precedence, safe reduction, executable cases, independent verification, coverage, and risk reporting.
7. The guide contains at least three self-contained worked examples:
   - two binary conditions with four verified complete rules;
   - a multi-condition, multi-action rule with checked arithmetic and exact oracles;
   - a constrained/overlapping/default/precedence/reduction example with impossible and intentional negative cases.
8. Every worked example states assumptions, preconditions, complete inputs, actions, exact oracles, stable IDs, traceability, and checked coverage arithmetic.
9. Coverage separates condition entries, actions, feasible rules, negative/forbidden rules, reduced-rule expansion, optional exhaustive Cartesian coverage, and metadata.
10. Reusable templates contain the required condition, action, constraint, precedence, rule, test-case, coverage, and risk fields.
11. Limitations, common mistakes, complementary techniques, and a practical verification checklist are included.
12. No unsupported claim says that Decision Table coverage proves all values, complete combinations, requirements, branches, states, higher-order interactions, or non-functional behavior.
13. Both deliverables contain no non-English text, placeholders, duplicate headings, malformed Markdown tables, vague expected results, or arbitrary action labels presented as real behavior.
14. The implementation does not modify unrelated files.

## Required final verification

Before considering the future implementation complete:

- read both deliverables end-to-end;
- verify English-only content and consistent heading hierarchy;
- parse every Markdown table and confirm matching header, separator, and body column counts;
- check frontmatter fields and exact skill guide reference;
- independently recalculate all worked-example rule, condition-entry, action, feasible, negative, and reduction coverage arithmetic;
- verify each example's rule-to-test traceability and exact oracles;
- verify impossible/forbidden combinations are excluded with rationale and invalid cases have constraint IDs;
- inspect all overlap, precedence, default, and don't-care reduction claims;
- check that no placeholders, duplicate headings, or vague “works correctly” oracles remain;
- run `git diff --check`;
- inspect `git diff --name-only` and confirm only the intended future deliverables changed during their implementation;
- report any pre-existing unrelated working-tree changes separately;
- do not commit or push unless explicitly requested.
