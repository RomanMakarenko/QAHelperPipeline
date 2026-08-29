---
name: decision-table
description: Apply ISTQB-aligned Decision Table Testing to a supplied requirement, acceptance criterion, existing decision table, policy, API rule, multi-field form, authorization rule, pricing rule, or business rule. Trigger when the user asks to model conditions and actions, enumerate or review business-rule combinations, analyze defaults or precedence, validate rule completeness, or prepare decision-table test cases.
version: 0.1.0
---

# Decision Table Testing Design

Apply this skill when the user provides a requirement, acceptance criterion, existing decision table, policy, API validation rule, configuration rule, multi-field form, authorization rule, pricing rule, order rule, or other business rule and asks to model conditions and actions or prepare decision-table test cases.

Use this project's detailed guide as the primary terminology, workflow, example, and template reference:

`docs/TestDesignAndSoftwareTestingTechniques/BlackBox/decisionTable.md`

If the guide is unavailable, use the rules in this skill and ISTQB-consistent black-box, specification-based terminology as a usable fallback. Do not invent product behavior, conditions, actions, constraints, precedence, defaults, or oracles that are not stated in the supplied requirement.

## Scope and core principle

Treat Decision Table Testing as a black-box, specification-based technique that represents combinations of conditions and the actions or outcomes selected by each combination. Conditions may be inputs, predicates, facts, roles, states, context values, or environmental variables. Actions may be responses, calculations, messages, persistence results, processing paths, invocations, notifications, or state changes.

A rule is one complete condition vector mapped to all materially relevant actions. Conventional tables place condition and action stubs vertically and rules horizontally as columns. A transposed table may use rule rows only when clearly labeled and stable Rule IDs are preserved. A rule is not an executable case until preconditions, setup, complete input data, steps, priority, exact oracle, and traceability are supplied.

A `-` means that every permitted value of that condition produces identical actions and oracles for that rule; it is not an unknown, omitted, untested, blank, or missing value. `N/A` means genuinely not applicable in the stated context. Keep impossible, forbidden, invalid, and legal combinations distinct. Rule coverage does not prove every input value, unmodeled combination, requirement, branch, state path, higher-order interaction, or non-functional property.

## Input contract

Accept:

- a complete requirement or acceptance criterion;
- an existing decision table that needs review, completion, reduction, or executable cases;
- an API validation or authorization rule;
- a policy, pricing, order, eligibility, routing, notification, or configuration rule;
- a multi-field form or business process with dependent conditions;
- a request for exhaustive, reduced, negative, risk-focused, or format-specific output.

Extract when available:

- operation under test and requirement references;
- every condition/cause and semantic meaning;
- domains, entries, partitions, representations, formats, units, endpoints, precision, and time context;
- every action/effect and exact observable outcome;
- roles, account/lifecycle state, locale, time zone, reference date, environment, and existing data;
- dependencies, allowed, forbidden, conditional, implication, mutual-exclusion, invalid, default, overlap, and precedence rules;
- desired exhaustive/reduced/negative scope and risk priorities;
- generator, version, model, constraints, priorities, reduction settings, seed, and reproducibility metadata.

Do not choose arbitrary values to fill a table. Use Equivalence Partitioning to identify behaviorally meaningful entries and Boundary Value Analysis for threshold representatives when applicable.

## Clarifications and status labels

Ask only questions that block safe modeling or make the expected oracle unknowable. Prioritize:

- missing or ambiguous conditions, entries, actions, or outcomes;
- unclear endpoint ownership, range semantics, units, precision, rounding, or date/time context;
- unknown legal, forbidden, impossible, conditional, or mutually exclusive combinations;
- overlap or conflict without confirmed precedence;
- unclear default/otherwise behavior;
- unknown response, status, message, calculation, persistence, processing path, invocation, or state-change oracle;
- unclear role, state, existing-data, locale, environment, or setup preconditions;
- uncertainty whether a `-` is a genuine don't-care.

Label information as:

- **Confirmed** — stated directly in the requirement or contract;
- **Assumption** — introduced to make a provisional model or example possible;
- **Question/TBD** — unresolved and requiring confirmation;
- **Residual risk** — meaningful behavior outside the current design.

Never present an invented behavior as Confirmed. If an ambiguity is blocking, ask the question and provide only provisional cases that do not depend on the unknown answer.

## Condition and action modeling

Model conditions and actions independently before constructing rules.

For every condition, record a stable ID, meaning, requirement reference, representation/type, complete relevant domain, behaviorally distinct entries, valid/invalid/missing/blank/null/malformed/unsupported/unreadable forms where behavior differs, ranges and endpoint ownership, units and precision, rounding, time zone/reference date, role/state/context, dependencies, expected influence, and status label.

For every action, record a stable ID, meaning, requirement reference, possible entries and execution states, exact status/message/calculation/persistence/path/invocation/state-change oracle, dependencies, mutual exclusions, and status label. Empty action entries must have a defined meaning such as “not executed.”

Classify conditions as mostly independent, dependent/constrained, derived, state/role/context-driven, or invalid/negative-test dimensions. Use `N/A` only for genuinely conditional inapplicability. Do not claim that one EP representative or BVA value proves all values in its class.

## Constraints, feasibility, overlap, precedence, defaults, and reduction

Formalize before generating positive rules:

- allowed and forbidden combinations;
- conditional entry availability and implications;
- mutual exclusion and derived-value consistency;
- role, account, lifecycle, locale, time-zone, reference-date, environment, and existing-data dependencies;
- invalid-input and rejection rules;
- default/otherwise behavior;
- overlap, conflict, and explicit precedence.

Enumerate candidate Cartesian combinations and classify each as legal/feasible, invalid, forbidden, or impossible/infeasible. Do not count impossible combinations as uncovered legal rules. An externally constructible invalid payload may be an intentional negative case only when it has a violated constraint ID and exact rejection/status/message oracle; it does not increase positive coverage.

Check completeness, consistency, duplicates, overlap, contradiction, default gaps, hidden precedence, and action compatibility. Use `-` only after checking every concrete expansion has identical actions, exact oracles, and applicable constraints. Record the reduction rationale, expansion count, and mapping. Do not reduce away materially distinct or high-risk behavior merely to lower row count.

## Repeatable Decision Table procedure

1. **Define scope and oracle.** Identify operation, requirement references, preconditions, and exact observable outcomes.
2. **Extract conditions.** List inputs, causes, predicates, roles, states, representations, context, and environment factors.
3. **Extract actions.** List distinct responses, calculations, statuses, messages, persistence, paths, operations, notifications, and state changes.
4. **Define condition domains.** Use finite or behaviorally partitioned entries; formalize endpoints, precision, units, formats, and context.
5. **Define action entries.** Specify exact values, execution markers, mutual exclusions, and observable oracles.
6. **Classify dependencies.** Mark independent, dependent, derived, state-driven, role-driven, and context-driven dimensions.
7. **Formalize constraints.** Record legal, forbidden, conditional, implication, mutual-exclusion, precedence, default, and invalid-input rules.
8. **Enumerate candidate space.** Calculate the Cartesian candidate count and classify feasible, invalid, forbidden, and impossible combinations.
9. **Construct the table.** Create stable condition, action, constraint, and Rule IDs and map each feasible rule to outcomes.
10. **Check completeness.** Find missing legal rules, gaps, missing defaults, and uncovered condition entries; state intentional exclusions.
11. **Check consistency and overlap.** Detect duplicates, intersections, conflicting actions, hidden precedence, and contradictions.
12. **Reduce safely.** Apply `-` only when all expansions preserve actions, oracles, and applicable constraints; record proof and expansion.
13. **Separate negative cases.** Keep invalid and forbidden cases out of legal positive coverage and attach violated constraints and exact negative oracles.
14. **Derive executable cases.** Add setup, complete inputs, steps, priority, exact oracle, and condition/action/rule traceability.
15. **Execute and compare.** Compare actual status, message, calculation, persistence, path, invocation, and state with the rule oracle.
16. **Verify coverage independently.** Recalculate deduplicated rule, entry, action, feasible, reduction, exclusion, constraint, default, and precedence coverage without relying only on a generator.
17. **Report gaps and risks.** List uncovered feasible rules, excluded/impossible rules, untested constraints, assumptions, Question/TBD items, and residual risks.
18. **Revise the model from evidence.** Split domains or rules when supposedly equivalent entries or reductions produce different behavior; document defects.

## Default output

Preserve a requested format such as Gherkin, JSON, CSV, or a test-management schema. When no format is specified, produce Markdown with these sections:

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

## Reusable templates

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

State the orientation before presenting the matrix. A row-oriented version is:

| Rule ID | `COND-1` | `COND-2` | `COND-3` | `ACTION-1` | `ACTION-2` | Feasibility/status | Constraint IDs | Covered cases | Requirement reference | Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `RULE-1` |  |  |  |  |  | Legal / invalid / impossible / default |  |  |  | Confirmed / Assumption / Question/TBD |

### Executable Decision Table test case

| Test case ID | Rule ID | Title/objective | Requirement reference | Priority | Preconditions/setup | Complete input data | Steps/actions | Exact expected result/oracle | Covered condition IDs/entries | Covered action IDs/outcomes | Violated constraint IDs | Technique tags | Assumptions/notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `DT-001` | `RULE-1` |  |  | High/Medium/Low |  |  | 1.  2.  3.  |  |  |  |  | Decision Table / EP / BVA / negative |  |

### Coverage and gaps

| Coverage ID | Metric | Required denominator | Exercised numerator | Percentage | Exclusions/uncovered items | Evidence/notes |
| --- | --- | --- | --- | --- | --- | --- |
| `COV-1` | Feasible-rule coverage |  |  |  |  |  |

### Risk and residual-risk inventory

| Risk ID | Uncovered or weakly modeled area | Reason not covered | Impact/priority | Complementary technique or follow-up | Status |
| --- | --- | --- | --- | --- | --- |
| `RISK-1` |  |  |  |  | Residual risk / Question/TBD |

## Coverage and review rules

Report separately, with explicit denominators and deduplicated stable IDs:

- **Condition-entry/value coverage:** exercised required entries / total required entries × 100%.
- **Action-entry/outcome coverage:** exercised required outcomes / total modeled outcomes × 100%.
- **Required feasible-rule coverage:** covered feasible Rule IDs / total required feasible Rule IDs × 100%.
- **Selected invalid/forbidden coverage:** exercised selected negative combinations / selected invalid or forbidden combinations × 100%, separate from legal positive coverage.
- **Full legal Cartesian coverage:** executed legal complete combinations / all declared feasible combinations × 100%, only when exhaustive testing was intentionally attempted.
- **Reduced-rule expansion coverage:** represented or executed concrete expansions / required concrete expansions × 100%; distinguish representation from execution.
- **Constraint/default/precedence coverage:** exercised important relationships / declared relationships × 100%.
- **Row/rule counts and metadata:** candidates, feasible rules, reduced rules, executed rows, exclusions, tool/version, model, seed, constraints, priorities, and reduction method.

Always list uncovered feasible rules, excluded/impossible/forbidden combinations with rationale, untested constraints/defaults/precedence, reduction assumptions and expansion proof, Question/TBD items, assumptions, and residual risks. Warn that 100% Decision Table rule coverage is relative to the declared domain and does not prove all values, unmodeled combinations, requirements, branches, states, higher-order interactions, or non-functional properties.

## Limitations and complementary techniques

Decision tables do not replace EP for behavioral classes, BVA for thresholds, Pairwise for many mostly independent dimensions, state-transition testing for lifecycle paths, condition/cause-effect coverage for complex Boolean logic, use-case/scenario testing for actor goals, error guessing for malformed or unusual inputs, risk-based testing for high-impact behavior, or security, performance, accessibility, reliability, usability, compatibility, and exploratory testing. Recommend appropriate follow-ups for risks outside the declared decision.

## Manual verification checklist

Before presenting a result, verify:

- [ ] Scope, operation, requirement basis, preconditions, and exact oracle are documented.
- [ ] Conditions and actions have stable IDs, complete domains/outcomes, representations, dependencies, and statuses.
- [ ] Endpoint, unit, precision, role, state, locale, time-zone, environment, and context assumptions are explicit where relevant.
- [ ] The table orientation and stable Rule IDs are explicit.
- [ ] `Y`, `N`, enumerated values, ranges, `-`, `N/A`, unknown, omitted, blank, and untested have distinct meanings.
- [ ] Legal, forbidden, conditional, invalid, and impossible combinations are classified with formal constraints.
- [ ] Impossible combinations have exclusion rationales and are not counted as uncovered legal rules.
- [ ] Invalid cases are separate, identify violated constraint IDs, and have exact rejection/status oracles.
- [ ] Rules are complete and consistent; duplicates, overlap, conflicts, defaults, and precedence are resolved or documented.
- [ ] Every `-` reduction has an expansion/equivalence rationale covering identical actions, oracles, and constraints.
- [ ] Positive and negative cases have setup, complete inputs, actions, priority, exact outcomes, and traceability.
- [ ] Condition, action, feasible-rule, invalid/forbidden, reduced expansion, Cartesian, constraint, default, and precedence metrics are separate.
- [ ] Coverage arithmetic uses deduplicated IDs and lists uncovered/excluded items.
- [ ] Generation and reduction metadata are recorded where applicable.
- [ ] Assumptions, Question/TBD items, and residual risks are visible.
- [ ] EP, BVA, Pairwise, state-transition, condition/cause-effect, error-guessing, risk-based, and exploratory follow-ups are identified where appropriate.
- [ ] Markdown tables render correctly, with no placeholders, duplicate headings, vague oracles, or non-English text.

## Compact reference

For detailed terminology, workflow, examples, templates, coverage guidance, and the complete verification checklist, use:

`docs/TestDesignAndSoftwareTestingTechniques/BlackBox/decisionTable.md`

Do not copy assumptions from the guide's teaching examples into a real requirement without confirmation.
