---
name: boundary-value-analysis
description: Apply ISTQB-aligned Boundary Value Analysis to a supplied requirement or test case. Trigger when the user asks to identify boundaries, select values around limits, review boundary coverage, or prepare structured boundary-focused test cases, including numeric, length, date/time, file-size, count, and business-rule boundaries.
version: 0.1.0
---

# Boundary Value Analysis Test Design

Apply this skill when the user provides a requirement, acceptance criterion, existing test case, input field, API parameter, file-upload rule, date/time rule, limit, threshold, or business rule and asks to use **Boundary Value Analysis (BVA)** to prepare or review test cases.

Use this project's detailed technique guide as the primary terminology, workflow, and example reference:

`docs/TestDesignAndSoftwareTestingTechniques/BlackBox/boundaryValueAnalysis.md`

If the guide is unavailable, follow the rules in this skill and use ISTQB-consistent black-box, specification-based test-design terminology. The guide is the authoritative practical supplement for this project; do not invent product behavior that is not stated in the supplied requirement.

## Scope and core principle

Treat BVA as a black-box, specification-based test design technique focused on values at and around the edges of equivalence partitions. BVA targets places where expected behavior changes, such as an inclusive minimum, exclusive maximum, length limit, time cutoff, quota, or tariff transition.

BVA depends on an equivalence-partition model:

- **Equivalence Partitioning (EP)** identifies behavioral partitions.
- **BVA** identifies the boundaries between adjacent partitions and relevant outer boundaries.
- BVA selects the nearest representable values below, at, and above a boundary, or the two sides when 2-value BVA is explicitly selected.
- EP and BVA complement one another; neither technique replaces the other.

BVA helps expose comparison-operator, off-by-one, rounding, truncation, unit-conversion, and predecessor/successor defects. It does not prove that all values in a partition work, and it does not inherently cover malformed formats, interior values, combinations, state transitions, security, performance, usability, accessibility, compatibility, or exploratory risks.

## Input contract

Accept any of the following:

- A complete requirement or acceptance criterion
- An existing test case that needs boundary review or expansion
- A single form field or API parameter with a minimum, maximum, threshold, length, size, count, date, or time rule
- A multi-field form, endpoint, batch rule, or file-upload requirement
- A date/time cutoff, duration, age, quota, capacity, or pricing rule
- A business rule with roles, account states, existing data, or lifecycle context

Extract, when available:

- System action or operation under test
- Every input and its semantic meaning
- Data type, format, unit, representation, and ordering
- Equivalence partitions and their expected behaviors
- Lower, upper, internal, threshold, transition, and outer boundaries
- Inclusive/exclusive endpoint ownership
- Discrete increment, decimal precision, rounding, and truncation
- Date format, reference date/instant, time zone, daylight-saving behavior, and time precision
- String measurement unit: characters, Unicode code points, grapheme clusters, or bytes
- Valid and invalid behavior on each side of a boundary
- Observable oracles: response code, validation message, acceptance/rejection, calculation, persistence, processing path, or state change
- Preconditions, setup data, user role, account state, existing records, locale, and dependencies
- Requirement ID, severity, and priority

An existing test case is evidence to review, not proof that boundaries, endpoint ownership, precision, or coverage are correct.

## Clarifications and assumptions

Ask only questions that block safe boundary selection or make the expected oracle unknowable. Prioritize:

- Inclusive versus exclusive lower and upper endpoints
- Which partition owns the exact boundary value
- Integer versus decimal domain and supported precision
- Smallest meaningful increment for discrete, date, time, size, or count values
- Rounding, truncation, unit conversion, and overflow behavior
- Date format, reference date/instant, time zone, daylight-saving rules, and time precision
- Whether string length counts characters, code points, graphemes, or bytes
- Whitespace trimming, Unicode normalization, locale, and encoding
- Null, missing, empty, whitespace-only, malformed, unsupported, unreadable, or negative representations
- State-, role-, account-, or context-dependent behavior
- Exact expected status, message, calculation, persistence, processing path, or state transition

If clarification is not essential, proceed and label information explicitly as one of:

- **Confirmed** — stated in the supplied requirement
- **Assumption** — introduced to make a provisional boundary model possible
- **Question/TBD** — unresolved and requiring confirmation
- **Residual risk** — a meaningful area not covered by the generated cases

Never present an invented endpoint rule or oracle as confirmed. If endpoint ownership or expected behavior is unknown and materially changes the cases, provide the clarification and only provisional cases that do not depend on the unknown behavior.

Malformed, missing, null, blank, unsupported, overflow, and non-numeric representations are usually EP or negative/format cases, not boundary positions. Include them separately when they can reach the system and their behavior is relevant.

## Boundary-modeling procedure

Follow the detailed workflow in `docs/TestDesignAndSoftwareTestingTechniques/BlackBox/boundaryValueAnalysis.md`:

1. **Extract observable outcomes.** Identify acceptance, rejection, response codes, validation messages, calculations, persistence, processing paths, and state changes.
2. **Model or confirm EP partitions first.** Write the valid, invalid, and differently behaving partitions before choosing boundary values.
3. **List inputs and dependencies.** Include fields, representations, units, precision, roles, states, existing data, locale, time zone, and cross-field dependencies.
4. **Formalize intervals and ordered rules.** Use notation such as `[1,1000]`, `(1,1000]`, `x < 1`, or `x > 1000`. State every endpoint's ownership.
5. **Inventory boundaries.** Include inner boundaries (transitions between adjacent partitions), lower and upper limits, threshold changes, and relevant outer boundaries such as zero, empty, first item, or maximum capacity.
6. **Check the model.** Partitions must be non-empty, mutually exclusive, and collectively exhaustive for the declared domain. Resolve overlaps and gaps before generating cases.
7. **Determine the nearest values.** Use the smallest meaningful increment for discrete domains, the supported precision for decimal values, and the smallest meaningful unit for dates and times.
8. **Select the variant.** Explicitly state **3-value BVA** (below, at, above) or **2-value BVA** (the selected values on either side). Also state whether the scope is normal BVA or robust BVA that includes invalid-side values.
9. **Choose baseline data.** Vary the target boundary input while keeping other independent inputs at valid, nominal values. Deliberately combine boundaries only when testing an interaction.
10. **Create executable cases.** Include preconditions, complete input data, actions, priority, exact oracle, boundary position, boundary ID, and related partition IDs.
11. **Execute and compare.** Check the actual response, message, calculation, stored value, processing path, or state against the requirement oracle.
12. **Report coverage and risk.** Calculate boundary-position coverage separately from EP partition coverage and combination coverage. List uncovered boundaries, assumptions, questions, and residual risks.
13. **Update the model when evidence changes it.** If values expected to behave equivalently do not, split the partition or record the defect and revise the design.

## Boundary-value selection rules

### Discrete inclusive range

For an integer range `[L,U]` with increment `1`, 3-value BVA normally selects:

- Lower boundary: `L-1` (**below**), `L` (**at**), `L+1` (**above**)
- Upper boundary: `U-1` (**below**), `U` (**at**), `U+1` (**above**)

For `[1,1000]`, this is `0, 1, 2` and `999, 1000, 1001`. When the invalid-side values are representable and intentionally included, label this **3-value robust BVA**; a normal design may keep invalid-side behavior as separate EP/negative coverage. Do not confuse representable out-of-range numbers with malformed input.

### Exclusive endpoints and transitions

For an exclusive endpoint, the exact boundary belongs to the other adjacent partition. Select the nearest representable values on each side and state the ownership explicitly. For example, for `x < 10` versus `x >= 10`, `10` is on the second partition; `9` and `11` are its nearest integer neighbors.

For a rule such as:

- `t > 24`: discount
- `3 < t <= 24`: basic tariff
- `0 < t <= 3`: surcharge

`24` belongs to the basic partition and `3` belongs to the surcharge partition. Values just above `24` are discount; values just below `24` are basic. Values just above `3` are basic; values just below `3` are surcharge. If `t` is hours remaining, explain numeric ordering because “earlier” and “later” can be misleading.

### Decimal, date, time, and non-numeric boundaries

- Define supported decimal places before choosing an epsilon; do not use an unrepresentable mathematical infinitesimal.
- Define the smallest meaningful date/time unit, such as one day, minute, or second.
- For date/time rules, fix the time zone, reference instant/date, precision, endpoint ownership, and daylight-saving behavior.
- For string length, define whether the unit is characters, code points, graphemes, or bytes and state whitespace and normalization behavior.
- For file size, item count, quota, and capacity, state whether the limit is inclusive and use one meaningful unit above and below it.
- If there is no meaningful ordering, use EP, decision tables, or another suitable technique instead of forcing BVA.

## Default output format

Preserve a format requested by the user (Gherkin, JSON, CSV, test-management table, or another schema). If no format is requested, output Markdown with these sections:

```markdown
## Scope and requirement assumptions

## Identified equivalence partitions

## Boundary inventory

## Selected BVA variant

## Boundary test cases

## Boundary coverage summary

## Uncovered risks and complementary techniques

## Verification checklist
```

### Scope and requirement assumptions

Summarize the feature, operation, requirement basis, input domain, endpoint rules, precision, selected BVA variant, and observable oracles. State what BVA will verify and what it will not cover.

### Identified equivalence partitions

Use stable IDs and formal definitions before selecting boundary cases:

| Partition ID | Input ID | Formal definition | Valid/invalid | Expected behavior/oracle | Representative value | Status |
| --- | --- | --- | --- | --- | --- | --- |
| `INPUT-P1` | `INPUT` |  |  |  |  | Confirmed / Assumption / Question/TBD |

### Boundary inventory

Use one row per inner transition or relevant outer boundary:

| Boundary ID | Input/condition | Adjacent partitions | Formal boundary definition | Endpoint ownership | Unit and precision | Values selected | Expected behavior on each side | Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `INPUT-B1` |  |  |  |  |  |  |  | Confirmed / Assumption / Question/TBD |

### Selected BVA variant

State one of the following explicitly:

- **3-value BVA**: below, at, and above each selected boundary.
- **2-value BVA**: the two selected values on either side of each boundary; explain why the exact boundary is covered elsewhere or why the reduced variant is appropriate.
- **Normal BVA**: focuses on the specified/expected valid domain.
- **Robust BVA**: includes representable invalid-side values as well.

Do not call malformed values such as `abc` a numeric “above” or “below” value.

### Boundary test cases

Generate executable cases rather than only listing values:

| Test case ID | Boundary ID | Title/objective | Requirement reference | Priority | Preconditions/setup | Complete input data | Steps | Exact expected result/oracle | Position (`below`/`at`/`above`) | Related partition IDs | Assumptions/notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `BVA-001` | `INPUT-B1` |  |  | High/Medium/Low |  |  | 1.  2.  3.  |  |  |  |  |

For several inputs, identify the target boundary and keep non-target independent inputs nominal. Record every boundary intentionally combined in the case.

### Boundary coverage summary

For 3-value BVA, calculate:

`exercised below/at/above positions / required below/at/above positions × 100%`

For 2-value BVA, calculate coverage against the two positions selected for each boundary. Also report EP partition coverage separately:

`exercised partitions / identified partitions × 100%`

Boundary coverage is not partition coverage and neither metric is full combination coverage. A suite can cover all edges and still miss an interior behavior, or cover every partition while missing a comparison defect at an edge.

### Uncovered risks and complementary techniques

Record untested boundaries, malformed and unsupported representations, untested combinations, context/state gaps, assumptions, and questions/TBDs. Recommend complementary techniques according to risk.

### Verification checklist

Before presenting the design, verify:

- Requirement and exact oracle are traceable.
- EP partitions were modeled before boundaries were selected.
- Every boundary has a stable ID and adjacent partitions.
- Endpoint ownership is explicit and intervals have no overlap or gap.
- The domain, unit, increment, precision, rounding, and representation are stated.
- Date/time reference, time zone, precision, and DST assumptions are documented where relevant.
- String length measurement and normalization are documented where relevant.
- The selected BVA variant is explicitly identified as 2-value or 3-value, and normal or robust.
- Every case has the correct `below`, `at`, or `above` position where applicable.
- Every selected value is representable and belongs to the stated side and partition.
- Preconditions, complete data, steps, priority, and exact observable expected results are executable.
- Non-target inputs are nominal unless a combination is intentional and documented.
- Malformed, blank, null, unsupported, and overflow tests are separate EP/negative coverage, not mislabeled boundary values.
- Boundary-position coverage is reported separately from partition and combination coverage.
- Uncovered boundaries, assumptions, questions/TBDs, and residual risks are visible.
- Complementary EP, decision-table, pairwise, state-transition, condition/cause-effect, error-guessing, and risk-based testing is recommended where appropriate.
- No vague oracle such as “the system works correctly” remains.

## Complementary techniques

Use BVA with:

- **Equivalence Partitioning** to identify valid, invalid, and interior behavioral classes.
- **Decision tables** to cover combinations of conditions and actions.
- **Pairwise testing** to cover interactions among mostly independent parameters.
- **State-transition testing** to cover state-dependent boundaries and lifecycle rules.
- **Condition/cause-effect coverage** to exercise complex Boolean relationships.
- **Error guessing** to add malformed, unusual, overflow, rounding, timezone, and historically defective values.
- **Risk-based testing** to prioritize safety-critical, financial, high-impact, or historically problematic boundaries.

## Compact examples

Use the full project guide for detailed examples and tables:

`docs/TestDesignAndSoftwareTestingTechniques/BlackBox/boundaryValueAnalysis.md`

Its examples cover:

- Integer values in `[1,1000]`: `0,1,2` and `999,1000,1001`.
- Username length `6–15` inclusive with explicit Unicode and normalization assumptions.
- Airline pricing around `24:00`, `3:00`, and `0:00` with one-minute precision and corrected endpoint ownership.
- A UTC date/time cutoff tested one second before, at, and one second after the inclusive boundary.

Do not copy example assumptions into a real product requirement without confirmation.
