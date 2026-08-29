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

When several inputs each have their own partition set, covering every partition at least once is commonly called **Each Choice Coverage**. It does not mean that every combination of input partitions has been tested; use decision tables or pairwise testing for that risk.

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

| Test case ID | Requirement / input | Partition ID and description | Preconditions | Input value | Steps | Expected result / oracle | Priority | Covered partitions | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| EP-001 | Admission percentage | P2: valid range `[50,90]` | Admission form is open; candidate data is available | `70` | Enter `70` and submit the form | The value is accepted; no range-validation error is shown | High | P2 | Confirm endpoints separately with BVA |
|  |  |  |  |  |  |  |  |  |  |

A simple coverage review can use a matrix with one row per partition and one column per test case. Every partition should have at least one marked test. If a test covers several independent fields, record each field/partition pair rather than marking only the test-case ID.

## Worked examples

### Example 1: College admission percentage

**Requirement assumption:** The percentage field accepts whole-number values from `50%` through `90%`, inclusive. Values outside that interval are rejected. Non-numeric and blank values are also rejected, with different validation behavior. If the actual requirement does not distinguish these errors, combine the corresponding invalid classes.

| Partition | Definition | Representative | Expected result |
| --- | --- | --- | --- |
| P1: below range | Numeric percentage `<50` | `49` | Reject and show the below-minimum validation error. |
| P2: valid range | Numeric percentage `[50,90]` | `70` | Accept the value and continue admission processing. |
| P3: above range | Numeric percentage `>90` | `91` | Reject and show the above-maximum validation error. |
| P4: malformed | A value that cannot be parsed as a percentage | `abc` | Reject and show the format validation error. |
| P5: blank/missing | No value or an empty value | blank | Reject and show the required-field error. |

The representatives test the classes, but they do not replace boundary tests. Add `49`, `50`, `90`, and `91` as boundary-focused tests when applying Boundary Value Analysis (BVA).

### Example 2: Numeric field from 1 through 1000

**Requirement assumption:** The field accepts integer values in `[1,1000]`. It rejects values below `1`, values above `1000`, and input that cannot be parsed as an integer.

| Partition | Representative | Expected result |
| --- | --- | --- |
| P1: below minimum (`x < 1`) | `-37` | Reject as out of range. |
| P2: valid range (`1 <= x <= 1000`) | `46` | Accept and process the value. |
| P3: above maximum (`x > 1000`) | `1773` | Reject as out of range. |
| P4: malformed or non-numeric | `Name` | Reject as invalid numeric input. |

Do not automatically put negative numbers, letters, symbols, and decimal values into one class. Make them one partition only if the requirement and observed behavior treat them identically. If decimals are possible, add classes for accepted precision and unsupported precision.

### Example 3: Airline baggage pricing

**Requirement assumption:** Let `t` be the number of hours remaining before scheduled departure. Payment more than 24 hours before departure receives a 50% discount. Payment from 24 hours up to, but not including, 3 hours before departure uses the basic tariff. Payment during the final three hours receives a 20% surcharge. Payment at or after departure is prohibited, and malformed time input is invalid.

The interval definitions are:

- `t > 24`: 50% discount
- `3 < t <= 24`: basic tariff
- `0 < t <= 3`: 20% surcharge
- `t <= 0`: invalid because departure has arrived or passed
- Malformed or missing time: validation error

These classes are mutually exclusive and cover every numeric value of `t`; the malformed/missing class covers non-numeric or absent representations.

| Partition | Representative | Expected result |
| --- | --- | --- |
| P1: early payment (`t > 24`) | `30 hours` | Apply the 50% discount. |
| P2: basic period (`3 < t <= 24`) | `10 hours` | Apply the basic tariff. |
| P3: final three hours (`0 < t <= 3`) | `2 hours` | Apply the 20% surcharge. |
| P4: departure reached or passed (`t <= 0`) | `0 hours` | Reject payment because departure has arrived or passed. |
| P5: malformed/missing | `two hours` | Reject and show a time-format or required-field error. |

EP identifies these behavioral classes. BVA should additionally check values immediately below, at, and immediately above the `24`-hour and `3`-hour boundaries, while respecting the ownership of `24` and `3` in the requirement.

### Example 4: Date of birth

**Requirement assumption:** The service accepts dates in `YYYY-MM-DD` format from `1900-01-01` through a fixed reference date, `2026-08-29`, inclusive. A date after the reference date is rejected. Impossible dates, malformed formats, and blank values have separate validation behavior.

| Partition | Definition | Representative | Expected result |
| --- | --- | --- | --- |
| P1: before supported history | Valid calendar date before `1900-01-01` | `1899-12-31` | Reject as outside the supported date range. |
| P2: valid past date | Valid date in `[1900-01-01, 2026-08-29]` | `1990-05-20` | Accept and calculate/store the date of birth. |
| P3: future date | Valid calendar date after `2026-08-29` | `2026-08-30` | Reject as a future date. |
| P4: impossible date | A date that cannot exist in the calendar | `2026-02-30` | Reject as an invalid calendar date. |
| P5: malformed format | A value that is not `YYYY-MM-DD` | `05/20/1990` | Reject as a format error. |
| P6: blank/missing | No date supplied | blank | Reject as a required field. |

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
