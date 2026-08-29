---
name: pairwise-testing
description: Apply ISTQB-aligned Pairwise Testing to a supplied requirement, acceptance criterion, configuration matrix, API parameter set, multi-field form, compatibility rule, or business rule. Trigger when the user asks to model factors and levels, generate or review all-pairs coverage, analyze legal combinations, or prepare interaction-focused test cases.
version: 0.1.0
---

# Pairwise Testing Design

Apply this skill when the user provides a requirement, acceptance criterion, existing test matrix, API parameter set, multi-field form, compatibility rule, configuration matrix, file-upload combination, feature-flag set, or business rule and asks to use **Pairwise Testing** (all-pairs or 2-way combinatorial testing) to prepare or review test cases.

Use this project's detailed technique guide as the primary terminology, workflow, and example reference:

`docs/TestDesignAndSoftwareTestingTechniques/BlackBox/pairwiseTesting.md`

If the guide is unavailable, follow the rules in this skill and ISTQB-consistent black-box, specification-based test-design terminology. The guide is the authoritative practical supplement for this project; do not invent product behavior, levels, constraints, or oracles that are not stated in the supplied requirement.

## Scope and core principle

Treat Pairwise Testing as a black-box, specification-based, combinatorial test-design technique focused on interactions among multiple parameters. A parameter/factor is an input, configuration dimension, condition, environmental variable, or context variable. A level/value is one selected option or representative value.

For every two **distinct** parameters, 2-way testing requires every required legal pair of levels to appear in at least one legal complete configuration/test row. A covering array is a set of rows designed to provide this interaction coverage; it need not be balanced. Pairwise reduces redundant configurations, but it does not prove that every value, complete combination, requirement, branch, state, higher-order interaction, or non-functional property is defect-free.

Pairwise is not Equivalence Partitioning (EP) Each Choice Coverage, Boundary Value Analysis (BVA), decision-table coverage, state-transition coverage, full Cartesian coverage, branch coverage, security testing, performance testing, accessibility testing, or exploratory testing. Use EP to identify behaviorally meaningful classes and BVA to select boundary representatives before combining factors.

## Input contract

Accept any of the following:

- A complete requirement or acceptance criterion
- An existing test case or compatibility matrix that needs Pairwise review
- An API parameter set, multi-field form, or file-upload combination
- Browser, operating-system, device, database, locale, feature-flag, or environment configuration
- A business rule with roles, states, dates, existing data, or dependencies

Extract, when available:

- Operation under test and requirement reference
- Every factor/parameter and its semantic meaning
- Levels/values, representations, data types, formats, and semantic classes
- Baseline/default values and high-risk values
- EP partitions and BVA boundary representatives where applicable
- Exact observable oracles: status code, validation message, acceptance/rejection, calculation, persistence, processing path, or state change
- Independent, dependent, derived, state-driven, role-driven, and context-driven relationships
- Allowed, forbidden, conditional, implication, precedence, and dependency rules
- Roles, account state, lifecycle state, locale, time zone, reference date, environment, and existing data
- Desired interaction strength (`2-way`, `3-way`, or higher)
- Risk, severity, priority, and historically defective combinations
- Generator/tool, version, model, seed, weights, and optimization metadata

Do not choose arbitrary levels just to fill a matrix. Malformed, null, missing, blank, unsupported, unreadable, and overflow representations usually require separate EP or negative-test treatment rather than being silently mixed into legal positive levels.

## Clarifications and status labels

Ask only questions that block safe modeling or make the expected oracle unknowable. Prioritize:

- Missing or ambiguous factors or levels
- Unknown legal, forbidden, conditional, implication, or precedence combinations
- Unclear representation or semantic class for numeric, date/time, string, file, or Boolean values
- Unknown status, message, calculation, persistence, processing path, or state-change oracle
- Unspecified interaction strength when risk may require more than 2-way coverage
- Unclear role, state, locale, time zone, environment, or existing-data preconditions

If clarification is not essential, proceed and label information explicitly as one of:

- **Confirmed** — stated directly in the supplied requirement or contract.
- **Assumption** — introduced to make a provisional model possible.
- **Question/TBD** — unresolved and requiring confirmation.
- **Residual risk** — a meaningful area not covered by the generated design.

Never present an invented level, constraint, or behavior as Confirmed. If a blocking ambiguity remains, ask the question and provide only provisional cases that do not depend on the unknown behavior.

## Parameter, level, and dependency modeling

Model each parameter independently before generating rows. For every factor, document its meaning, representation, relevant level set, semantic class, baseline, expected outcome, dependencies, context, and status.

Classify factors as:

- Mostly independent
- Dependent or constrained
- Derived from another parameter
- State-, role-, or context-driven
- Invalid or negative-test dimension

Use EP to select levels that represent behaviorally different classes. Use BVA for numeric, length, date/time, file-size, count, quota, and threshold levels. A level can be an EP representative or a BVA `below`, `at`, or `above` value. Do not claim that one level proves all values in its class.

Conditional parameters must be modeled explicitly. Use `N/A` only when the parameter is genuinely not applicable in that configuration; do not use it to hide an unknown, omitted, or untested value.

## Constraints and legal completion

Formalize constraints before generating positive rows. Record:

- Allowed and forbidden combinations
- Conditional level availability
- Implication and precedence rules
- Dependencies on other parameters, roles, states, locale, time zone, existing data, or environment
- Whether each candidate pair has at least one legal complete-row extension

Under constraints, count a candidate pair as a required legal pair only if it can be extended to at least one legal complete configuration. List impossible or forbidden pairs with their rationale; do not silently call them uncovered.

Every generated positive row must assign all modeled factors and satisfy all applicable constraints. An invalid row may be included only as an intentional negative test with a violated constraint ID, preconditions/actions, and an exact rejection or error oracle. Invalid rows do not increase legal positive pairwise coverage.

## Selecting interaction strength

Use 2-way/all-pairs as the default only when it is justified for mostly independent parameters and the likely defect risk. Choose 3-way or higher `t`-way testing when evidence, architecture, defect history, safety/financial impact, or a critical feature indicates that three or more factors may jointly cause failure.

For explicit combinations of business conditions and actions, use decision tables or condition/cause-effect coverage. For lifecycle behavior, use state-transition testing. Do not use 2-way coverage to imply higher-order coverage.

## Repeatable Pairwise procedure

1. **Define scope and oracle.** Identify operation, requirements, acceptance criteria, and exact observable outcomes.
2. **Identify factors.** List inputs, configuration dimensions, context variables, roles, states, representations, and environmental conditions.
3. **Define levels.** Use EP classes and BVA representatives where appropriate; include reproducible baseline and risk levels.
4. **Classify dependencies.** Mark independent, dependent, derived, state-driven, role-driven, context-driven, and negative-test factors.
5. **Formalize constraints.** Document legal, forbidden, conditional, implication, precedence, and dependency rules.
6. **Check legal completion.** Determine whether each candidate pair can be extended to a legal complete row.
7. **Choose strength.** Select and justify 2-way, 3-way, or stronger `t`-way interaction coverage.
8. **Build the interaction universe.** Enumerate required legal pair IDs, or legal `t`-tuple IDs, for distinct factors. Keep excluded impossible interactions visible with reasons.
9. **Generate or construct rows.** Record method, tool/version, model, strength, constraints, seed, weights, and optimization.
10. **Review every row.** Verify positive rows are complete, representable, reproducible, and legal. Separate intentional invalid rows.
11. **Verify coverage independently.** Check every modeled level and every required legal pair/tuple. Do not treat generator output as proof.
12. **Add complementary cases.** Add baseline, BVA, EP negative, decision-table, state, exhaustive, and high-risk cases as required.
13. **Create executable cases.** Include preconditions, complete configuration, steps, exact oracle, priority, requirement reference, pair IDs, constraint IDs, and assumptions.
14. **Calculate and report coverage.** Use deduplicated IDs, not row occurrences. Report levels, per-factor-pair, overall pairwise, optional Cartesian, forbidden/negative, and higher-order metrics separately.
15. **Record gaps and revise.** List uncovered pairs/tuples, excluded pairs, untested constraints, assumptions, questions/TBDs, and residual risks. Split the model if evidence shows supposedly equivalent levels or interactions differ.

## Generator and reproducibility rules

A generator can construct candidate covering-array rows but cannot decide whether the model reflects the requirement. Review:

- Complete and meaningful factor/level selection
- Constraints and legal-row rules
- Conditional parameters and `N/A` semantics
- Duplicate rows, missing levels, illegal rows, and accidental type coercion
- Independent pair/tuple inventory and coverage
- Exact oracle for each row or outcome class
- Tool, version, model, strength, seed, constraints, weights, optimization, and other metadata needed to reproduce the design

A covering array may be unbalanced. Balance is not required for Pairwise coverage unless the project explicitly requires it. A smaller optimized array is not automatically safer, clearer, or easier to diagnose, and no fixed row count is universally sufficient.

## Default output format

Preserve a format requested by the user (Gherkin, JSON, CSV, test-management table, or another schema). If no format is requested, output Markdown with these sections:

```markdown
## Scope and oracle

## Clarifications, assumptions, questions, and residual risks

## Parameter/value model

## Constraints and dependencies

## Selected interaction strength and generation method

## Pairwise test matrix

## Pair inventory and coverage summary

## Uncovered risks and complementary techniques

## Verification checklist
```

### Parameter/value model

| Parameter ID | Meaning | Representation/type | Levels and semantic classes | Baseline | Expected behavior/oracle | Dependencies/context | Requirement reference | Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `PARAM-1` |  |  |  |  |  |  |  | Confirmed / Assumption / Question/TBD |

### Constraint/dependency model

| Constraint ID | Formal rule | Allowed/forbidden combinations | Affected parameters | Impact on legal rows or pairs | Observable consequence | Status |
| --- | --- | --- | --- | --- | --- |
| `CON-1` |  |  |  |  |  | Confirmed / Assumption / Question/TBD |

### Pair inventory

| Pair ID | Parameter A / level | Parameter B / level | Legal/required? | Legal-completion rationale | Covered test cases | Status |
| --- | --- | --- | --- | --- | --- |
| `PAIR-1` |  |  |  |  |  | Confirmed / Assumption / Question/TBD |

A required pair uses two distinct parameters and has at least one legal complete-row extension.

### Executable Pairwise test cases

| Test case ID | Title/objective | Requirement reference | Priority | Preconditions/setup | Complete configuration | Steps | Exact expected result/oracle | Covered pair IDs | Violated constraint IDs | Technique tags | Assumptions/notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `PW-001` |  |  | High/Medium/Low |  |  | 1.  2.  3.  |  |  |  | Pairwise / EP / BVA / negative |  |

For intentional invalid rows, populate violated constraint IDs and exact rejection/status oracles. For generated positive rows, all parameter values must form a legal complete configuration.

## Coverage calculations

Use deduplicated required IDs:

```text
Level/Each Choice coverage =
exercised modeled levels / total modeled levels × 100%
```

```text
Per-parameter-pair coverage =
covered legal level-pairs / required legal level-pairs × 100%
```

```text
Overall 2-way coverage =
covered required pair IDs / total required legal pair IDs × 100%
```

For `t`-way testing, replace pairs with required legal `t`-tuples. Report separately:

1. Level/Each Choice coverage.
2. Per-parameter-pair coverage for every distinct factor pair.
3. Overall deduplicated Pairwise coverage.
4. Full legal Cartesian coverage, only if exhaustive testing was intentionally attempted.
5. Forbidden/invalid-combination coverage for selected negative tests.
6. Higher-order tuple coverage when selected.
7. Row count and generation metadata.

Always list uncovered pairs/tuples, excluded/impossible pairs, untested constraints, assumptions, Question/TBD items, and residual risks. Never imply that 100% Pairwise coverage equals full combination, requirements, branch, state, or non-functional coverage.

## Limitations and complementary techniques

Pairwise alone does not cover unmodeled values or representations, malformed/null/missing/blank/unsupported/overflow input, boundaries without BVA, complex Boolean rules, state paths, higher-order interactions, full Cartesian coverage, security, performance, reliability, usability, accessibility, compatibility, or exploratory risks.

Avoid confusing Each Choice with Pairwise, same-parameter levels with a pair, impossible pairs with uncovered pairs, invalid rows with legal positive rows, covering arrays with orthogonal arrays, or generator output with requirement validation. Do not use arbitrary levels or vague oracles, assume a fixed row count, hide metadata, or remove critical risk-based cases to minimize rows.

Combine the technique with:

- **EP** for behaviorally meaningful valid and invalid levels.
- **BVA** for boundary and threshold representatives.
- **Decision tables** for explicit condition/action combinations.
- **State-transition testing** for lifecycle behavior.
- **Condition/cause-effect coverage** for complex logic.
- **3-way or higher `t`-way testing** for higher-order interaction evidence.
- **Exhaustive testing** for small or safety/financial-critical legal domains.
- **Error guessing and risk-based testing** for malformed, historical, unusual, and high-impact combinations.
- **Security, performance, accessibility, usability, reliability, and exploratory testing** for risks not modeled as ordinary functional factors.

## Manual verification checklist

Before presenting the result, verify:

- Scope, requirement basis, operation, and exact oracle are documented.
- Every relevant parameter, representation, semantic class, and level is listed.
- Levels are meaningful, reproducible, and traceable to the requirement, EP, BVA, or an explicit assumption.
- Level/Each Choice coverage is separate from Pairwise coverage.
- Every pair uses levels from two distinct parameters.
- Dependencies, legal, forbidden, conditional, implication, and precedence rules are explicit.
- Required pairs have legal complete-row extensions; impossible pairs have exclusion rationale.
- Positive rows are complete and legal; intentional negative rows are separate and have violated constraint IDs and exact negative oracles.
- Selected interaction strength is explicit and justified.
- Generator method, tool/version, model, seed, constraints, weights, and optimization are recorded where applicable.
- Every required pair or `t`-tuple is independently inventoried and checked.
- Coverage arithmetic uses deduplicated IDs, not row occurrences.
- Full Cartesian and forbidden/invalid coverage are reported separately.
- Cases contain preconditions, complete configuration, steps, priority, exact oracle, requirement reference, and traceability.
- Baseline, EP, BVA, decision-table, state, higher-order, and risk follow-ups are identified where appropriate.
- Uncovered pairs/tuples, untested constraints, assumptions, Question/TBD items, and residual risks are visible.
- No claim says Pairwise proves all values, complete combinations, requirements, states, branches, or higher-order behavior.
- Markdown headings/tables render correctly and no placeholders, duplicate headings, broken tables, or non-English text remain.

## Compact reference

For detailed terminology, workflow, templates, and self-contained examples, use:

`docs/TestDesignAndSoftwareTestingTechniques/BlackBox/pairwiseTesting.md`

Do not copy its example assumptions into a real requirement without confirmation.
