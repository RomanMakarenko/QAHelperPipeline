# Boundary Value Analysis (BVA)

## Purpose

Boundary Value Analysis (BVA) is a black-box, specification-based test design technique that derives test values at and around the edges of equivalence partitions. A boundary is a point where the expected behavior changes, such as the minimum accepted value, the maximum accepted value, or a transition from one tariff to another.

Defects are often concentrated at these edges because of incorrect comparison operators (`<` instead of `<=`), off-by-one calculations, rounding, truncation, unit conversion, or an incorrect predecessor/successor. BVA gives those risks deliberate coverage. It does not inspect the implementation and does not replace testing of the values and combinations that are not near a boundary.

This guide is a practical supplement to the terminology and principles of the **ISTQB Certified Tester Foundation Level (CTFL)** syllabus. The supplied task does not name a particular CTFL syllabus version or a separate BVA section, so no version-specific section number is asserted here. Use the current official syllabus for formal exam or organizational terminology.

## Relationship between EP and BVA

BVA and Equivalence Partitioning (EP) are related but different techniques:

- **EP identifies behavioral partitions**: groups of input or output values for which equivalent behavior is expected according to the specification.
- **BVA identifies the edges** of those partitions and the transition between adjacent partitions.
- **BVA selects values at or near an edge**, using the smallest meaningful increment for the domain.
- **EP and BVA are commonly combined**, but BVA does not replace EP and EP does not automatically test boundaries.

For example, EP can identify the valid integer partition `[1,1000]`, the below-range partition `x < 1`, and the above-range partition `x > 1000`. Three-value BVA then focuses on `0, 1, 2` and `999, 1000, 1001`. A representative such as `46` is useful for EP but is not a lower- or upper-boundary test.

## Core terminology

| Term | Meaning |
| --- | --- |
| **Equivalence partition/class** | A portion of an input or output domain for which behavior is expected to be equivalent according to the specification. |
| **Boundary/edge** | The limit or transition between adjacent partitions, or an outer limit of the modeled domain. |
| **Boundary value** | The value at the edge whose endpoint ownership is defined by the requirement. |
| **Value just below** | The nearest representable value on the lower side of a boundary, using the declared precision or increment. |
| **Value just above** | The nearest representable value on the higher side of a boundary, using the declared precision or increment. |
| **Lower boundary** | The smallest value accepted by a range, or the transition at its lower edge. |
| **Upper boundary** | The largest value accepted by a range, or the transition at its upper edge. |
| **Valid side** | The partition whose values satisfy the relevant rule. A boundary itself is on this side only when the endpoint is inclusive. |
| **Invalid side** | A partition whose values violate the relevant rule and should produce a defined rejection or error behavior. |
| **2-value BVA** | Selects the nearest representable values on the two sides of a boundary. Use it when the boundary value is already covered elsewhere or when the project explicitly chooses this reduced variant. |
| **3-value BVA** | Selects the value below, the boundary value, and the value above a boundary. This guide uses 3-value BVA in the worked examples. |
| **Boundary coverage** | The proportion of required boundary positions exercised by the test cases. It is distinct from partition coverage and from full combination coverage. |
| **Oracle** | The precise observable result used to determine pass or fail, such as acceptance, a validation message, an HTTP status, a calculated tariff, persistence, or a state change. |

### Boundaries are not only numeric

A boundary exists wherever expected behavior changes. Examples include:

- numeric minimums, maximums, and thresholds;
- string length in characters, graphemes, or bytes;
- file size and number of uploaded items;
- quantity, quota, inventory, or capacity limits;
- dates, times, durations, and age calculations;
- an ordered enumeration where a rule changes between adjacent values;
- a score threshold or a calculated total.

An unordered set such as `{red, green, blue}` has categories but no natural numeric boundary. Use EP or decision tables for that set unless the specification defines an ordered transition.

## Endpoint, increment, and precision rules

Before selecting a test value, make the domain reproducible. Document the following for every input or condition:

1. **Endpoint ownership**: whether each lower and upper endpoint is included (`[`, `]`) or excluded (`(`, `)`). For example, `[1,1000]` includes both endpoints, while `(1,1000]` excludes `1` and includes `1000`.
2. **Domain type**: whether values are discrete (integers, minutes, item counts) or continuous/decimal.
3. **Smallest meaningful increment**: for integers, usually `1`; for a duration, perhaps one minute or one second; for a quantity, one item.
4. **Decimal precision and rounding**: for example, two decimal places with half-up rounding. A test value such as `10.001` is not meaningful if the interface accepts only cents.
5. **Representation and parsing**: accepted date format, numeric notation, locale, sign, leading zeros, and whether the interface accepts values outside the normal domain.
6. **Time settings**: time zone, daylight-saving behavior, reference instant/date, and precision.
7. **Text measurement**: whether length means Unicode code points, grapheme clusters, UTF-8 bytes, or another unit; also define normalization.
8. **Context and dependencies**: role, account state, existing records, and baseline values for other fields.
9. **Unrepresentable values**: how null, missing, blank, malformed, overflow, or negative values are handled. These are usually EP or negative tests, not boundary positions.

When the requirement is ambiguous, ask for clarification if endpoint ownership or the expected oracle changes the test model. If testing must proceed, label the chosen behavior as an **Assumption**, record a **Question/TBD**, and state the residual risk.

### 2-value and 3-value BVA

For **3-value BVA**, select the boundary and one nearest value on each side. For an integer range `[1,1000]`:

- lower boundary: `0` (below), `1` (at), `2` (above);
- upper boundary: `999` (below), `1000` (at), `1001` (above).

For **2-value BVA**, select the two nearest values that straddle the boundary, such as `0` and `1` around the lower edge or `1000` and `1001` around the upper edge. State which two positions are selected. Do not silently call a two-value design “3-value BVA.”

For a continuous domain, “just below” and “just above” require an explicit epsilon based on the supported precision. For a value rounded to two decimal places, the smallest supported step might be `0.01`; for a timestamp stored to seconds, it might be one second. A mathematical infinitesimal is not an executable test value.

## Repeatable BVA procedure

Use this workflow for a form field, API parameter, business rule, file limit, or calculated value:

1. **Read the requirement.** Extract acceptance criteria, rejection rules, state changes, calculations, messages, response codes, and processing paths.
2. **Identify the relevant EP partitions.** BVA relies on partitions and cannot assign a boundary to the correct side without them.
3. **List every input and dependency.** Include representation, unit, context, role, state, existing data, and other fields that can change the result.
4. **Formalize each interval or ordered rule.** Write inequalities or interval notation and state whether every endpoint is included.
5. **Inventory the boundaries.** Include finite lower and upper limits, transitions between adjacent partitions, and outer boundaries such as zero or an empty collection when they have defined behavior.
6. **Determine endpoint ownership.** Assign every boundary value to exactly one expected-behavior partition. Resolve overlaps and gaps before creating cases.
7. **Determine predecessor and successor values.** Use the smallest meaningful discrete increment or the declared decimal/date/time precision.
8. **Select the BVA variant.** Record whether the design uses 2-value or 3-value BVA and whether it is normal (valid-side focus) or robust (also includes invalid-side values).
9. **Choose baseline data.** When several inputs exist, vary the target boundary input while keeping other independent inputs at valid, nominal values. Use decision tables or pairwise testing for deliberate combinations.
10. **Write executable test cases.** Include preconditions, complete input data, actions, boundary position, exact oracle, and traceability to boundary and partition IDs.
11. **Execute and compare with the oracle.** Record acceptance/rejection, exact message or status, calculation, persisted value, processing path, and state change as applicable.
12. **Report coverage and residual risk.** Calculate boundary-position coverage separately from EP partition coverage. Record boundaries that could not be exercised, assumptions, and untested combinations.
13. **Revise the model when evidence changes it.** If supposedly equivalent values behave differently, split the partition or record the defect and update the design.

## Recommended BVA deliverable structure

When no project-specific format is supplied, prepare the design in this order:

1. Scope and requirement assumptions
2. Identified equivalence partitions
3. Boundary inventory
4. Selected BVA variant (2-value or 3-value)
5. Boundary test cases
6. Boundary coverage summary
7. Uncovered risks and complementary techniques
8. Verification checklist

### Boundary inventory template

Use one row for every boundary. The status must distinguish confirmed requirement facts from assumptions and unresolved questions.

| Boundary ID | Input/condition | Adjacent partitions | Formal boundary definition | Endpoint ownership | Unit and precision | Values selected | Expected behavior on each side | Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| B-001 |  |  |  |  |  |  |  | Confirmed / Assumption / Question/TBD |
| B-002 |  |  |  |  |  |  |  | Confirmed / Assumption / Question/TBD |

### Reusable test-case template

Copy and adapt this table to a test-management tool. `Position` should be `below`, `at`, or `above` for 3-value BVA; for 2-value BVA, record the two positions selected by the project.

| Test case ID | Boundary ID | Title/objective | Requirement reference | Priority | Preconditions/setup | Complete input data | Steps | Exact expected result/oracle | Position | Related partition IDs | Assumptions/notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| BVA-001 | B-001 |  |  | High/Medium/Low |  |  | 1.  2.  3.  |  | below / at / above |  |  |

For multi-field cases, record the target boundary input and the nominal values used for all other fields. If another field is intentionally at a boundary, give that boundary its own ID and explain the combination strategy.

## Worked example 1: Integer range

### Requirement assumption and partition model

**Assumption A1:** An integer field named `quantity` accepts every integer in `[1,1000]`, inclusive. The application rejects values below `1` with `Quantity must be at least 1` and values above `1000` with `Quantity must not exceed 1000`. The field accepts integer syntax only. Blank, null, letters, decimals, signs not accepted by the parser, and overflow are separate EP/format cases and are not boundary values in this example. On acceptance, the submitted integer is stored unchanged.

The relevant partitions are:

| Partition ID | Formal definition | Classification | Expected oracle |
| --- | --- | --- | --- |
| `QUANTITY-P1` | `x < 1` for integer `x` | Invalid | Reject; show `Quantity must be at least 1`; do not persist the submission. |
| `QUANTITY-P2` | `1 <= x <= 1000` | Valid | Accept; persist the submitted integer unchanged. |
| `QUANTITY-P3` | `x > 1000` for integer `x` | Invalid | Reject; show `Quantity must not exceed 1000`; do not persist the submission. |

**Selected variant:** 3-value BVA with an increment of one integer. The two finite boundaries are `1` and `1000`.

| Boundary ID | Adjacent partitions | Formal boundary | Endpoint ownership | Values and positions | Status |
| --- | --- | --- | --- | --- | --- |
| `QUANTITY-B1` | `QUANTITY-P1` / `QUANTITY-P2` | Lower edge of `[1,1000]` | `1` belongs to `QUANTITY-P2`; `0` is below and `2` is above. | `0` below, `1` at, `2` above | Assumption A1 |
| `QUANTITY-B2` | `QUANTITY-P2` / `QUANTITY-P3` | Upper edge of `[1,1000]` | `1000` belongs to `QUANTITY-P2`; `999` is below and `1001` is above. | `999` below, `1000` at, `1001` above | Assumption A1 |

### Integer boundary test cases

**Common preconditions:** The quantity form is open, the user is authorized to submit it, and no prior record with the test identifier exists. For each case, enter the stated value and submit once.

| Test case ID | Boundary ID | Objective | Priority | Input value | Steps | Expected result/oracle | Position | Related partitions |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `BVA-Q-001` | `QUANTITY-B1` | Verify the nearest value below the minimum | High | `0` | Enter `0`; submit. | Submission is rejected; `Quantity must be at least 1` is shown; no record is persisted. | below | `QUANTITY-P1` |
| `BVA-Q-002` | `QUANTITY-B1` | Verify the inclusive minimum | High | `1` | Enter `1`; submit. | Submission is accepted and the stored quantity equals integer `1`. | at | `QUANTITY-P2` |
| `BVA-Q-003` | `QUANTITY-B1` | Verify the first value above the minimum | Medium | `2` | Enter `2`; submit. | Submission is accepted and the stored quantity equals integer `2`. | above | `QUANTITY-P2` |
| `BVA-Q-004` | `QUANTITY-B2` | Verify the last value below the maximum | Medium | `999` | Enter `999`; submit. | Submission is accepted and the stored quantity equals integer `999`. | below | `QUANTITY-P2` |
| `BVA-Q-005` | `QUANTITY-B2` | Verify the inclusive maximum | High | `1000` | Enter `1000`; submit. | Submission is accepted and the stored quantity equals integer `1000`. | at | `QUANTITY-P2` |
| `BVA-Q-006` | `QUANTITY-B2` | Verify the nearest value above the maximum | High | `1001` | Enter `1001`; submit. | Submission is rejected; `Quantity must not exceed 1000` is shown; no record is persisted. | above | `QUANTITY-P3` |

The malformed value `abc` is not “just above” or “just below” a numeric boundary. Cover it separately with EP, input-format, and error-handling tests. A passing case for `1` or `1000` is evidence about that selected value, not proof that every valid integer behaves correctly.

## Worked example 2: String length

### Requirement assumption and boundary model

**Assumption S1:** A username must contain **6–15 Unicode code points**, inclusive. The service trims leading and trailing ASCII spaces before counting. A string containing only spaces is therefore blank and is rejected with `Username is required`. Internal spaces are allowed. The test environment uses Unicode normalization form NFC before validation; storage preserves the normalized value. The API rejects lengths below 6 with `Username must contain at least 6 characters` and lengths above 15 with `Username must not exceed 15 characters`.

If the product counts grapheme clusters or UTF-8 bytes instead, replace the test data and recompute the neighboring values. That is a requirement question, not an implementation detail that can be guessed safely.

**Selected variant:** 3-value BVA; the increment is one Unicode code point.

| Boundary ID | Adjacent partitions | Formal boundary | Endpoint ownership | Values and positions | Status |
| --- | --- | --- | --- | --- | --- |
| `USERNAME-B1` | `USERNAME-P1` / `USERNAME-P2` | Lower edge of `[6,15]` code points | Length `6` is valid; lengths `5` and `7` are the adjacent values. | 5 below, 6 at, 7 above | Assumption S1 |
| `USERNAME-B2` | `USERNAME-P2` / `USERNAME-P3` | Upper edge of `[6,15]` code points | Length `15` is valid; lengths `14` and `16` are the adjacent values. | 14 below, 15 at, 16 above | Assumption S1 |

The partitions are:

- `USERNAME-P1`: normalized length `< 6`, invalid;
- `USERNAME-P2`: normalized length `[6,15]`, valid;
- `USERNAME-P3`: normalized length `> 15`, invalid;
- `USERNAME-P4`: trimmed value is empty, invalid required-field case (EP/format coverage, not a length position in this example).

### Username boundary test cases

**Common preconditions:** The username endpoint is available, the test user is authorized, and the selected username does not already exist. Generate strings from `a` so their code-point lengths are unambiguous; additional Unicode cases are shown in the notes.

| Test case ID | Boundary ID | Objective | Priority | Input value | Steps | Expected result/oracle | Position | Related partitions | Assumptions/notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `BVA-U-001` | `USERNAME-B1` | Verify five code points are rejected | High | `aaaaa` | Submit the username. | Request is rejected; `Username must contain at least 6 characters` is returned; no username is created. | below | `USERNAME-P1` | Length is 5 after trimming. |
| `BVA-U-002` | `USERNAME-B1` | Verify six code points are accepted | High | `aaaaaa` | Submit the username. | Request succeeds; the username is created and stored as `aaaaaa`. | at | `USERNAME-P2` | Inclusive lower endpoint. |
| `BVA-U-003` | `USERNAME-B1` | Verify seven code points are accepted | Medium | `aaaaaaa` | Submit the username. | Request succeeds; the username is created and stored as `aaaaaaa`. | above | `USERNAME-P2` | First value above lower boundary. |
| `BVA-U-004` | `USERNAME-B2` | Verify fourteen code points are accepted | Medium | `aaaaaaaaaaaaaa` | Submit the username. | Request succeeds; the username is created and stored unchanged. | below | `USERNAME-P2` | Length is 14. |
| `BVA-U-005` | `USERNAME-B2` | Verify fifteen code points are accepted | High | `aaaaaaaaaaaaaaa` | Submit the username. | Request succeeds; the username is created and stored unchanged. | at | `USERNAME-P2` | Inclusive upper endpoint. |
| `BVA-U-006` | `USERNAME-B2` | Verify sixteen code points are rejected | High | `aaaaaaaaaaaaaaaa` | Submit the username. | Request is rejected; `Username must not exceed 15 characters` is returned; no username is created. | above | `USERNAME-P3` | Length is 16. |

These cases do not fully test whitespace, Unicode normalization, or grapheme behavior. Add EP and risk-based tests for leading/trailing spaces, an internal space, composed and decomposed accented characters, emoji, and byte length when those representations can reach the system. Do not assume that a visual glyph always equals one code point or one byte.

## Worked example 3: Airline baggage prepayment pricing

### Requirement assumption and corrected interval definitions

Let `t` be the time remaining before scheduled departure. Greater `t` means that payment occurs earlier. The pricing requirement is modeled with **minute precision**:

- `t > 24 hours`: apply a 50% discount;
- `3 hours < t <= 24 hours`: apply the basic tariff;
- `0 hours < t <= 3 hours`: apply a 20% surcharge;
- `t <= 0 hours`: reject payment because departure has arrived or passed;
- malformed or missing `t`: return a time-validation error (EP/format coverage).

**Assumption A2:** The exact endpoints follow the interval definitions above. Therefore, `24:00` belongs to the basic-tariff partition and `3:00` belongs to the surcharge partition. The source wording about “earlier than 24 hours” supports `t > 24`; the assignment of `3:00` to surcharge is retained as an explicit assumption because prose about check-in starting at three hours can otherwise be read differently. Confirm this rule with the product owner before execution.

**Assumption A3:** The basic price for the test booking is `100.00` in the booking currency. A discount of 50% produces `50.00`; a 20% surcharge produces `120.00`. Currency rounding is to two decimal places. Payment at `t <= 0` is rejected with `Payment is unavailable after departure`; malformed or missing input receives a format or required-field error.

The partitions are:

| Partition ID | Formal definition | Expected oracle |
| --- | --- | --- |
| `BAGGAGE-P1` | `t > 24:00` | Payment succeeds and final price is `50.00`. |
| `BAGGAGE-P2` | `3:00 < t <= 24:00` | Payment succeeds and final price is `100.00`. |
| `BAGGAGE-P3` | `0:00 < t <= 3:00` | Payment succeeds and final price is `120.00`. |
| `BAGGAGE-P4` | `t <= 0:00` | Payment is rejected with `Payment is unavailable after departure`; no payment is captured. |
| `BAGGAGE-P5` | Missing or malformed time representation | Request is rejected with the applicable required-field or time-format error. |

**Selected variant:** 3-value BVA with one minute as the smallest meaningful increment. The boundary inventory includes both tariff transitions (`24:00`, `3:00`) and the outer departure boundary (`0:00`). A negative duration is included only if the interface can represent it.

| Boundary ID | Adjacent partitions | Formal boundary and ownership | Selected values | Expected behavior | Status |
| --- | --- | --- | --- | --- | --- |
| `BAGGAGE-B1` | `BAGGAGE-P1` / `BAGGAGE-P2` | `24:00`; exact value belongs to `P2` | `23:59` below / `24:00` at / `24:01` above | Basic / basic / discount | Assumptions A2–A3 |
| `BAGGAGE-B2` | `BAGGAGE-P2` / `BAGGAGE-P3` | `3:00`; exact value belongs to `P3` | `2:59` below / `3:00` at / `3:01` above | Surcharge / surcharge / basic | Assumptions A2–A3 |
| `BAGGAGE-B3` | `BAGGAGE-P3` / `BAGGAGE-P4` | `0:00`; exact value belongs to invalid `P4` | `-0:01` below / `0:00` at / `0:01` above | Reject / reject / surcharge | Assumptions A2–A3; negative input must be representable |

“Below” and “above” in the table refer to the numeric value of `t` (hours remaining), not the chronological direction of the clock. At the 24-hour boundary, `24:01` is numerically above `24:00` and belongs to the discount class; `23:59` is numerically below it and remains basic. To avoid this common ambiguity, each case below states its tariff explicitly.

### Airline boundary test cases

**Common preconditions:** The flight departs at a fixed time; the booking is eligible for extra baggage; the basic tariff is `100.00`; the payment service is available; the test clock and time zone are controlled. For each case, submit one prepayment request with the stated time remaining.

| Test case ID | Boundary ID | Objective | Priority | Input value | Steps | Expected result/oracle | Position | Related partitions | Assumptions/notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `BVA-B-001` | `BAGGAGE-B1` | Verify payment just earlier than 24 hours | High | `24:01` | Submit prepayment with `t = 24:01`; inspect the calculated price and payment result. | Payment succeeds; the 50% discount is applied; final price is exactly `50.00`. | above | `BAGGAGE-P1` | Earlier payment has greater `t`. |
| `BVA-B-002` | `BAGGAGE-B1` | Verify exact 24-hour endpoint | High | `24:00` | Submit prepayment with `t = 24:00`. | Payment succeeds at the basic tariff; final price is exactly `100.00`, not `50.00`. | at | `BAGGAGE-P2` | Exact endpoint is basic by A2. |
| `BVA-B-003` | `BAGGAGE-B1` | Verify payment just later than 24 hours | High | `23:59` | Submit prepayment with `t = 23:59`. | Payment succeeds at the basic tariff; final price is exactly `100.00`. | below | `BAGGAGE-P2` | “Later” means less time remaining. |
| `BVA-B-004` | `BAGGAGE-B2` | Verify payment just earlier than three hours | High | `3:01` | Submit prepayment with `t = 3:01`. | Payment succeeds at the basic tariff; final price is exactly `100.00`. | above | `BAGGAGE-P2` | `3:01` has more than three hours remaining. |
| `BVA-B-005` | `BAGGAGE-B2` | Verify exact three-hour endpoint | High | `3:00` | Submit prepayment with `t = 3:00`. | Payment succeeds with the 20% surcharge; final price is exactly `120.00`. | at | `BAGGAGE-P3` | Exact endpoint is surcharge by A2. |
| `BVA-B-006` | `BAGGAGE-B2` | Verify payment just later than three hours | High | `2:59` | Submit prepayment with `t = 2:59`. | Payment succeeds with the 20% surcharge; final price is exactly `120.00`. | below | `BAGGAGE-P3` | Less time remains than three hours. |
| `BVA-B-007` | `BAGGAGE-B3` | Verify one minute before departure | High | `0:01` | Submit prepayment with `t = 0:01`; inspect the calculated price. | Payment succeeds with the 20% surcharge; final price is exactly `120.00`. | above | `BAGGAGE-P3` | The service accepts positive remaining time. |
| `BVA-B-008` | `BAGGAGE-B3` | Verify exact departure-time endpoint | Critical | `0:00` | Submit prepayment with `t = 0:00`. | Payment is rejected with `Payment is unavailable after departure`; no payment is captured. | at | `BAGGAGE-P4` | Exact zero is invalid by A2. |
| `BVA-B-009` | `BAGGAGE-B3` | Verify time after departure | High | `-0:01` | Submit prepayment with `t = -0:01`. | Payment is rejected with `Payment is unavailable after departure`; no payment is captured. | below | `BAGGAGE-P4` | Execute only if negative durations are representable. |

The six cases around `24:00` and `3:00` correspond to the two tariff transitions. The three cases around `0:00` cover the outer invalid boundary when negative time can be represented. The source's Class 2 label for a 30-hour example is incorrect: `30:00` belongs to `BAGGAGE-P1` and receives the discount. A nominal EP case such as `10:00` should additionally verify the interior basic partition; it is not a boundary position.

## Worked example 4: Date/time cutoff

### Cutoff requirement and boundary model

**Assumption D1:** A report submission is accepted through `2026-08-29T17:00:00Z`, inclusive. The service interprets timestamps in UTC, accepts ISO 8601 timestamps with second precision, and compares the instant rather than the displayed local date. A timestamp after the cutoff is rejected with HTTP `409 Conflict` and `Submission window has closed`; an accepted request returns HTTP `202 Accepted` and stores the normalized UTC timestamp.

The relevant boundary is:

| Boundary ID | Adjacent partitions | Formal boundary | Endpoint ownership | Precision and values | Status |
| --- | --- | --- | --- | --- | --- |
| `CUTOFF-B1` | `CUTOFF-P1` / `CUTOFF-P2` | `submittedAt <= 2026-08-29T17:00:00Z` is accepted; later instants are rejected | `17:00:00Z` belongs to accepted `CUTOFF-P1` | One-second precision: `16:59:59Z` below, `17:00:00Z` at, `17:00:01Z` above | Assumption D1 |

The partitions are:

- `CUTOFF-P1`: `submittedAt <= 2026-08-29T17:00:00Z`, accepted;
- `CUTOFF-P2`: `submittedAt > 2026-08-29T17:00:00Z`, rejected;
- malformed, missing, or timezone-invalid timestamps: separate EP/format partitions.

### Date/time boundary test cases

**Common preconditions:** The submission service uses a controllable clock or accepts a test timestamp, the report data is valid, and the test account is authorized. Submit the same report with the stated timestamp and inspect the response and persistence.

| Test case ID | Boundary ID | Objective | Priority | Input value | Steps | Expected result/oracle | Position | Related partitions | Assumptions/notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `BVA-D-001` | `CUTOFF-B1` | Verify one second before the cutoff | High | `2026-08-29T16:59:59Z` | Submit the report with the timestamp; inspect response and stored timestamp. | Response is HTTP `202 Accepted`; submission is persisted with normalized instant `2026-08-29T16:59:59Z`. | below | `CUTOFF-P1` | UTC and second precision. |
| `BVA-D-002` | `CUTOFF-B1` | Verify the inclusive cutoff instant | Critical | `2026-08-29T17:00:00Z` | Submit the report with the timestamp. | Response is HTTP `202 Accepted`; submission is persisted with normalized instant `2026-08-29T17:00:00Z`. | at | `CUTOFF-P1` | Endpoint is inclusive by D1. |
| `BVA-D-003` | `CUTOFF-B1` | Verify one second after the cutoff | Critical | `2026-08-29T17:00:01Z` | Submit the report with the timestamp. | Request is rejected; HTTP `409 Conflict` and `Submission window has closed` are returned; no submission is persisted. | above | `CUTOFF-P2` | Rejection code is an explicit assumption in D1 and must match the API contract. |

If the product uses local time, convert all three cases to the declared time zone and test daylight-saving transitions separately. If precision is one minute, the nearest values are one minute apart, not one second apart. If the endpoint is exclusive, the expected result for `17:00:00Z` changes and the boundary model must be updated before execution.

## Boundary coverage and partition coverage

Boundary coverage must be reported independently from EP coverage. A suite can exercise every boundary position while missing a meaningful interior value, and an EP suite can cover every partition while missing the edge where an operator defect occurs.

For 3-value BVA, calculate:

`exercised below/at/above positions / required below/at/above positions × 100%`

For 2-value BVA, calculate coverage against the two positions selected for each boundary. Also report EP partition coverage separately:

`exercised partitions / identified partitions × 100%`

Example for the integer range:

| Boundary | Required positions | Cases | Coverage |
| --- | --- | --- | --- |
| `QUANTITY-B1` | below, at, above | `BVA-Q-001`, `BVA-Q-002`, `BVA-Q-003` | 3/3 |
| `QUANTITY-B2` | below, at, above | `BVA-Q-004`, `BVA-Q-005`, `BVA-Q-006` | 3/3 |
| **Total** | 6 positions | 6 cases | **6/6 = 100% boundary-position coverage** |

This does not claim that all integers work. It means only that the selected boundary positions were exercised. The same cases also exercise all three numeric EP partitions, but malformed, blank, decimal, and overflow partitions remain uncovered under Assumption A1.

Do not claim that a fixed number of tests is always sufficient. The count depends on the number of boundaries, endpoint rules, domain precision, selected BVA variant, reusable cases, and whether robust invalid-side testing is included.

## When to use BVA

BVA is especially useful when:

- a requirement specifies minimum or maximum values;
- behavior changes at a threshold or interval edge;
- a field has a length, size, quantity, quota, or capacity limit;
- a date, time, duration, age, or cutoff controls behavior;
- incorrect comparison operators or off-by-one calculations are plausible;
- a calculation changes tariff, eligibility, discount, tax, or workflow at a defined value;
- a finite boundary is high-impact, safety-relevant, or historically defective.

Apply BVA after the partitions, endpoint ownership, and expected oracle are understood. Use normal BVA for the expected valid domain and robust BVA when invalid-side behavior is also important and representable.

## Limitations and common mistakes

BVA alone is insufficient for:

- malformed formats, non-numeric input, null, missing, and unsupported representations;
- values well inside a partition;
- combinations of multiple conditions or fields;
- state transitions and lifecycle rules;
- unordered sets and categories without meaningful ordering;
- security, performance, reliability, usability, accessibility, compatibility, and exploratory risks.

Avoid these mistakes:

1. **Leaving endpoint ownership implicit.** Write `[`, `]`, `(`, `)`, or an equivalent inequality for every interval.
2. **Using the wrong neighboring value.** Derive predecessor and successor from the smallest supported increment, not from an arbitrary number.
3. **Forgetting the exact edge.** In 3-value BVA, include the boundary value itself and label it `at`.
4. **Confusing chronological and numeric direction.** For “hours before departure,” `24:01` is more time remaining than `24:00`; name the partition rather than relying on “before” or “after.”
5. **Treating malformed input as a boundary value.** `abc` is not numerically just above `1000`; it belongs to a format partition.
6. **Assuming characters equal bytes or glyphs.** Define the measurement unit and normalization before testing string length.
7. **Ignoring one-sided or outer boundaries.** Zero, empty collection, first item, maximum capacity, or an invalid negative value can have important behavior even when there is no lower valid interval.
8. **Changing several variables at once.** Hold non-target inputs nominal unless testing a planned interaction.
9. **Using a vague oracle.** Record the exact message, status, calculated amount, persistence, or state transition.
10. **Reporting BVA as full coverage.** Boundary coverage does not prove every value, partition, combination, state, or non-functional property is covered.
11. **Copying contradictory examples.** Recalculate every representative from the formal interval; for example, `30 hours` belongs to `t > 24`, not to `3 < t <= 24`.
12. **Ignoring rounding and clock behavior.** Currency precision, timestamp precision, time zones, DST, and reference clocks can move the effective boundary.

## Complementary techniques

| Technique | How it complements BVA |
| --- | --- |
| **Equivalence Partitioning** | Identifies valid, invalid, and interior behavioral partitions; BVA then targets their edges. |
| **Decision tables** | Covers combinations of conditions and their actions when a boundary result depends on several rules. |
| **Pairwise testing** | Covers interactions among mostly independent parameters without enumerating every combination. |
| **State-transition testing** | Tests boundaries whose behavior depends on the current lifecycle state or event history. |
| **Condition/cause-effect coverage** | Exercises complex Boolean relationships that cannot be represented by one ordered boundary. |
| **Error guessing** | Adds malformed, unusual, overflow, rounding, timezone, and historically defective values. |
| **Risk-based testing** | Prioritizes boundaries by business impact, likelihood, detectability, and safety or financial consequences. |

A practical sequence is to use EP for the behavioral model, BVA for each meaningful edge, decision tables or pairwise testing for combinations, state-transition testing for lifecycle context, and risk-based or error-guessing tests for values that the formal boundary model cannot predict.

## Verification checklist

Before approving a BVA test design, confirm:

- [ ] The requirement, scope, and observable oracle are documented.
- [ ] Relevant EP partitions were identified before the boundaries were selected.
- [ ] Every modeled interval has explicit lower and upper endpoint ownership.
- [ ] Partitions are non-empty, mutually exclusive, and collectively exhaustive for the declared domain.
- [ ] Every finite transition and relevant outer boundary has a stable Boundary ID.
- [ ] The input unit, discrete increment, decimal precision, date/time precision, and rounding rules are stated.
- [ ] Date/time tests state the reference date or instant, time zone, endpoint rule, and DST assumptions where relevant.
- [ ] String tests state whether length means characters, graphemes, or bytes and address normalization where relevant.
- [ ] The selected BVA variant is explicitly named as 2-value or 3-value.
- [ ] Each 3-value boundary has a value labeled `below`, `at`, and `above`; each 2-value boundary has its two selected positions recorded.
- [ ] Every selected value is representable and belongs to the expected side or partition.
- [ ] Preconditions, complete input data, actions, priority, and exact oracles are executable.
- [ ] Other fields are nominal unless an interaction is intentionally under test.
- [ ] Malformed, blank, null, unsupported, and overflow inputs are covered separately by EP or negative testing and are not mislabeled as BVA.
- [ ] Boundary coverage is calculated separately from partition coverage and combination coverage.
- [ ] Uncovered boundaries, assumptions, questions/TBDs, and residual risks are visible.
- [ ] Airline interval examples assign `24:00` to basic, `3:00` to surcharge, and `t <= 0` to the stated invalid behavior when those are the selected assumptions.
- [ ] No vague oracle such as “the system works correctly” remains.
- [ ] Complementary techniques are selected for combinations, states, logic, security, and non-functional risks.
- [ ] Markdown headings and tables render correctly; no duplicate headings, article metadata, placeholders, or non-English text remain.

## Summary

Boundary Value Analysis is a focused way to test where specified behavior changes. Start with an explicit equivalence-partition model, define endpoint ownership and the smallest meaningful increment, select a stated 2-value or 3-value variant, and write cases with exact observable oracles. Use boundary coverage to report which edges were exercised, but combine BVA with EP and other techniques because edge tests alone cannot cover interiors, malformed input, combinations, states, or non-functional risks.
