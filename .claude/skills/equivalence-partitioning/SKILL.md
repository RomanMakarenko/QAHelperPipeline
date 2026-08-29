---
name: equivalence-partitioning
description: Apply ISTQB-aligned Equivalence Partitioning to a supplied requirement or test case. Trigger when the user asks to identify equivalence classes, choose representative values, or prepare structured functional test cases, including multi-input rules. Output traceable partitions, executable cases, coverage, assumptions, and complementary-technique follow-ups.
version: 0.1.0
---

# Equivalence Partitioning Test Design

Apply this skill when the user provides a requirement, acceptance criterion, existing test case, input field, API parameter, file-upload rule, or business rule and asks to use **Equivalence Partitioning (EP)** to prepare test cases.

Use this project's detailed technique guide as the primary terminology and workflow reference:

`docs/TestDesignAndSoftwareTestingTechniques/BlackBox/equivalencePartitioning.md`

If the guide is unavailable, follow the rules in this skill and ISTQB CTFL terminology from section 4.2.1. The related learning objective is FL-4.2.1 (K3): apply equivalence partitioning to derive test cases.

## Scope and core principle

Treat EP as a black-box, specification-based functional test design technique. An equivalence partition/class is a portion of an input or output domain for which behavior is expected to be equivalent according to the specification.

Design test cases to execute representative values from the identified partitions. In principle, exercise every partition at least once. For multiple independent inputs, use **Each Choice Coverage**: cover every input/partition pair at least once. Do not describe this as full combination coverage.

EP reduces redundant tests; it does not prove that every value in a partition is defect-free. It does not replace Boundary Value Analysis (BVA), decision tables, pairwise testing, state-transition testing, security testing, performance testing, accessibility testing, compatibility testing, or exploratory testing.

## Input contract

Accept any of the following:

- A complete requirement or acceptance criterion
- An existing test case that needs EP review or expansion
- A single input field or API parameter
- A multi-field form, endpoint, batch rule, or file-upload requirement
- A business rule with roles, state, dates, or other context

Extract, when available:

- System action or operation under test
- Inputs and their semantic meaning
- Representations and data types
- Ranges, sets, enumerations, formats, lengths, precision, and flags
- Valid and invalid behavior
- Observable oracles: status codes, messages, calculations, persisted values, processing paths, or state changes
- Preconditions, setup data, user role, account state, existing records, locale, time zone, and reference date
- Dependencies among fields and conditions
- Requirement ID, severity, and priority

An existing test case is input evidence, not proof that its values or coverage are sufficient. Analyze its requirement and identify missing partitions.

## Clarifications and assumptions

Ask only questions that block safe partitioning or make the expected oracle unknowable. Prioritize:

- Inclusive versus exclusive endpoints
- Integer/decimal precision and numeric representation
- Date format, reference date, time zone, and time precision
- Null, missing, empty, and whitespace-only behavior
- Malformed, unsupported, unreadable, or oversized input behavior
- State-, role-, or context-dependent behavior
- Exact expected status, message, calculation, persistence, or state transition

If clarification is not essential, proceed and label information explicitly as one of:

- **Confirmed** — stated in the supplied requirement
- **Assumption** — introduced to make a provisional model possible
- **Question/TBD** — unresolved and requiring confirmation
- **Residual risk** — a meaningful area not covered by the generated cases

Never present an invented behavior as a confirmed requirement. If a blocking ambiguity remains, provide a clarification section and only provisional cases that do not depend on the unknown behavior.

## Partition-modeling procedure

Follow the application workflow documented in this project's Equivalence Partitioning guide:

1. **Extract observable outcomes.** Identify what acceptance, rejection, processing, calculation, message, response, persistence, or state change means for the feature.
2. **List inputs and dependencies.** Include input fields, representations, context, existing data, roles, states, and time-related conditions.
3. **Define each domain.** State the relevant possible values, including malformed, missing, null, empty, whitespace-only, unsupported, or unreadable representations when they can reach the system.
4. **Formalize rules and endpoint ownership.** Use notation such as `[1,1000]`, `(1,1000]`, `x < 1`, or `x > 1000`. State precision, format, locale, time zone, and reference-date assumptions.
5. **Identify behavioral partitions.** Separate valid and invalid partitions. Split an invalid class only when its expected message, status, processing path, or other observable behavior differs.
6. **Review the model.** Every partition must be non-empty, mutually exclusive, collectively exhaustive for the declared domain, traceable to the requirement, and behaviorally coherent.
7. **Select representatives.** Choose at least one clear, typical, reproducible representative from every partition. Use exact values for singleton classes, practical values for unbounded classes, and minimal reproducible malformed values for format-error classes.
8. **Generate executable cases.** Define preconditions, complete input data, steps, exact expected result/oracle, priority, and partition traceability.
9. **Review coverage and risk.** Map every partition to one or more cases, calculate coverage, identify uncovered combinations and assumptions, and recommend complementary techniques.

Do not partition by data type alone. Values with the same type can have different formats, lengths, permissions, business meaning, or processing paths. Conversely, values with different representations may share a partition only when the specified behavior is equivalent.

## Representative-value rules

For every partition:

- Select at least one value that actually belongs to the formal definition.
- Prefer an interior value for a range when the purpose is EP rather than BVA.
- Choose values that are understandable, reproducible, and relevant to product risk.
- Use the exact value for an exact-match or singleton partition.
- For an unbounded class, choose a realistic value and document why it represents the class.
- Add extra representatives for high-risk, high-impact, historically defective, or format-sensitive areas.
- Mark endpoint and neighboring values as BVA follow-up unless they are intentionally included for another reason.

Never claim that one passing representative proves all values in the partition work.

## Multi-input policy

Model each input's partitions independently before designing combinations.

- Classify inputs as independent, dependent, or state/context-driven.
- When testing one target partition, use valid baseline values for non-target inputs unless the requirement says otherwise.
- Record every input/partition pair covered by a case.
- Default to Each Choice Coverage, not an uncontrolled Cartesian product.
- State which combinations remain untested.
- Use decision tables for interacting conditions and actions.
- Use pairwise testing for broad interaction coverage among mostly independent parameters.
- Use state-transition testing for lifecycle and context behavior.
- Add risk-based combinations for high-impact rules.

## Default output format

Preserve a format requested by the user (Gherkin, JSON, CSV, test-management table, or another schema). If no format is requested, output Markdown with these sections:

```markdown
## Scope and oracle

## Clarifications, assumptions, and questions

## Input model

## Partition model

## Prepared test cases

## Coverage

## Complementary coverage

## Manual verification
```

### Scope and oracle

Summarize the feature, operation, requirement basis, and observable outcomes. State what the generated tests will verify and what EP will not cover.

### Clarifications, assumptions, and questions

List confirmed facts, assumptions, unresolved questions, and residual risks. Include endpoint ownership, precision, formats, null/blank behavior, date/time settings, state, role, locale, and oracle details when relevant.

### Input model

Use a table with at least:

| Input ID | Meaning | Representation/type | Declared domain | Dependencies/context | Expected outcomes | Requirement reference |
| --- | --- | --- | --- | --- | --- | --- |

Use stable input IDs such as `PERCENTAGE`, `EMAIL`, or `DEPARTURE_TIME`.

### Partition model

Use stable partition IDs scoped to an input, such as `PERCENTAGE-P1` or `EMAIL-P3`:

| Partition ID | Input ID | Formal definition | Valid/invalid | Expected behavior/oracle | Representative value | Representative rationale | Status |
| --- | --- | --- | --- | --- | --- | --- | --- |

`Status` must identify whether the row is confirmed, assumed, or TBD. Ensure the formal definitions are disjoint and complete for the declared domain.

### Prepared test cases

Generate executable cases rather than only explaining the technique:

| Test case ID | Title | Requirement reference | Priority | Preconditions/setup | Complete input data | Steps | Expected result/oracle | Covered partitions | Technique tags | Assumptions/notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |

Use IDs such as `EP-001`. Every case must have a precise expected result: status code, validation message, accepted/rejected state, calculation, persistence, processing path, or state transition. Do not use “the system works correctly” as an oracle.

When the user requests Gherkin, keep partition IDs and expected behavior in scenario tags or comments so traceability is not lost.

### Coverage

Provide a partition-to-test-case matrix or equivalent mapping:

| Partition ID | Covered by test cases | Exercised? | Notes/risk |
| --- | --- | --- | --- |

For a single input, report:

`exercised partitions / identified partitions × 100%`

For multiple inputs, calculate coverage using unique input/partition pairs. State the count and percentage, and explicitly say whether this is Each Choice Coverage. Never imply that every combination was tested unless combination coverage was actually designed and executed.

### Complementary coverage

Recommend follow-up techniques based on observed risk:

- **BVA:** just below, at, and just above numeric, length, date, and time boundaries.
- **Decision tables:** combinations of conditions and resulting actions.
- **Pairwise testing:** interactions among mostly independent inputs.
- **State-transition testing:** behavior across states and events.
- **Condition/cause-effect coverage:** complex logical relationships.
- **Error guessing/risk-based testing:** domain-specific, historical, security, and likely-user-error cases.

### Manual verification

Before presenting the result, verify:

- Requirement and oracle are traceable.
- All relevant inputs, representations, dependencies, and context are listed.
- Valid and materially different invalid partitions are modeled.
- Every partition is non-empty, mutually exclusive, and collectively exhaustive for the declared domain.
- Endpoint ownership and precision are explicit.
- Null, missing, empty, whitespace, malformed, unsupported, and unreadable values are considered where applicable.
- Date, locale, time zone, and reference-date assumptions are documented.
- Every representative belongs to its stated partition.
- Preconditions, steps, complete input data, and exact expected results are executable.
- Every partition/input pair is mapped to test cases.
- Coverage arithmetic is correct.
- BVA and combination/state/risk follow-ups are identified.
- Unverified assumptions and residual risks are visible.

If execution or product feedback shows that two supposedly equivalent values behave differently, revise the partition model and create separate partitions or document the defect.

## Compact modeling examples

Use examples only to clarify the method; replace their requirements with the user's actual rules.

### Numeric range

For an integer field accepting `[1,1000]`, model:

- `NUMBER-P1`: `x < 1`, invalid, representative `-37`
- `NUMBER-P2`: `1 <= x <= 1000`, valid, representative `46`
- `NUMBER-P3`: `x > 1000`, invalid, representative `1773`
- `NUMBER-P4`: malformed/non-numeric representation, invalid, representative `Name`

Do not combine numeric negatives, letters, symbols, and decimals unless the requirement gives them equivalent behavior. Add BVA for `0`, `1`, `2`, `999`, `1000`, and `1001` when appropriate.

### Time-based business rule

If `t` is hours before departure and the requirement defines:

- `t > 24`: 50% discount
- `3 < t <= 24`: basic tariff
- `0 < t <= 3`: 20% surcharge
- `t <= 0`: reject because departure has arrived or passed
- malformed/missing: validation error

Select representatives such as `30`, `10`, `2`, `0`, and `two hours`. Verify that `24` belongs to the basic class and `3` belongs to the surcharge class. Add BVA around both boundaries.

Do not copy these rules into a user's product specification without confirmation. Label them as assumptions if used provisionally.
