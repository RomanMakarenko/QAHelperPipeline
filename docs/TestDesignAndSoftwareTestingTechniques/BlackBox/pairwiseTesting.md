# Pairwise Testing

## Purpose and scope

Pairwise Testing, also called **all-pairs testing** or **2-way combinatorial testing**, is a black-box, specification-based test-design technique for interactions among multiple parameters. It selects a comparatively small set of complete configurations so that every required legal pair of levels from every two distinct parameters appears at least once.

The technique addresses interaction defects while reducing the number of configurations compared with exhaustive testing. It does not prove that all values, all complete combinations, all requirements, all branches, all states, all higher-order interactions, or any non-functional property are correct.

> This file was empty before this guide was authored. No rewrite of supplied source material is claimed.

This guide uses ISTQB-consistent test-design terminology as a practical supplement. It is not a replacement for the current official ISTQB syllabus, product requirements, acceptance criteria, or project test strategy.

## Core principle

For parameters `P1` and `P2`, a pair consists of one level of `P1` and one level of `P2`. A 2-way design requires every **required legal** pair for every distinct parameter pair to occur in at least one **legal complete test row**.

A pair is not two levels of the same parameter, one arbitrary pair selected from a test case, or one complete configuration. Pairwise coverage is interaction coverage; it is not full Cartesian coverage.

Pairwise is useful when several parameters are mostly independent, each has meaningful values, and failures may depend on their interaction. A covering array can reduce redundant combinations, but a smaller array is not automatically a better design: completeness of the model, legality of rows, observability of outcomes, and product risk remain the tester's responsibility.

## Terminology

| Term | Definition |
| --- | --- |
| **Parameter/factor** | An input, configuration dimension, condition, environmental variable, or context variable under test. |
| **Level/value** | One selected option or representative value of a parameter. A level may represent an EP class or a BVA position. |
| **Configuration/test row** | One complete assignment of levels to all modeled parameters. |
| **Interaction** | Behavior produced by a combination of parameter levels. |
| **Pair** | One level from each of two distinct parameters. |
| **Legal combination** | A complete configuration satisfying all stated constraints and preconditions. |
| **Legal completion** | At least one legal complete row that extends a candidate pair. |
| **2-way/all-pairs testing** | A design in which every required legal pair appears in at least one legal row. |
| **`t`-way testing** | A generalization in which every required legal `t`-tuple appears at least once. |
| **Covering array** | A set of rows designed to cover required interactions at a selected strength. It need not be balanced. |
| **Pairwise coverage** | Covered required legal pair IDs divided by all required legal pair IDs. |
| **Each Choice Coverage** | Coverage in which every modeled parameter/level (or input/partition) appears at least once. It does not guarantee every cross-parameter pair. |
| **Constraint** | A rule that permits, forbids, conditions, or orders combinations of levels. |
| **Forbidden combination** | A combination that cannot be a positive legal configuration, or that is intentionally exercised as a negative test. |
| **Conditional parameter** | A parameter whose levels apply only in a particular context, such as a payment detail shown only for a selected payment method. |

Under constraints, a candidate pair is a required legal pair only if it can be extended to at least one legal complete configuration. Impossible pairs must be listed as excluded with a reason; they must not be silently reported as missing coverage.

## Distinguishing Pairwise from related techniques

| Technique | Primary question | What Pairwise does differently |
| --- | --- | --- |
| **Equivalence Partitioning (EP)** | Which values are expected to behave equivalently? | Uses selected levels to cover interactions between parameters. EP Each Choice Coverage covers each input/partition at least once, not every cross-parameter pair. |
| **Boundary Value Analysis (BVA)** | What happens below, at, and above a boundary? | Combines selected boundary representatives with other parameters; it does not replace BVA. |
| **Full Cartesian/exhaustive testing** | Has every legal complete configuration been executed? | Usually executes a smaller covering array and intentionally leaves many complete configurations untested. |
| **Decision tables** | What action follows each relevant combination of conditions? | Does not guarantee every explicit business-rule combination or action rule. |
| **State-transition testing** | What happens for events and paths through states? | Does not replace lifecycle and state-path coverage. A state or event may be a factor, but Pairwise alone does not cover paths. |
| **Orthogonal arrays** | Is there a balanced mathematical design with additional properties? | A covering array may be unbalanced and is not automatically an orthogonal array. |
| **`t`-way testing** | Are interactions among `t` parameters covered? | 3-way or stronger testing covers higher-order tuples and generally requires more rows. |

Even 100% Pairwise coverage is not requirements coverage, branch or condition coverage, state coverage, compatibility coverage, security coverage, performance coverage, usability coverage, accessibility coverage, or evidence that all higher-order interactions work.

## Status labels and clarification policy

Use these labels in the model and in examples:

- **Confirmed** — stated directly in the requirement, acceptance criterion, or observed contract.
- **Assumption** — introduced to make a provisional model or self-contained example possible.
- **Question/TBD** — unresolved and requiring confirmation.
- **Residual risk** — a meaningful area not covered by the current design.

Ask only questions that block safe modeling or make the expected oracle unknowable. Prioritize:

- missing or ambiguous parameters or levels;
- unknown legal, forbidden, conditional, or implication combinations;
- unclear representations or semantic classes for numeric, date/time, string, or file values;
- unknown response status, validation message, calculated result, persistence, processing path, or state change;
- unspecified interaction strength where risk may require more than 2-way coverage;
- unclear role, account state, lifecycle state, locale, time zone, existing data, or environment.

If clarification is not essential, proceed with an explicit **Assumption** and state the resulting **Residual risk**. Never present an invented behavior or guessed constraint as **Confirmed**.

## Input contract and preparation

Pairwise can be applied to a complete requirement, acceptance criterion, existing test matrix, API parameter set, multi-field form, compatibility matrix, configuration rule, file-upload combination, feature-flag set, or business rule.

Extract, where available:

- operation under test and requirement references;
- each factor and its semantic meaning;
- level values, representations, data types, formats, and behavioral classes;
- baseline/default level and high-risk levels;
- exact expected outcomes and observable oracles;
- independent, dependent, derived, state-driven, role-driven, and context-driven relationships;
- legal, forbidden, conditional, precedence, and implication constraints;
- locale, time zone, reference date, environment, and existing-data assumptions;
- desired interaction strength (`2-way`, `3-way`, or higher);
- risk, severity, priority, and historically defective combinations;
- generator/tool, version, model, seed, weights, and optimization metadata.

Do not choose arbitrary levels merely to fill a matrix. Use **EP** to identify meaningful behavioral classes and **BVA** to choose representatives around numeric, length, date/time, file-size, count, or quota boundaries. Malformed, null, missing, blank, unsupported, unreadable, and overflow representations need their own negative or EP design unless the requirement explicitly makes them ordinary levels.

## Repeatable Pairwise workflow

1. **Define scope and observable outcomes.** Identify the operation, requirement basis, acceptance criteria, and exact oracle: status code, message, acceptance/rejection, calculation, persisted value, processing path, or state change.
2. **Identify factors.** List inputs, configuration dimensions, context variables, roles, states, representations, and environmental conditions that can affect behavior.
3. **Define level sets.** Select complete, relevant, reproducible levels. Use EP classes and BVA representatives where applicable; record baseline and risk-driven levels.
4. **Classify dependencies.** Mark factors as mostly independent, dependent/constrained, derived, state/context-driven, role-driven, or negative-test dimensions.
5. **Formalize constraints.** Record allowed, forbidden, conditional, implication, precedence, and dependency rules before generating rows.
6. **Check legal completion.** Determine whether each candidate pair can be extended to at least one legal complete configuration. Keep impossible pairs visible but excluded with a reason.
7. **Select interaction strength.** Use 2-way as a justified default for mostly independent parameters. Select 3-way or higher when risk, evidence, defect history, or a critical rule indicates higher-order failures.
8. **Build the interaction universe.** Enumerate required legal pair IDs (or legal `t`-tuples) for every distinct factor combination. Do not count two levels of one factor as a pair.
9. **Generate or construct a covering array.** Record the method, tool/version, model, strength, constraints, seed, weights, and optimization settings. A generator produces candidates; it does not validate requirements.
10. **Review every row.** Verify every positive row is complete, representable, reproducible, and legal. Separate intentionally invalid rows and their violated constraint IDs.
11. **Verify level and pair coverage independently.** Check every required level and every required legal pair, preferably with an independent script or manually auditable pair inventory.
12. **Add required non-generated cases.** Add baseline/smoke cases, BVA cases, EP negative cases, decision-table rules, state paths, and high-risk combinations as appropriate. Do not remove a critical case merely to preserve a minimal row count.
13. **Create executable test cases.** Include preconditions, complete configuration, actions, exact oracle, priority, requirement reference, pair IDs, constraint IDs where applicable, and assumptions/notes.
14. **Calculate and report coverage.** Report level/Each Choice, per-parameter-pair, deduplicated overall pairwise, optional full legal Cartesian, forbidden/negative, and higher-order coverage separately.
15. **Record gaps and revise the model.** List uncovered pairs/tuples, excluded pairs, untested constraints, assumptions, questions/TBDs, and residual risks. If supposedly equivalent levels or interactions behave differently, split the model or document the defect.

## Constraints and legal rows

Constraints must be applied before generating positive rows. Examples include:

- `Safari` is supported only on `macOS`;
- `PayPal` is unavailable on `Linux`;
- `MFA method` is applicable only when `MFA = On`;
- `Express shipping` requires a destination in a supported region;
- a derived value must agree with its source parameter.

A positive row must assign a value to every modeled factor and satisfy every applicable constraint. Use `N/A` only when a parameter is genuinely not applicable in that configuration; it is not a shortcut for an unknown or untested value.

An invalid row may be included only intentionally as a negative test. Identify the violated constraint and define an exact oracle such as HTTP `400`, a specific validation message, no record creation, no payment attempt, or a defined fallback path. Invalid rows do not increase legal positive pairwise coverage.

A nominal pair can be excluded for two different reasons:

1. It has no legal complete-row extension under the constraints and is therefore not a required legal pair.
2. It is a legal pair but is not present in the selected rows and is therefore an uncovered coverage gap.

Document the distinction explicitly.

## Generator and covering-array guidance

A tool such as PICT, a constraint-aware combinatorial generator, or a custom algorithm can construct candidate rows. Tool choice is not the test design itself. The tester remains responsible for:

- checking that all meaningful levels are modeled;
- validating constraints against the requirement;
- checking conditional parameters and `N/A` semantics;
- independently rebuilding the required pair inventory;
- reviewing duplicates, missing levels, illegal rows, and accidental type coercion;
- defining an oracle for every row or outcome class;
- recording generator, version, model, strength, seed, constraints, weights, optimization, and other reproducibility metadata.

A covering array may be unbalanced: pairwise coverage does not require each level to occur equally often unless the project says so. Optimization may reduce row count, but a smaller array can be harder to diagnose or can hide a high-risk combination. Do not assume a fixed number of rows is universally sufficient.

## Reusable design templates

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

A required pair must use two distinct parameters and have at least one legal complete-row extension.

### Executable Pairwise test case

| Test case ID | Title/objective | Requirement reference | Priority | Preconditions/setup | Complete configuration | Steps | Exact expected result/oracle | Covered pair IDs | Violated constraint IDs | Technique tags | Assumptions/notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `PW-001` |  |  | High/Medium/Low |  |  | 1.  2.  3.  |  |  |  | Pairwise / EP / BVA / negative |  |

Use exact oracles. Examples are HTTP `200` or `400`, an exact validation message, an exact calculated amount, accepted/rejected status, persisted values equal to submitted values, a named processing path, or a defined state transition. “The system works correctly” is not an oracle.

## Worked examples

The examples below are self-contained teaching models. Their requirements are **Assumptions**, not product facts.

### Example 1: Four unconstrained binary factors

#### Assumed requirement and model

A configuration endpoint accepts these four parameters:

| Parameter ID | Meaning | Levels | Status |
| --- | --- | --- | --- |
| `OS` | Operating system | `Windows`, `macOS` | Assumption |
| `BROWSER` | Browser | `Chrome`, `Firefox` | Assumption |
| `LOCALE` | UI locale | `en-US`, `fr-FR` | Assumption |
| `AUTH` | Authentication mode | `Password`, `SSO` | Assumption |

- **Assumption E1:** every `2 × 2 × 2 × 2 = 16` complete configuration is legal.
- **Assumption E2:** a valid submission returns HTTP `200 OK`, renders the main screen, and persists all four selected settings unchanged.
- **Residual risk:** the array does not test every complete configuration, malformed representations, performance, security, or higher-order behavior.

#### Selected 2-way array

The following rows use a verified binary covering array:

| Test case ID | OS | BROWSER | LOCALE | AUTH |
| --- | --- | --- | --- | --- |
| `PW1-001` | Windows | Chrome | en-US | Password |
| `PW1-002` | Windows | Chrome | fr-FR | SSO |
| `PW1-003` | Windows | Firefox | en-US | SSO |
| `PW1-004` | Windows | Firefox | fr-FR | Password |
| `PW1-005` | macOS | Chrome | en-US | SSO |
| `PW1-006` | macOS | Chrome | fr-FR | Password |
| `PW1-007` | macOS | Firefox | en-US | Password |
| `PW1-008` | macOS | Firefox | fr-FR | SSO |

Each row is a complete legal configuration. Pair IDs can be named by factor pair and levels, for example `PAIR-OS-BROWSER-Windows-Chrome` and `PAIR-LOCALE-AUTH-fr-FR-SSO`.

#### Preconditions, actions, and oracle

Common preconditions for every row:

- the endpoint is available;
- the test account is authorized;
- no conflicting configuration exists;
- the execution environment matches the row's OS and browser.

Actions:

1. Set `OS`, `BROWSER`, `LOCALE`, and `AUTH` to the values in the row.
2. Submit or save the configuration.
3. Reload the configuration and inspect the response.

Exact oracle:

- HTTP response is `200 OK`;
- the main screen renders without an error;
- persisted `OS`, `BROWSER`, `LOCALE`, and `AUTH` equal the submitted values;
- no value is silently coerced to a fallback.

#### Coverage arithmetic

There are `C(4,2) = 6` distinct factor pairs. Each factor pair has `2 × 2 = 4` possible level pairs, so the required deduplicated universe contains `6 × 4 = 24` pair IDs. The eight rows above cover all four level pairs for each of the six factor pairs:

| Factor pair | Required legal pairs | Covered | Coverage |
| --- | ---: | ---: | ---: |
| `OS × BROWSER` | 4 | 4 | 100% |
| `OS × LOCALE` | 4 | 4 | 100% |
| `OS × AUTH` | 4 | 4 | 100% |
| `BROWSER × LOCALE` | 4 | 4 | 100% |
| `BROWSER × AUTH` | 4 | 4 | 100% |
| `LOCALE × AUTH` | 4 | 4 | 100% |
| **Overall** | **24** | **24** | **100%** |

All eight levels appear at least once; in fact, each level appears in four of eight rows. The selected suite covers `8 / 16 = 50%` of the full Cartesian configurations. **Full exhaustive coverage is not claimed.**

### Example 2: Constrained multi-valued compatibility

#### Assumed requirement and constraints

A checkout page is tested with:

| Parameter ID | Meaning | Levels | Status |
| --- | --- | --- | --- |
| `OS` | Client operating system | `Windows`, `macOS`, `Linux` | Assumption |
| `BROWSER` | Browser | `Chrome`, `Firefox`, `Safari` | Assumption |
| `PAYMENT` | Payment method | `Card`, `PayPal` | Assumption |

Constraints:

| Constraint ID | Formal rule | Effect | Status |
| --- | --- | --- | --- |
| `CON-1` | `BROWSER = Safari ⇒ OS = macOS` | `Windows + Safari` and `Linux + Safari` cannot be legal positive rows. | Assumption |
| `CON-2` | `OS = Linux ⇒ PAYMENT ≠ PayPal` | `Linux + PayPal` cannot be a legal positive row. | Assumption |

**Assumption E3:** all combinations not excluded by `CON-1` or `CON-2` are legal. A valid row returns HTTP `200 OK`, loads checkout, preserves the selected browser and payment method, and shows no unsupported-combination warning. An invalid row returns the exact error stated below, creates no checkout session, and does not attempt payment authorization.

#### Legal positive covering rows

| Test case ID | OS | BROWSER | PAYMENT | Covered constraint status |
| --- | --- | --- | --- | --- |
| `PW2-001` | Windows | Chrome | Card | Legal |
| `PW2-002` | Windows | Firefox | PayPal | Legal |
| `PW2-003` | macOS | Chrome | PayPal | Legal |
| `PW2-004` | macOS | Firefox | Card | Legal |
| `PW2-005` | macOS | Safari | Card | Legal |
| `PW2-006` | macOS | Safari | PayPal | Legal |
| `PW2-007` | Linux | Chrome | Card | Legal |
| `PW2-008` | Linux | Firefox | Card | Legal |
| `PW2-009` | macOS | Firefox | PayPal | Legal |

Common preconditions: checkout service is available, the test user is authorized, and no checkout session exists for the test order. Actions: set the complete row, open checkout, submit the configuration, and inspect the response and resulting session. Exact positive oracle: HTTP `200 OK`; checkout loads; the selected values are displayed unchanged; a checkout session is created; no unsupported warning appears.

#### Legal-completion analysis and pair inventory

For `OS × BROWSER`, there are nine nominal pairs. `Windows + Safari` and `Linux + Safari` have no legal completion because of `CON-1`, leaving seven required legal pairs. All seven occur in the positive rows: `Windows + Chrome`, `Windows + Firefox`, `macOS + Chrome`, `macOS + Firefox`, `macOS + Safari`, `Linux + Chrome`, and `Linux + Firefox`.

For `OS × PAYMENT`, there are six nominal pairs. `Linux + PayPal` has no legal completion because of `CON-2`, leaving five required legal pairs. All five occur: `Windows + Card`, `Windows + PayPal`, `macOS + Card`, `macOS + PayPal`, and `Linux + Card`.

For `BROWSER × PAYMENT`, every `3 × 2 = 6` pair has a legal completion and all six occur in the rows.

| Factor pair | Required legal pairs | Covered | Coverage | Excluded impossible pairs |
| --- | ---: | ---: | ---: | --- |
| `OS × BROWSER` | 7 | 7 | 100% | `Windows + Safari`, `Linux + Safari` (`CON-1`) |
| `OS × PAYMENT` | 5 | 5 | 100% | `Linux + PayPal` (`CON-2`) |
| `BROWSER × PAYMENT` | 6 | 6 | 100% | None |
| **Overall** | **18** | **18** | **100%** | Listed above |

All eight levels appear at least once. There are `4` legal Windows configurations, `6` legal macOS configurations, and `2` legal Linux configurations: `12` legal complete configurations in total. The nine selected positive rows cover `9 / 12`; exhaustive legal coverage is not claimed.

#### Intentional forbidden-combination tests

These rows are not positive covering-array rows and do not increase legal pairwise coverage:

| Test case ID | OS | BROWSER | PAYMENT | Violated constraint | Preconditions/actions | Exact negative oracle |
| --- | --- | --- | --- | --- | --- | --- |
| `PW2-N001` | Windows | Safari | Card | `CON-1` | Authorized user; submit the complete row. | HTTP `400 Bad Request`; exact message `Browser Safari is supported only on macOS`; no checkout session is created. |
| `PW2-N002` | Linux | Chrome | PayPal | `CON-2` | Authorized user; submit the complete row. | HTTP `400 Bad Request`; exact message `PayPal is unavailable on Linux`; no payment authorization is attempted. |

The forbidden/invalid-combination metric is therefore `2 / 2 = 100%` for these selected negative rules, reported separately from the `18 / 18 = 100%` legal positive pairwise metric. The nominal forbidden pairs are excluded, not uncovered required pairs.

### Example 3: Higher-order interaction risk

#### Assumed model

A settings operation accepts:

| Parameter ID | Meaning | Levels | Status |
| --- | --- | --- | --- |
| `PLAN` | Subscription plan | `Basic`, `Pro` | Assumption |
| `LOCALE` | UI locale | `en-US`, `fr-FR` | Assumption |
| `MFA` | Multi-factor authentication | `Off`, `On` | Assumption |

**Assumption E4:** all eight complete configurations are legal. For every legal configuration, the expected result is HTTP `200 OK`, a loaded dashboard, persisted plan/locale/MFA values equal to the request, and labels localized to the requested locale.

#### 2-way array and its limit

| Test case ID | PLAN | LOCALE | MFA |
| --- | --- | --- | --- |
| `PW3-001` | Basic | en-US | Off |
| `PW3-002` | Basic | fr-FR | On |
| `PW3-003` | Pro | en-US | On |
| `PW3-004` | Pro | fr-FR | Off |

Common preconditions: the account is authorized, settings storage is empty or reset, and the settings service is available. Actions: submit the complete row, reload settings, and inspect the response, labels, and persisted values. Exact oracle: HTTP `200 OK`; dashboard loads; all three submitted values persist unchanged; localized labels match the selected locale.

The array covers `PLAN × LOCALE`, `PLAN × MFA`, and `LOCALE × MFA` at `4 / 4 = 100%` each, for `12 / 12 = 100%` overall 2-way coverage. However, the triple `Pro + fr-FR + On` is absent.

**Residual risk R1:** a defect may occur only when `PLAN = Pro`, `LOCALE = fr-FR`, and `MFA = On`, returning HTTP `500`, showing the wrong language, or failing to persist MFA even though every individual level and every 2-way pair passes.

#### Appropriate higher-order follow-up

Add the targeted 3-way case:

| Test case ID | PLAN | LOCALE | MFA | Exact oracle |
| --- | --- | --- | --- | --- |
| `PW3-005` | Pro | fr-FR | On | HTTP `200 OK`; dashboard loads; French labels are displayed; `Pro`, `fr-FR`, and `MFA=On` persist unchanged. |

The complete 3-way universe has `2 × 2 × 2 = 8` tuples. The initial four rows cover `4 / 8 = 50%`; the missing tuples are `Basic + en-US + On`, `Basic + fr-FR + Off`, `Pro + en-US + Off`, and `Pro + fr-FR + On`. If all higher-order tuples are required, add the four missing rows and report `8 / 8 = 100%` 3-way coverage.

Choose 3-way or stronger testing when risk, defect history, architecture, or evidence indicates that three or more parameters can jointly cause failure. Use a decision table when the defect is an explicit combination of business conditions and actions, state-transition testing for lifecycle paths, and EP/BVA for missing behavioral classes or boundaries. Pairwise alone does not cover this higher-order defect.

## Coverage reporting

Calculate coverage using deduplicated required pair or tuple IDs, not row occurrences.

### Level/Each Choice coverage

```text
Level/Each Choice coverage =
exercised modeled levels / total modeled levels × 100%
```

For multiple inputs, state whether every parameter/level pair was exercised. This metric does not guarantee cross-parameter interaction coverage.

### Per-parameter-pair coverage

For every distinct parameter pair `(Pi, Pj)`:

```text
Per-parameter-pair coverage =
covered legal level-pairs / required legal level-pairs × 100%
```

### Overall pairwise coverage

```text
Overall 2-way coverage =
covered required pair IDs / total required legal pair IDs × 100%
```

A pair ID is counted once even if several rows cover it. For `t`-way testing, replace pairs with required legal `t`-tuples.

### Other required metrics

Report separately:

1. **Level/Each Choice coverage**.
2. **Per-parameter-pair coverage** for every distinct factor pair.
3. **Overall deduplicated pairwise coverage**.
4. **Full legal Cartesian coverage**, only when exhaustive legal testing was intentionally attempted.
5. **Forbidden/invalid-combination coverage**, for selected negative rows and constraint rules.
6. **Higher-order coverage**, such as 3-way tuple coverage, when selected.
7. **Row count and generation metadata**, including tool/version, model, strength, seed, constraints, weights, and optimization settings.

Always list uncovered pairs/tuples, excluded or impossible pairs, untested constraints, assumptions, questions/TBDs, and residual risks. Do not imply that 100% pairwise coverage equals full combination, requirement, branch, state, or non-functional coverage.

## When to use Pairwise Testing

Pairwise is especially useful when:

- several mostly independent parameters have multiple meaningful levels;
- interaction defects are plausible;
- a legal Cartesian product is too large for available time or resources;
- browser, operating-system, device, locale, database, feature-flag, configuration, payment, or API combinations must be sampled systematically;
- the team needs traceable interaction coverage rather than an ad hoc subset of configurations.

Use it after the primary workflow is stable and individual levels have basic coverage. A failure in a dense combination row is easier to diagnose when baseline behavior and each important level are already understood.

## Limitations and common mistakes

Pairwise alone is insufficient for:

- values or representations that were not modeled as levels;
- malformed, null, missing, blank, unsupported, unreadable, or overflow input;
- numeric, length, date/time, file-size, count, or quota boundaries unless BVA levels were deliberately selected;
- three-way or higher-order interaction defects;
- state transitions and lifecycle paths;
- complex Boolean logic and business rules that require decision tables or condition/cause-effect coverage;
- security, performance, reliability, usability, accessibility, compatibility, and exploratory risks;
- full legal Cartesian coverage and every requirement outcome.

Avoid these mistakes:

1. Calling Each Choice Coverage pairwise coverage.
2. Forming a pair from two levels of the same parameter.
3. Ignoring constraints or generating illegal positive rows.
4. Treating an impossible pair as an ordinary uncovered required pair.
5. Choosing arbitrary values instead of EP classes and BVA representatives.
6. Treating a generator's output as proof that the model is correct.
7. Assuming a fixed row count is always sufficient.
8. Counting repeated row occurrences instead of deduplicated pair IDs.
9. Using a vague oracle or changing the oracle without documenting why.
10. Mixing invalid combinations into positive rows without explicit negative intent.
11. Treating a conditional parameter as independent or using `N/A` without semantic justification.
12. Ignoring higher-order interaction evidence.
13. Hiding generator, constraint, seed, or model metadata.
14. Calling an unbalanced covering array an orthogonal array.
15. Removing a risk-critical baseline, boundary, state, or negative case merely to minimize rows.

## Complementary techniques

Use Pairwise as part of a broader design:

- **Equivalence Partitioning** identifies valid, invalid, and materially different behavioral classes for levels.
- **Boundary Value Analysis** selects below/at/above or adjacent boundary representatives before combining parameters.
- **Decision tables** cover explicit combinations of conditions and resulting business actions.
- **State-transition testing** covers events, states, and lifecycle paths; a state factor in a row does not cover paths by itself.
- **Condition/cause-effect coverage** addresses complex logical relationships.
- **3-way or higher `t`-way testing** addresses evidence of higher-order interactions.
- **Exhaustive testing** is appropriate when the legal domain is small or the risk is safety-critical or financially critical.
- **Error guessing** adds malformed, unusual, coercion, locale, and historically defective cases.
- **Risk-based testing** prioritizes safety, security, compatibility, financial, and high-impact interactions.
- **Security, performance, accessibility, usability, reliability, and exploratory testing** address non-functional and unpredictable risks not represented by Pairwise rows.

A practical sequence is: use EP and BVA to define meaningful levels, formalize constraints, use Pairwise for mostly independent interactions, use decision tables for explicit business rules, state-transition testing for lifecycle behavior, and stronger or risk-based coverage where evidence requires it.

## Verification checklist

Before approving a Pairwise design, confirm:

- [ ] Scope, operation, requirement basis, and exact observable oracle are documented.
- [ ] Every relevant parameter/factor and representation is listed.
- [ ] Every level is meaningful, reproducible, and traceable to the requirement, EP, BVA, or an explicit assumption.
- [ ] Level/Each Choice coverage is reported separately from pairwise coverage.
- [ ] Every pair uses levels from two distinct parameters.
- [ ] Dependencies, allowed combinations, forbidden combinations, and conditional levels are formalized.
- [ ] A required pair has at least one legal complete-row extension.
- [ ] Impossible/excluded pairs are listed with rationale rather than silently counted as uncovered.
- [ ] Positive generated rows are complete and legal.
- [ ] Invalid rows are intentional, separately labeled, linked to violated constraint IDs, and have exact negative oracles.
- [ ] The selected strength (`2-way`, `3-way`, or higher) is explicit and justified.
- [ ] Generator method, tool/version, model, seed, constraints, weights, and optimization are recorded where applicable.
- [ ] Every required legal pair or `t`-tuple is independently inventoried and checked.
- [ ] Coverage arithmetic uses deduplicated required pair/tuple IDs, not row occurrences.
- [ ] Full legal Cartesian coverage is reported separately and only if attempted.
- [ ] Forbidden/invalid-combination coverage is reported separately from legal positive coverage.
- [ ] Each case has preconditions, a complete configuration, executable actions, priority, exact oracle, and traceability.
- [ ] Baseline, EP, BVA, state, decision-table, and high-risk follow-up cases are included where appropriate.
- [ ] Uncovered pairs/tuples, untested constraints, assumptions, Question/TBD items, and residual risks are visible.
- [ ] The design does not claim to prove all values, complete combinations, requirements, states, branches, or higher-order behavior.
- [ ] Markdown headings and tables render correctly; no duplicate headings, placeholders, broken tables, or non-English text remain.

## Summary

Pairwise Testing is a disciplined way to sample interactions among multiple parameters. It requires a correct factor/level model, explicit constraints, legal complete rows, an exact oracle, independent coverage verification, and honest reporting of what remains untested. Use EP and BVA to make levels meaningful, Pairwise to cover mostly independent interactions, and decision tables, state-transition, stronger `t`-way, exhaustive, or risk-based techniques when the risk is not adequately represented by 2-way coverage.
