# Equivalence Partitioning

## Purpose

Equivalence Partitioning (EP) is a black-box, specification-based test design technique. It helps a QA engineer reduce redundant test cases while still checking the important categories of input defined by the requirements.

This guide explains how to identify equivalence partitions, select representative values, document test cases, and recognize when another technique is needed. The examples use common UI and business-rule requirements, but the same process applies to APIs, batch jobs, and service integrations.

### ISTQB alignment

The guide follows the terminology and basic coverage principle from the **ISTQB Certified Tester Foundation Level (CTFL) syllabus, section 4.2.1, Equivalence Partitioning**. The related learning objective is **FL-4.2.1 (K3): apply equivalence partitioning to derive test cases**. The sections below add practical documentation and risk-based guidance; they do not replace the project requirements or the official syllabus.

## What is Equivalence Partitioning?

An **equivalence partition** (also called an **equivalence class**) is a portion of an input or output domain for which the system's behavior is expected to be the same, based on the specification. In practical test design, values in one partition should be handled according to the same rule and have the same relevant behavioral outcome. The exact output value may legitimately depend on the input; equivalence concerns the behavior being tested, such as acceptance, rejection, validation handling, or pricing rule.

Partitions can be continuous or non-continuous, and may be finite or infinite. For example, if a percentage field accepts every whole-number value from 50 through 90 inclusive, all values in that interval may form one valid partition. A tester can select one representative value, such as `70`, instead of testing every integer in the interval.

EP is based on the following principle:

> Design test cases to execute representative values from the equivalence partitions; in principle, cover each partition at least once.

In practice:

1. Identify the relevant domain of possible values.
2. Divide the domain into non-empty partitions with equivalent expected behavior.
3. Select at least one representative value from each partition.
4. Create test cases with explicit preconditions and expected results.
5. Record which partitions are covered and which assumptions remain unverified.

EP reduces the number of tests; it does **not** prove that every value in a partition works. A defect can still affect only part of a partition, and EP alone does not specifically target boundary defects or combinations of conditions.

## Important terminology

| Term | Meaning |
| --- | --- |
| **Input domain** | The relevant set of values or representations that can reach the system, such as numbers, strings, dates, files, or null values. |
| **Valid partition** | Values that satisfy the requirement and should be accepted or processed normally. |
| **Invalid partition** | Values that violate a rule and should be rejected, ignored, or handled by a defined error path. |
| **Representative value** | A concrete value selected to represent one partition in a test case. |
| **Oracle** | The expected observable result used to decide whether the test passed, such as a response code, validation message, stored value, or calculated price. |
| **Partition coverage** | The percentage of modeled partitions exercised by the test suite. A basic target is to exercise every partition at least once. |

For one input, partition coverage can be calculated as:

`exercised partitions / identified partitions × 100%`

When several inputs each have their own partition set, **Each Choice Coverage** means exercising every input/partition pair at least once. It does not mean that every combination of input partitions has been tested; use decision tables, pairwise testing, or exhaustive testing for that risk.

Partitions are derived from the requirement, not only from the data type. Two values that look similar may belong to different partitions if they cause different messages, response codes, processing paths, permissions, or state changes.

## Rules for sound partitions

A good partition model follows these rules:

1. **Partitions are non-empty.** Every partition has at least one possible value in the stated domain.
2. **Partitions are mutually exclusive.** A value belongs to one partition only. Use interval notation or explicit precedence rules when endpoints could overlap.
3. **Partitions are collectively exhaustive for the stated domain.** Every relevant input is accounted for, including malformed, blank, or null values where those can be supplied.
4. **Expected behavior is equivalent within a partition.** Do not group values together merely because they have the same data type.
5. **Invalid values are split when behavior differs.** For example, a blank required field may produce a required-field message, while `abc` may produce a format message. They should be separate partitions if the distinction is observable.
6. **Assumptions are written down.** State whether endpoints are inclusive, what numeric precision is allowed, how whitespace is treated, which date format and time zone apply, and what reference date is used.

When requirements are incomplete, document the assumption as a question or risk instead of silently inventing behavior.

## Common types of partitions

| Input or rule type | Possible partitions | Example and caution |
| --- | --- | --- |
| Numeric range | Below range, allowed range, above range | For `1`–`1000` inclusive: `<1`, `[1,1000]`, `>1000`. Define whether decimals are allowed. |
| Exact value | Required value, all other values | If the rule requires quantity `1`, `1` is valid and other numeric values are invalid. |
| Set or enumeration | Each value or group with the same behavior, values outside the set | If `Basic`, `Premium`, and `Enterprise` have the same behavior, they can share a partition; separate them when their prices or outcomes differ. |
| Boolean or flag | `true`, `false`, and invalid representations if the interface permits them | A JSON boolean and the string `"true"` may not be equivalent. |
| String length | Below minimum, allowed length, above maximum | For 8–20 characters inclusive: `<8`, `[8,20]`, `>20`. |
| String format | Correct format, malformed format, disallowed characters | Email, phone, identifier, and date formats usually need negative classes. |
| Null, empty, and whitespace | Null/missing, empty string, whitespace-only, non-empty value | Keep them separate only when validation or processing differs. |
| Date or time | Valid interval, before interval, after interval, impossible date, invalid format | Define time zone, precision, reference date, and whether the endpoint is included. |
| File type or size | Supported type and size, unsupported type, too small/large, unreadable file | File extension and actual content type may produce different behavior. |
| State or context | Inputs valid in one state/context and invalid or differently processed in another | Combine EP with state-transition or decision-table testing when outcomes depend on context. |

## Step-by-step application procedure

### 1. Read the requirement and identify observable outcomes

Extract acceptance criteria, validation rules, business rules, response codes, messages, and state changes. Highlight words such as `between`, `at least`, `before`, `only`, `required`, and `one of`, because they determine partition boundaries and ownership.

### 2. List the inputs and relevant dependencies

Record every input that can affect the result:

- Form fields and API parameters
- Data type, format, length, and precision
- Null, missing, empty, and whitespace representations
- User role, account status, or other context
- Existing records and system state
- Dates, times, locale, and time zone
- Uploaded files and their metadata

Do not assume that each field can be tested independently. Note dependencies for later decision-table, pairwise, or state-based testing.

### 3. Define the domain and the rules

For each input, write the complete relevant domain and make endpoint ownership explicit. For example:

- Accepted integer values: `[1,1000]`
- Accepted percentage values: `[50,90]`
- Accepted departure time: `t > 0`, measured in hours before departure; pricing classes may further divide this range
- Accepted date format: `YYYY-MM-DD`

Also define what happens to input that is missing, malformed, or outside the business range.

### 4. Identify equivalence partitions

Create a partition for each group expected to have the same processing and oracle. Start with valid and invalid classes, then split a class when the requirement specifies a different result.

For example, “invalid input” might be one partition if every invalid value gets the same error. If numeric overflow, malformed syntax, and missing input produce different errors, model them separately.

### 5. Review the partition model

Check that the partitions are:

- Non-empty
- Mutually exclusive
- Collectively exhaustive for the declared domain
- Consistent with the requirement
- Explicit about endpoint inclusivity and data representation

Ask another tester or product specialist to review ambiguous rules before turning them into test cases.

### 6. Select representative values

Choose a value clearly inside each partition, not a value that accidentally belongs to another class. A useful representative is:

- Valid according to the stated partition
- Easy to understand and reproduce
- Typical of the data users are expected to provide
- Relevant to the risk of the feature

Use additional values when risk justifies them. One passing representative does not guarantee that all values in the same partition are defect-free.

### 7. Define the test case and oracle

For each representative, specify:

- Preconditions and test data setup
- Steps to submit or process the value
- Input value and its partition
- Expected response, message, stored result, calculation, or state change
- Priority and requirement reference

An expected result such as “the system works correctly” is not an adequate oracle. State exactly what should be observed.

### 8. Execute and record coverage

Run the cases, record the actual result, and mark the covered partition. If a partition cannot be tested, record the reason and the resulting risk. Update the partition model when execution reveals that two supposedly equivalent values behave differently.

## Reusable test-case worksheet

Use the following template as a starting point. Adapt the columns to the test-management tool used by the project.

| Test case ID | Requirement reference | Input ID | Partition ID and formal definition | Priority | Preconditions/setup | Complete input/context | Steps/actions | Exact expected result/oracle | Covered input/partition pairs | Technique tags | Assumptions/notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `EP-001` | `REQ-1` | `PERCENTAGE` | `PERCENTAGE-P2`: valid `[50,90]` | High | Admission form is open; candidate data is available | `percentage=70`; all other fields valid and nominal | Enter `70`; submit the form | HTTP `200`; value is accepted; no range-validation error; admission processing continues | `PERCENTAGE/PERCENTAGE-P2` | EP | Confirm endpoints separately with BVA; replace assumptions with the product contract |

A simple coverage review can use a matrix with one row per partition and one column per test case. Every partition should have at least one marked test. If a test covers several independent fields, record each field/partition pair rather than marking only the test-case ID.

## Worked examples

### Example 1: College admission percentage

**Requirement assumption `REQ-EP-01` (Assumption):** The percentage field accepts whole-number values from `50%` through `90%`, inclusive. Values outside that interval are rejected. Non-numeric and blank values are also rejected with different validation behavior. The cases below use HTTP `200` for accepted input and HTTP `400` for each rejection; replace these assumed statuses and messages with the actual contract before execution.

| Partition ID | Input ID | Formal definition | Representative | Exact expected result/oracle | Status |
| --- | --- | --- | --- | --- | --- |
| `PERCENTAGE-P1` | `PERCENTAGE` | Numeric percentage `<50` | `49` | HTTP `400`; below-minimum validation error; admission processing does not start | Assumption |
| `PERCENTAGE-P2` | `PERCENTAGE` | Numeric percentage `[50,90]` | `70` | HTTP `200`; value is accepted and admission processing continues | Assumption |
| `PERCENTAGE-P3` | `PERCENTAGE` | Numeric percentage `>90` | `91` | HTTP `400`; above-maximum validation error; admission processing does not start | Assumption |
| `PERCENTAGE-P4` | `PERCENTAGE` | Value cannot be parsed as a percentage | `abc` | HTTP `400`; format validation error; admission processing does not start | Assumption |
| `PERCENTAGE-P5` | `PERCENTAGE` | Missing or empty value | blank | HTTP `400`; required-field error; admission processing does not start | Assumption |

The representatives test the classes, but they do not replace boundary tests. Add `49`, `50`, `90`, and `91` as boundary-focused tests when applying Boundary Value Analysis (BVA).

### Example 2: Numeric field from 1 through 1000

**Requirement assumption `REQ-EP-02` (Assumption):** The field accepts integer values in `[1,1000]`. It rejects values below `1`, values above `1000`, and input that cannot be parsed as an integer. Accepted input returns HTTP `200` and persists the submitted integer; rejected input returns HTTP `400` and does not persist it.

| Partition ID | Input ID | Formal definition | Representative | Exact expected result/oracle | Status |
| --- | --- | --- | --- | --- | --- |
| `QUANTITY-P1` | `QUANTITY` | Numeric integer `x < 1` | `-37` | HTTP `400`; below-minimum error; value is not persisted | Assumption |
| `QUANTITY-P2` | `QUANTITY` | Numeric integer `1 <= x <= 1000` | `46` | HTTP `200`; stored quantity equals integer `46` | Assumption |
| `QUANTITY-P3` | `QUANTITY` | Numeric integer `x > 1000` | `1773` | HTTP `400`; above-maximum error; value is not persisted | Assumption |
| `QUANTITY-P4` | `QUANTITY` | Malformed or non-numeric representation | `Name` | HTTP `400`; invalid-numeric error; value is not persisted | Assumption |

Do not automatically put negative numbers, letters, symbols, and decimal values into one class. Make them one partition only if the requirement and observed behavior treat them identically. If decimals are possible, add classes for accepted precision and unsupported precision.

### Example 3: Airline baggage pricing

**Requirement assumption `REQ-EP-03`:** Let `t` be the number of hours remaining before scheduled departure. Payment more than 24 hours before departure receives a 50% discount. Payment from 24 hours up to, but not including, 3 hours before departure uses the basic tariff. Payment during the final three hours receives a 20% surcharge. Payment at or after departure is prohibited, and malformed time input is invalid. For the fixture, the basic tariff is `USD 100.00`, the discount is `USD 50.00`, and the surcharge is `USD 120.00`; accepted payment returns HTTP `200` and rejected payment returns HTTP `400`.

The interval definitions are:

- `t > 24`: 50% discount
- `3 < t <= 24`: basic tariff
- `0 < t <= 3`: 20% surcharge
- `t <= 0`: invalid because departure has arrived or passed
- Malformed or missing time: validation error

These classes are mutually exclusive and cover every numeric value of `t`; the malformed/missing class covers non-numeric or absent representations.

| Partition | Representative | Expected result |
| --- | --- | --- |
| P1: early payment (`t > 24`) | `30 hours` | HTTP `200`; payment succeeds and final price is `USD 50.00`. |
| P2: basic period (`3 < t <= 24`) | `10 hours` | HTTP `200`; payment succeeds and final price is `USD 100.00`. |
| P3: final three hours (`0 < t <= 3`) | `2 hours` | HTTP `200`; payment succeeds and final price is `USD 120.00`. |
| P4: departure reached or passed (`t <= 0`) | `0 hours` | HTTP `400`; reject payment with `Payment is unavailable after departure`; no payment is captured. |
| P5: missing time | blank | HTTP `400`; return `TIME_REQUIRED`; no payment is captured. |
| P6: malformed time | `two hours` | HTTP `400`; return `TIME_INVALID`; no payment is captured. |

EP identifies these behavioral classes. BVA should additionally check values immediately below, at, and immediately above the `24`-hour and `3`-hour boundaries, while respecting the ownership of `24` and `3` in the requirement.

### Example 4: Date of birth

**Requirement assumption `REQ-EP-04` (Assumption):** The service accepts dates in `YYYY-MM-DD` format from `1900-01-01` through a fixed reference date, `2026-08-29`, inclusive. A date after the reference date is rejected. Impossible dates, malformed formats, and blank values have separate validation behavior. Accepted input returns HTTP `200` and persists the normalized date; every rejected input returns HTTP `400` and does not persist a date.

| Partition | Definition | Representative | Expected result |
| --- | --- | --- | --- |
| P1: before supported history | Valid calendar date before `1900-01-01` | `1899-12-31` | HTTP `400`; return `DATE_OUT_OF_RANGE`; no date is persisted. |
| P2: valid past date | Valid date in `[1900-01-01, 2026-08-29]` | `1990-05-20` | HTTP `200`; return and persist normalized date `1990-05-20`. |
| P3: future date | Valid calendar date after `2026-08-29` | `2026-08-30` | HTTP `400`; return `DATE_IN_FUTURE`; no date is persisted. |
| P4: impossible date | A date that cannot exist in the calendar | `2026-02-30` | HTTP `400`; return `DATE_INVALID`; no date is persisted. |
| P5: malformed format | A value that is not `YYYY-MM-DD` | `05/20/1990` | HTTP `400`; return `DATE_FORMAT_INVALID`; no date is persisted. |
| P6: blank/missing | No date supplied | blank | HTTP `400`; return `DATE_REQUIRED`; no date is persisted. |

If the product displays different behavior for age groups, such as child, adult, and senior eligibility, those groups can become additional partitions only after fixing the reference date and defining the age-calculation rule. The date-of-birth input and the calculated age are not automatically the same equivalence domain.

## When to use Equivalence Partitioning

EP is especially useful when:

- A field or API parameter accepts a large range of possible values.
- A requirement defines valid and invalid input conditions.
- Many values are expected to produce the same observable result.
- You need efficient functional coverage for forms, APIs, imports, or batch processing.
- You want a traceable way to design positive and negative tests from requirements.
- The team needs to reduce repetitive tests without ignoring a meaningful category of behavior.

## When EP alone is insufficient

Use EP as one technique in a broader test strategy. EP alone is not sufficient for:

- Defects concentrated at boundaries or transitions between classes
- Combinations of several conditions or input fields
- Highly interdependent inputs
- State-dependent workflows and lifecycle transitions
- Security properties, such as authorization, injection resistance, or abuse handling
- Performance, reliability, usability, accessibility, or compatibility risks
- Exploratory testing and unexpected product behavior
- Values within one apparent partition that have special business meaning

## Combining EP with other techniques

| Complementary technique | How it extends EP |
| --- | --- |
| **Boundary Value Analysis (BVA)** | Tests values just below, at, and just above each boundary. Use it for numeric, length, date, and time partitions. |
| **Decision tables** | Tests combinations of conditions and confirms the correct action for each rule combination. |
| **Pairwise testing** | Covers interactions among multiple mostly independent parameters without testing every possible combination. |
| **State-transition testing** | Checks whether the same input is accepted or rejected correctly in different states and after different events. |
| **Condition or cause-effect coverage** | Exercises complex logical relationships that cannot be represented by one independent partition. |
| **Error guessing and risk-based testing** | Adds values suggested by product history, implementation risk, domain knowledge, or likely user mistakes. |

A practical sequence is to use EP to identify the broad behavioral classes, BVA to strengthen each boundary, and decision tables, pairwise, or state-transition testing for interactions and context.

## Benefits and limitations

### Benefits

- Reduces redundant test cases for large input domains
- Encourages systematic positive and negative test design
- Makes assumptions and expected behavior visible
- Helps trace tests to requirements and partition coverage
- Works with numeric, textual, date, file, API, and business-rule inputs
- Provides a useful foundation for BVA and combination techniques

### Limitations

- Depends on accurate and sufficiently detailed requirements
- Can miss a defect that affects only part of a partition
- Does not inherently test boundaries or combinations
- Can produce misleading coverage when partitions are modeled incorrectly
- Requires extra analysis for dependencies, state, locale, precision, and time zones
- A passing representative is evidence about one selected value, not proof of the entire class

## Common mistakes

Avoid these mistakes when applying EP:

1. **Partitioning by data type only.** Two strings can have different formats, lengths, permissions, or business meanings.
2. **Using intuition instead of the requirement.** Derive the classes from defined behavior and document assumptions.
3. **Overlapping partitions.** For example, defining both `x >= 50` and `x <= 90` without specifying the complete interval and endpoint ownership.
4. **Leaving gaps.** Check what happens to values between listed ranges and to malformed or missing representations.
5. **Creating empty or impossible classes.** Every class should contain at least one value in the declared domain.
6. **Lumping all invalid values together.** Separate them when the system returns different errors or follows different paths.
7. **Choosing a representative outside its partition.** Verify the value against the formal definition before executing the case.
8. **Ignoring representation details.** Consider decimal precision, leading zeros, whitespace, encoding, locale, time zone, and null versus empty input.
9. **Failing to define an oracle.** Specify the exact response, message, calculation, persistence, or state change expected.
10. **Treating EP as a replacement for BVA or combination testing.** EP models classes; complementary techniques address other risks.
11. **Assuming one successful test proves the class.** Add risk-based values or additional techniques when the impact of a missed defect is high.

## Practical checklist

Before executing an EP-based test set, confirm:

- [ ] The requirement and all relevant observable outcomes are understood.
- [ ] Every input, format, context, and dependency is listed.
- [ ] Valid and materially different invalid partitions are identified.
- [ ] Each partition is non-empty, mutually exclusive, and exhaustive for the stated domain.
- [ ] Endpoint inclusivity and precision are explicit.
- [ ] Null, missing, empty, whitespace, and malformed values are considered.
- [ ] Date, time zone, locale, and reference-date assumptions are documented.
- [ ] Each partition has at least one representative value.
- [ ] Every representative belongs to the partition it claims to cover.
- [ ] Preconditions, steps, and a specific oracle are recorded.
- [ ] Boundaries and combinations are covered by BVA or another suitable technique.
- [ ] Untested partitions and residual risks are documented.

## Summary

Equivalence Partitioning is a disciplined way to divide a requirement's input domain into groups with equivalent expected behavior and to test a representative from each group. Start with the requirement, define clear and disjoint partitions, include distinct invalid behavior, select reproducible representatives, and record explicit expected results. Use BVA, decision tables, pairwise, state-transition, and risk-based testing to cover boundaries, interactions, workflows, and risks that EP does not address by itself.
