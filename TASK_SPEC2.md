# Task Specification 2: Pairwise Test Design

## Goal

Prepare a clear, structured, English-language documentation guide and a reusable project-local skill for **Pairwise Testing** (also called all-pairs or 2-way combinatorial testing).

The guide must follow the quality standard established for the project's Boundary Value Analysis documentation. A junior QA engineer should be able to replace the example requirement with a real case, model parameters and values, identify constraints, generate or review a covering array, derive executable test cases, and report pairwise coverage. The accompanying skill must make the same workflow reusable when a future user supplies a requirement, acceptance criterion, configuration matrix, or business rule.

Pairwise Testing is an interaction-focused technique. It reduces the number of configurations compared with exhaustive testing, but it does not prove that all combinations, requirements, states, branches, higher-order interactions, or non-functional properties are correct.

## Target files

The task has two future implementation targets:

1. **Pairwise guide**
   `docs/TestDesignAndSoftwareTestingTechniques/BlackBox/pairwiseTesting.md`
2. **Project-local Pairwise skill**
   `.claude/skills/pairwise-testing/SKILL.md`

The Pairwise guide currently exists but is empty. There is no source article content to preserve or correct. Author the guide from ISTQB-consistent test-design principles, the requirements below, and the conventions established by:

- `docs/TestDesignAndSoftwareTestingTechniques/BlackBox/boundaryValueAnalysis.md`
- `.claude/skills/boundary-value-analysis/SKILL.md`
- `.claude/skills/equivalence-partitioning/SKILL.md`

Do not claim that the empty Pairwise file was rewritten from supplied source material.

---

# Requirements for both deliverables

## 1. Language, structure, and audience

Both deliverables must:

- be written entirely in English;
- use a consistent Markdown heading hierarchy;
- use correctly rendered Markdown tables;
- be readable by a junior QA engineer while maintaining senior-QA precision;
- explain the technique before presenting examples;
- state assumptions explicitly when an example requirement is incomplete;
- avoid placeholders, duplicate headings, broken tables, unsupported claims, and vague expected results.

## 2. Shared guide quality requirements

The Pairwise guide must explain:

- what the technique is;
- why it is used;
- what risks it addresses;
- how to apply it step by step;
- when to use it;
- when it is insufficient;
- how to combine it with other test-design techniques.

Examples must be internally consistent and include:

- an explicit requirement assumption;
- identified parameters/factors and levels/values;
- formal constraints and legal/forbidden combinations where applicable;
- selected configurations/test rows;
- preconditions and actions;
- exact observable result oracles;
- traceability to parameters, levels, pairs, and constraints;
- checked coverage arithmetic.

The guide must include:

- a reusable test-case template that can be copied into a test-management tool;
- a practical checklist for reviewing a Pairwise design;
- limitations and common mistakes;
- explicit labels for **Confirmed**, **Assumption**, **Question/TBD**, and **Residual risk**;
- a warning that a passing covering array does not prove all values, combinations, requirements, or higher-order interactions are defect-free;
- precise oracles such as response codes, validation messages, acceptance/rejection, calculated values, persisted values, processing paths, or state changes.

---

# Pairwise Testing requirements

## 1. Definition and ISTQB alignment

Explain Pairwise Testing as a black-box, specification-based, combinatorial test-design technique for interactions among multiple parameters.

Define the following terms:

- **Parameter/factor** — an input, configuration dimension, condition, or environmental variable under test;
- **Level/value** — one selected option or representative value of a parameter;
- **Configuration/test row** — one complete assignment of levels to all modeled parameters;
- **Interaction** — the behavior produced by a combination of parameter levels;
- **Pair** — one level from each of two distinct parameters;
- **Legal combination** — a complete configuration that satisfies all stated constraints;
- **2-way/all-pairs testing** — a design in which every required legal pair of levels for every distinct parameter pair appears in at least one legal test row;
- **`t`-way testing** — a generalization in which every required legal `t`-tuple of levels appears at least once;
- **Covering array** — a set of test rows designed to cover all required interactions at a chosen strength, potentially without balanced level frequencies;
- **Pairwise coverage** — the proportion of required legal pairs exercised by the selected rows.

Use ISTQB-consistent terminology and principles. The supplied task does not name a particular CTFL syllabus version or section for Pairwise Testing, so do not invent a version-specific citation. Explain that the guide is a practical supplement, not a replacement for the current official ISTQB syllabus or project requirements.

A pair is always formed from **two different parameters**. Two levels of the same parameter are not a pair. Pairwise is not “one arbitrary pair per test” and is not the same as testing every complete combination.

## 2. Distinctions from related techniques

Clearly distinguish Pairwise Testing from:

- **Equivalence Partitioning (EP)**: EP identifies behaviorally equivalent classes; Pairwise uses meaningful selected levels to cover interactions between parameters. EP Each Choice Coverage covers each input/partition at least once, not every cross-parameter pair.
- **Boundary Value Analysis (BVA)**: BVA targets values at or around boundaries; use it to choose meaningful boundary levels before combining factors. Pairwise does not replace BVA.
- **Full Cartesian/exhaustive coverage**: tests every legal complete configuration. Pairwise normally tests a much smaller covering array and intentionally leaves many complete combinations untested.
- **Decision tables**: represent combinations of conditions and expected actions when the rules and outcome logic are explicit. Pairwise does not guarantee coverage of every business-rule combination.
- **State-transition testing**: covers events, states, and lifecycle paths; Pairwise does not replace it.
- **Orthogonal arrays**: a specific balanced mathematical construction with additional properties. A covering array may be unbalanced; a generated Pairwise set is not automatically an orthogonal array.
- **`t`-way testing**: stronger interaction coverage than 2-way when failures are likely to depend on three or more parameters, at the cost of more rows.

Pairwise is interaction coverage only. It is not requirements coverage, branch/condition coverage, state coverage, usability coverage, security coverage, performance coverage, compatibility coverage, or evidence that all higher-order interactions work.

## 3. Modeling parameters, levels, and constraints

Require the guide and skill to model parameters independently before generating rows. For every parameter, document:

- semantic meaning and input ID;
- representation and data type;
- complete relevant level set;
- semantic classes represented by each level;
- expected behavior or oracle;
- dependencies on other parameters, user roles, states, locale, time zone, existing data, or environment;
- requirement/assumption status.

Use EP to identify meaningful behavioral classes and BVA to select meaningful representatives around numeric, length, date/time, size, or count boundaries. Do not choose arbitrary values merely to fill a matrix. A level can itself be a representative from an EP partition or a BVA boundary position.

Classify parameters as:

- mostly independent;
- dependent or constrained;
- state/context-driven;
- derived from another parameter;
- invalid or negative-test dimensions.

Document every constraint, including:

- allowed combinations;
- forbidden combinations;
- conditional availability of a level;
- precedence or implication rules;
- dependencies such as “Safari is supported only on macOS”;
- whether a pair has at least one legal complete configuration.

Under constraints, count as a required pair only a pair that can be extended to at least one legal complete row. Explicitly list impossible or forbidden pairs instead of silently treating them as uncovered. Generated positive-test rows must be legal. Invalid rows may be included only when they are intentionally designed as negative tests with their own oracle and traceability.

## 4. Input and clarification policy

Accept a complete requirement, acceptance criterion, existing test matrix, API parameter set, configuration rule, multi-field form, file-upload combination, or business rule.

Extract, when available:

- operation under test;
- factors/parameters and their semantic meaning;
- levels/values, types, representations, formats, and partitions;
- expected outcomes and exact oracles;
- dependencies, roles, states, existing data, locale, time zone, and environment;
- allowed and forbidden combinations;
- desired interaction strength (`2-way`, `3-way`, or another `t`);
- risk, severity, and priority;
- generator/tool, version, seed, constraints, weighting, and optimization metadata.

Ask only questions that block safe modeling or make the expected oracle unknowable. Prioritize:

- missing or ambiguous parameter levels;
- unknown legal or forbidden combinations;
- unclear dependencies or conditional availability;
- unknown expected status, message, calculation, persistence, or state change;
- unspecified interaction strength when risk requires more than 2-way coverage;
- unclear representation or semantic class for numeric, date/time, string, or file values.

If clarification is not essential, proceed using explicit labels:

- **Confirmed** — stated in the requirement;
- **Assumption** — introduced to make a provisional model possible;
- **Question/TBD** — unresolved and requiring confirmation;
- **Residual risk** — a meaningful area not covered by the generated design.

Never present invented behavior or a guessed constraint as confirmed.

## 5. Repeatable Pairwise workflow

Use this workflow:

1. **Define scope and observable outcomes.** Identify the operation, requirements, acceptance criteria, and exact oracles.
2. **Identify parameters/factors.** List every input, configuration dimension, context variable, representation, and environmental condition that can affect behavior.
3. **Define level sets.** Use EP to model behavioral classes and BVA to choose meaningful boundary representatives where applicable. Include only relevant, reproducible levels.
4. **Classify dependencies.** Mark parameters as independent, dependent, derived, state-driven, role-driven, or context-driven.
5. **Formalize constraints.** Record legal, forbidden, conditional, and implication rules. Determine whether each candidate pair can be extended to a legal complete configuration.
6. **Select interaction strength.** Explicitly choose 2-way/all-pairs by default only when justified; choose 3-way or higher `t`-way when risk or evidence indicates higher-order interaction failures.
7. **Build the required interaction universe.** Enumerate every required legal pair for every distinct parameter pair, or every legal `t`-tuple for higher strength. Keep impossible interactions visible but excluded with a reason.
8. **Generate or construct the covering array.** Record the method, tool/version, seed, constraints, weights, optimization, and generator metadata when applicable. Do not treat generator output as proof of coverage.
9. **Review every row.** Verify that each positive row is complete and legal, all levels are representable, and no constraint is violated. Separate intentional negative rows.
10. **Verify level and pair coverage.** Confirm every required level appears and every required pair is covered at least once. A level can be covered while some of its interactions remain uncovered.
11. **Create executable cases.** Add preconditions, complete configuration, actions, exact observable oracle, priority, pair IDs, constraint IDs, and assumptions/notes.
12. **Calculate coverage independently.** Report per-parameter-pair and overall pairwise coverage, plus level/Each Choice coverage, full legal-combination coverage if attempted, and forbidden-combination negative coverage.
13. **Record gaps and risks.** List uncovered pairs or tuples, excluded/impossible pairs, untested constraints, higher-order risks, assumptions, questions/TBDs, and residual risks.
14. **Revise the model from evidence.** If supposedly equivalent levels or interactions behave differently, split the model or document the defect and update the cases.

## 6. Generator and covering-array rules

The guide must explain that a tool can generate candidate rows but cannot decide whether the model reflects the requirement. The tester remains responsible for:

- selecting complete and meaningful levels;
- validating constraints and legal-row rules;
- checking the generated pair inventory independently;
- ensuring expected outcomes are defined;
- checking the tool, version, seed, model, optimization, and reproducibility metadata;
- reviewing rows for duplicates, missing levels, illegal combinations, and accidental type coercion.

A covering array may be unbalanced: some levels or pairs can occur more often than others. Balance is not required for pairwise coverage unless the project explicitly requires it. Generator optimization can reduce row count, but a smaller array is not automatically better if it obscures risk or makes execution and diagnosis difficult.

## 7. Required examples

### Example 1: Unconstrained binary factors

Use a realistic configuration requirement with four binary parameters, for example:

- `OS`: `Windows`, `macOS`;
- `BROWSER`: `Chrome`, `Firefox`;
- `LOCALE`: `en-US`, `fr-FR`;
- `AUTH`: `Password`, `SSO`.

Assume every complete configuration is legal. Select explicit **2-way** coverage and provide a complete covering array. An 8-row array may be used, but verify it rather than claiming that a fixed count is universally sufficient.

The example must include:

- a parameter/value model with status labels;
- preconditions, setup, and actions;
- exact observable oracle, such as HTTP `200`, successful rendering, and persistence of the four selected settings;
- complete rows with stable test-case IDs;
- pair IDs for all six distinct parameter pairs;
- coverage arithmetic demonstrating 24 required legal level pairs and 100% pairwise coverage if the selected rows cover all of them;
- separate level/Each Choice coverage and an explicit statement that full Cartesian coverage is not claimed.

One valid 8-row design is possible for the four binary factors; verify all six factor pairs rather than presenting an unverified matrix.

### Example 2: Constrained multi-valued compatibility

Use at least three parameters with multiple levels, for example:

- `OS`: `Windows`, `macOS`, `Linux`;
- `BROWSER`: `Chrome`, `Firefox`, `Safari`;
- `PAYMENT`: `Card`, `PayPal`.

Define explicit assumptions and constraints such as:

- `Safari` is supported only on `macOS`;
- `PayPal` is unavailable on `Linux`.

The example must show:

- the formal allowed and forbidden rules;
- a distinction between a forbidden pair and an uncovered required pair;
- the legal-complete-row rule for counting required pairs;
- a legal covering array or a manually verified legal matrix;
- exact valid oracles and exact negative-test oracles for intentionally exercised forbidden combinations;
- pair IDs, constraint IDs, complete configurations, and coverage arithmetic;
- level coverage and pairwise coverage reported separately;
- any excluded/impossible pairs explicitly listed with rationale.

### Example 3: Higher-order interaction risk

Provide an example where all 2-way pairs are covered but a defect requires a specific combination of three or more parameters. Explain that 2-way coverage can miss this defect and show one of the following:

- a 3-way test requirement and the additional tuples that must be covered;
- a decision table for a business rule requiring specific condition combinations;
- state-transition testing for an interaction involving lifecycle state;
- BVA/EP follow-up for a level that represents a boundary or behavioral class.

State when the tester should increase strength to `3-way` or higher and when another technique is more appropriate. Do not claim pairwise alone covers the higher-order behavior.

Every worked example must be self-contained, state assumptions, provide executable actions and exact oracles, and preserve traceability from requirement to parameter, level, pair/tuple, row, and result.

## 8. Coverage requirements

For every distinct parameter pair `(Pi, Pj)`, calculate:

`covered legal level-pairs / required legal level-pairs × 100%`

Calculate overall 2-way coverage using deduplicated required pair IDs, not row occurrences:

`sum of covered required pair IDs / sum of required pair IDs × 100%`

For `t`-way testing, replace pairs with required legal `t`-tuples:

`covered legal t-tuples / required legal t-tuples × 100%`

Report these metrics separately:

1. **Level/Each Choice coverage** — whether every modeled level appears at least once.
2. **Per-parameter-pair coverage** — coverage for each distinct parameter pair.
3. **Overall pairwise coverage** — deduplicated required pair IDs covered by the design.
4. **Full legal Cartesian coverage** — only if exhaustive coverage was intentionally attempted.
5. **Forbidden/invalid-combination coverage** — negative tests for selected forbidden combinations, reported separately from legal positive coverage.
6. **Row count and generation metadata** — number of rows, tool/version, seed, constraints, weights, and optimization settings when applicable.

Explicitly list uncovered pairs/tuples, excluded or impossible pairs, untested constraints, and residual risks. Do not imply that 100% pairwise coverage equals full combination, requirement, state, branch, or non-functional coverage.

## 9. Recommended output structure

When no project-specific format is supplied, use:

1. Scope and oracle
2. Clarifications, assumptions, questions, and residual risks
3. Parameter/value model
4. Constraints and dependencies
5. Selected strength and generation method
6. Pairwise test matrix
7. Pair inventory and coverage summary
8. Uncovered risks and complementary techniques
9. Verification checklist

### Parameter/value model table

| Parameter ID | Meaning | Representation/type | Levels and semantic classes | Expected behavior/oracle | Dependencies/context | Status |
| --- | --- | --- | --- | --- | --- | --- |
| `PARAM-1` |  |  |  |  |  | Confirmed / Assumption / Question/TBD |

### Constraint/dependency table

| Constraint ID | Formal rule | Allowed/forbidden combinations | Impact on legal rows or pairs | Status |
| --- | --- | --- | --- | --- |
| `CON-1` |  |  |  | Confirmed / Assumption / Question/TBD |

### Pair inventory table

| Pair ID | Parameter A / level | Parameter B / level | Legal/required? | Legal-completion rationale | Covered test cases | Status |
| --- | --- | --- | --- | --- | --- | --- |
| `PAIR-1` |  |  |  |  |  | Confirmed / Assumption / Question/TBD |

A required pair must be from two distinct parameters and have at least one legal complete-row completion under the documented constraints.

### Pairwise test-case table

| Test case ID | Title/objective | Requirement reference | Priority | Preconditions/setup | Complete configuration | Steps | Exact expected result/oracle | Covered pair IDs | Technique tags | Assumptions/notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `PW-001` |  |  | High/Medium/Low |  |  | 1.  2.  3.  |  |  | Pairwise / EP / BVA / negative |  |

For intentionally invalid rows, include the violated constraint IDs and the exact rejection/status oracle. For generated positive rows, all parameter values must form a legal complete configuration.

## 10. When to use Pairwise Testing

Pairwise Testing is especially useful when:

- several mostly independent parameters have multiple meaningful values;
- interaction defects are plausible;
- the full legal Cartesian product is too large for available time or resources;
- configuration, compatibility, localization, device, browser, platform, feature-flag, or API parameter combinations must be sampled systematically;
- the team needs traceable interaction coverage rather than an ad hoc subset of configurations.

Use it after basic functionality and the individual parameter levels are understood. Stabilize the primary workflow and validate each important level before interpreting failures in a dense combination matrix.

## 11. Limitations and common mistakes

Pairwise alone is insufficient for:

- values or representations not modeled as levels;
- malformed, null, missing, blank, unsupported, unreadable, or overflow input;
- numeric, length, date/time, file-size, count, or quota boundaries unless levels are deliberately selected with BVA;
- three-way or higher-order interaction defects;
- state transitions and lifecycle rules;
- complex Boolean logic and business-rule combinations that require decision tables or condition/cause-effect coverage;
- security, performance, reliability, usability, accessibility, compatibility, and exploratory risks;
- complete legal Cartesian coverage and every requirement outcome.

Avoid these mistakes:

1. **Calling Each Choice Coverage pairwise coverage.** Every level appearing once does not ensure every cross-parameter pair appears.
2. **Counting same-parameter levels as a pair.** A pair uses two distinct parameters.
3. **Ignoring constraints.** A generated row that violates a forbidden combination is not a valid positive test.
4. **Treating an impossible pair as merely uncovered.** Determine whether it has a legal complete-row extension and document exclusions.
5. **Choosing arbitrary levels.** Use EP and BVA to make levels behaviorally meaningful and boundary-aware.
6. **Using a generator without reviewing the model.** Tools cannot detect missing requirements, wrong levels, or incorrect constraints.
7. **Assuming a fixed row count.** The number depends on factor count, level count, strength, constraints, and optimization.
8. **Confusing row occurrences with pair coverage.** Deduplicate required pair IDs when calculating coverage.
9. **Changing the oracle per row without documenting it.** Define expected status, calculation, persistence, and state outcomes for each relevant configuration.
10. **Combining invalid levels into positive rows unintentionally.** Negative combinations require explicit intent, violated constraint traceability, and rejection or error oracles.
11. **Ignoring higher-order risk.** Increase to `t`-way testing or use decision tables/state-transition testing when pairwise is not strong enough.
12. **Hiding generation metadata.** Record tool/version, model, constraints, seed, weights, and optimization for reproducibility.
13. **Treating an orthogonal array and covering array as synonyms.** An orthogonal array has stronger balance properties; a covering array need not be balanced.

## 12. Complementary techniques

Explain how Pairwise Testing works with:

- **Equivalence Partitioning** — derives behaviorally meaningful levels and invalid classes;
- **Boundary Value Analysis** — selects lower, upper, and threshold representatives before combining parameters;
- **Decision tables** — covers explicit combinations of conditions and actions;
- **State-transition testing** — covers lifecycle/state-dependent interactions;
- **Condition/cause-effect coverage** — covers complex Boolean relationships;
- **3-way or higher `t`-way testing** — covers higher-order interactions when 2-way is insufficient;
- **Error guessing** — adds malformed, unusual, coercion, locale, and historically defective combinations;
- **Risk-based testing** — prioritizes financial, security, compatibility, safety, and high-impact interactions;
- **Exploratory testing** — investigates behavior not predicted by the model.

A practical sequence is to use EP and BVA to define levels, use Pairwise for mostly independent interaction coverage, use constraints and decision tables for legal business combinations, use state-transition testing for lifecycle context, and increase interaction strength or add risk-based tests where evidence requires it.

## 13. Verification checklist

Before approving a Pairwise design, confirm:

- [ ] Scope, requirement basis, operation, and exact observable oracle are documented.
- [ ] Every relevant parameter/factor is listed with its semantic meaning and representation.
- [ ] Every level is meaningful, reproducible, and derived from the requirement, EP, BVA, or an explicit assumption.
- [ ] Level/Each Choice coverage is reported separately from pairwise coverage.
- [ ] A pair always uses levels from two distinct parameters.
- [ ] Dependencies, allowed combinations, forbidden combinations, and conditional levels are formalized.
- [ ] A required pair is counted only when it has at least one legal complete-row extension.
- [ ] Positive generated rows are complete and legal; invalid rows are intentional and separately labeled.
- [ ] The selected strength (`2-way`, `3-way`, or higher) is explicit and justified.
- [ ] Covering-array generation method, tool/version, seed, constraints, weights, and optimization are recorded where applicable.
- [ ] Every required legal pair or `t`-tuple is inventoried and independently verified.
- [ ] Coverage arithmetic uses deduplicated required pair/tuple IDs, not row occurrences.
- [ ] Full legal Cartesian coverage is reported separately and only if attempted.
- [ ] Forbidden/invalid-combination coverage is reported separately from valid pairwise coverage.
- [ ] Each test case has preconditions, complete configuration, actions, priority, exact oracle, and traceability.
- [ ] Uncovered pairs/tuples, excluded/impossible interactions, untested constraints, assumptions, Question/TBD items, and residual risks are visible.
- [ ] Pairwise limitations for malformed input, boundaries, interiors, states, higher-order interactions, security, performance, usability, accessibility, compatibility, and exploration are documented.
- [ ] EP, BVA, decision tables, state-transition, condition/cause-effect, `t`-way, error-guessing, and risk-based follow-ups are recommended where appropriate.
- [ ] No claim states that Pairwise proves all values, all combinations, all requirements, or all higher-order behavior.
- [ ] Markdown headings and tables render correctly; no duplicate headings, placeholders, broken tables, or non-English text remain.

## Quality and acceptance criteria

Before considering the work complete, verify that:

- The Pairwise guide and Pairwise skill are both standalone English deliverables.
- The guide explicitly notes that `pairwiseTesting.md` was empty and that no source rewrite is being claimed.
- The skill references the guide using the exact project-relative path:
  `docs/TestDesignAndSoftwareTestingTechniques/BlackBox/pairwiseTesting.md`.
- The skill follows the project's existing frontmatter convention: `name`, trigger-oriented `description`, and `version: 0.1.0`.
- The guide defines parameters, levels, rows/configurations, interactions, legal combinations, pairs, 2-way, `t`-way, covering arrays, and coverage.
- The guide states that a pair uses two distinct parameters and that 2-way coverage requires every legal level pair at least once.
- Pairwise is clearly distinguished from EP Each Choice Coverage, BVA, decision tables, full Cartesian coverage, orthogonal arrays, state-transition testing, and higher-order testing.
- Constraints and legal-completion logic are explicit; impossible/forbidden pairs are not silently counted as missing required coverage.
- Meaningful levels use EP/BVA where applicable, and malformed/invalid representations are not confused with valid interaction levels.
- The guide includes all three required worked-example categories: unconstrained binary, constrained multi-valued, and higher-order interaction risk.
- Every example has assumptions, complete rows, preconditions, actions, exact oracles, traceability, and checked coverage arithmetic.
- Coverage reports level/Each Choice, per-parameter-pair, total pairwise, full Cartesian if attempted, and forbidden/negative coverage separately.
- The reusable tables contain the required parameter, constraint, pair inventory, and test-case fields.
- Limitations, common mistakes, complementary techniques, assumptions, questions/TBDs, and residual risks are visible.
- No unsupported fixed-test-count or all-behavior claims remain.
- Markdown renders correctly with no duplicated headings, placeholders, broken tables, or non-English text.
- Only the requested guide and skill files are modified during the implementation; unrelated project files remain unchanged.
