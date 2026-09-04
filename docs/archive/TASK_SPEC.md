# Task Specification: Black-Box Test Design Techniques

## Goal

Prepare clear, structured, English-language documentation for the following black-box test design techniques:

1. **Equivalence Partitioning (EP) / Equivalence Class Partitioning**
2. **Boundary Value Analysis (BVA)**

The documentation must be written as a detailed senior-QA guide. A junior QA engineer should be able to read the guide, replace the example requirement with their own case, derive test conditions, and prepare executable test cases.

The documentation must be based on the project source material and aligned with ISTQB terminology and principles. Correct contradictions, ambiguous interval definitions, incomplete examples, and grammar issues from the source material instead of copying them unchanged.

## Target files

- Equivalence Partitioning:
  `docs/TestDesignAndSoftwareTestingTechniques/BlackBox/equivalencePartitioning.md`
- Boundary Value Analysis:
  `docs/TestDesignAndSoftwareTestingTechniques/BlackBox/boundaryValueAnalysis.md`

The corresponding source material is already available in each target file. Rewrite and structure the content in place. Do not modify unrelated documentation or IDE files unless explicitly requested.

---

# Requirements for both technique guides

Each guide must:

1. Be written entirely in English.
2. Use a consistent Markdown heading hierarchy and correctly rendered tables.
3. Explain the technique in practical terms before presenting examples.
4. Use ISTQB-consistent terminology and distinguish the technique from related techniques.
5. Be written for a junior QA engineer but contain the precision expected from a senior QA engineer.
6. Explain:
   - what the technique is;
   - why it is used;
   - what risks it addresses;
   - how to apply it step by step;
   - when to use it;
   - when it is insufficient;
   - how to combine it with other test design techniques.
7. Include realistic, technically correct examples with:
   - an explicit requirement assumption;
   - identified input or boundary conditions;
   - formal definitions or interval notation where applicable;
   - selected test values;
   - preconditions and actions;
   - exact expected results oracles;
   - traceability to the relevant partition or boundary.
8. Include a reusable test-case template that can be copied into a test-management tool.
9. Include a practical checklist for reviewing the designed tests.
10. Explain limitations and common mistakes.
11. Avoid unsupported claims such as “one test proves that all values work.”
12. State assumptions explicitly when the supplied requirement is incomplete.
13. Ensure every example is internally consistent:
    - all intervals have clear endpoint ownership;
    - representatives belong to the stated class or boundary;
    - expected results match the requirement;
    - no class or relevant boundary is accidentally omitted.

Use precise observable oracles, such as validation messages, response codes, acceptance/rejection, calculated values, persisted values, processing paths, or state changes. Do not use vague expected results such as “the system works correctly.”

---

# Equivalence Partitioning requirements

## 1. Definition and ISTQB alignment

Explain EP as a black-box, specification-based test design technique in which test cases execute representative values from equivalence partitions/classes. State that, in principle, each identified partition should be covered at least once.

Define an equivalence partition/class as a portion of an input or output domain for which behavior is expected to be equivalent according to the specification.

Explain that partitions may be:

- valid or invalid;
- continuous or non-continuous;
- finite or infinite;
- based on numeric, textual, date/time, file, Boolean, enumerated, output, internal, or contextual values.

Reference ISTQB CTFL section 4.2.1 and learning objective FL-4.2.1 (K3), while making clear that the guide is a practical supplement and not a replacement for the official syllabus.

## 2. EP core principles

Describe the following principles:

- Choose at least one representative value from each partition.
- Define partitions according to equivalent expected behavior, not only data type.
- Split invalid input into separate partitions when the error message, response, processing path, or other observable behavior differs.
- Partitions should be non-empty, mutually exclusive, and collectively exhaustive for the declared domain.
- One passing representative does not prove that every value in the partition is defect-free.
- EP reduces redundant tests but does not inherently detect boundary defects or all combinations.

Explain partition coverage and, for multiple independent input domains, **Each Choice Coverage**. Clearly distinguish it from full combination coverage.

## 3. EP partitioning rules and assumptions

The guide must show how to document:

- inclusive and exclusive endpoints;
- integer and decimal precision;
- accepted formats and representations;
- null, missing, empty, and whitespace-only values;
- malformed and unsupported values;
- locale and encoding;
- date format, reference date, time zone, and time precision;
- user role, account state, existing data, and other context;
- dependencies between input fields.

Explain that requirements should be clarified when ambiguity changes the partition model or expected oracle. Otherwise, proceed with clearly labelled assumptions, questions/TBDs, and residual risks.

## 4. EP application procedure

Document this repeatable workflow:

1. Read the requirement and extract acceptance criteria and observable outcomes.
2. Identify all inputs, representations, data types, and contextual dependencies.
3. Define the complete relevant domain for every input.
4. Identify valid and invalid behavioral partitions.
5. Split invalid classes only when their expected behavior differs.
6. Formalize ranges, sets, formats, endpoints, and special values.
7. Check that partitions are non-empty, mutually exclusive, and collectively exhaustive.
8. Select a representative value from every partition.
9. Choose clear, reproducible, risk-relevant representatives; prefer interior values when BVA will cover the edges.
10. Create test cases with preconditions, steps, complete input data, and exact expected results.
11. Map test cases to partition IDs and calculate partition coverage.
12. Record uncovered combinations, assumptions, and residual risks.
13. Update the partition model if execution shows that supposedly equivalent values behave differently.

## 5. EP examples

Include accurate examples such as:

### Numeric range

For an integer field accepting `[1,1000]`, show at least:

- below minimum: `x < 1`;
- valid range: `1 <= x <= 1000`;
- above maximum: `x > 1000`;
- malformed or non-numeric representation.

Use representatives and expected results. Do not combine numeric negatives, letters, symbols, and decimals unless the requirement gives them equivalent behavior.

### College admission percentage

Assume whole-number percentages from `50` through `90`, inclusive, are accepted. Show partitions for:

- below range;
- valid range;
- above range;
- malformed/non-numeric input, if applicable;
- blank/missing input, if its behavior differs.

Use representatives such as `49`, `70`, `91`, `abc`, and blank, and state the expected result for each.

### Time-based airline pricing

Define `t` as hours before departure and explicitly assign endpoints. For example:

- `t > 24`: discount;
- `3 < t <= 24`: basic tariff;
- `0 < t <= 3`: surcharge;
- `t <= 0`: rejected if departure has arrived or passed;
- malformed/missing input: validation error.

Use representatives such as `30`, `10`, `2`, `0`, and malformed input. Explain that BVA must additionally test the boundaries around `24` and `3`.

### Date of birth

Define accepted format, supported historical range, reference date, and time-zone assumptions. Include partitions for:

- valid date;
- date before the supported range;
- future date;
- impossible calendar date;
- malformed format;
- blank/missing date.

Explain that age categories are separate partitions only when they produce different observable outcomes and the reference date and age-calculation rules are fixed.

## 6. EP complementary techniques

Explain how EP works with:

- **Boundary Value Analysis** for values below, at, and above boundaries;
- **Decision tables** for combinations of conditions and actions;
- **Pairwise testing** for interactions among mostly independent inputs;
- **State-transition testing** for lifecycle and context-dependent behavior;
- **Condition/cause-effect coverage** for complex logical relationships;
- **Error guessing and risk-based testing** for historical, domain-specific, security, and likely-user-error risks.

## 7. EP output structure

The guide must recommend the following output structure when no project-specific format is supplied:

1. Scope and oracle
2. Clarifications, assumptions, questions, and residual risks
3. Input model
4. Partition model
5. Prepared test cases
6. Partition coverage
7. Complementary coverage
8. Manual verification checklist

The partition table should include:

- Partition ID
- Input ID
- Formal definition
- Valid/invalid classification
- Expected behavior/oracle
- Representative value
- Representative rationale
- Requirement/assumption status

The test-case table should include:

- Test case ID
- Title/objective
- Requirement reference
- Priority
- Preconditions/setup
- Complete input data
- Steps
- Exact expected result/oracle
- Covered partition IDs
- Technique tags
- Assumptions/notes

---

# Boundary Value Analysis requirements

## 1. Definition and ISTQB alignment

Explain BVA as a black-box, specification-based test design technique focused on the boundaries of equivalence partitions, where defects are frequently found.

Explain the relationship between EP and BVA:

- EP identifies the partitions.
- BVA identifies the edges of those partitions.
- BVA derives test values at or near the boundaries.
- The two techniques are commonly used together but are not interchangeable.

Reference ISTQB CTFL terminology and explain that BVA is applied after the relevant partitions and their endpoint ownership are understood. If a specific ISTQB syllabus version is named, use its terminology and state the version.

## 2. BVA terminology

Define:

- equivalence partition/class;
- boundary or edge of a partition;
- boundary value;
- value just below a boundary;
- value just above a boundary;
- lower and upper boundary;
- valid and invalid side of a boundary;
- 2-value BVA and 3-value BVA, if both are discussed;
- boundary coverage.

Explain that a boundary is not always numeric. Boundaries can occur in:

- numeric ranges;
- string length;
- date and time ranges;
- file size;
- number of items;
- age or other calculated values;
- ordered enumerations or rule transitions.

## 3. BVA test-value selection

Describe the standard approach:

1. Identify the equivalence partitions.
2. Identify every relevant boundary between adjacent partitions and the outer boundaries of the domain.
3. Determine which partition owns each boundary value.
4. Select values immediately below, at, and immediately above the boundary for 3-value BVA.
5. For discrete domains, use the smallest meaningful increment.
6. For continuous or decimal domains, define the precision and use the smallest supported increment.
7. For date/time domains, use the smallest meaningful unit, such as one day, minute, or second.
8. Execute the tests and compare actual results with the expected oracle.
9. Record boundary coverage and any uncovered boundary risks.

Explain 2-value BVA when appropriate: use the two values on either side of a boundary, especially when the goal is to check the transition between adjacent partitions and the value at the boundary has already been covered by another test. Explain that the chosen BVA variant must be stated rather than assumed.

For 3-value BVA, use the boundary value and one value on each side. For a range `[1,1000]`, illustrate the boundary sets:

- lower boundary: `0`, `1`, `2`;
- upper boundary: `999`, `1000`, `1001`.

Clarify that the exact test values depend on the domain, precision, and endpoint rules.

## 4. BVA endpoint and interval rules

The guide must require explicit endpoint ownership. For every interval, state whether the lower and upper endpoints are included.

For example, if the requirement says:

- `t > 24`: discount;
- `3 < t <= 24`: basic tariff;
- `0 < t <= 3`: surcharge;

then:

- `24` belongs to the basic-tariff partition;
- `3` belongs to the surcharge partition;
- values just above `24` belong to the discount partition;
- values just below `24` remain in the basic-tariff partition;
- values just above `3` belong to the basic-tariff partition;
- values just below `3` remain in the surcharge partition.

Correct any source example that contradicts its own class table or labels a representative with the wrong partition.

## 5. BVA examples

Include accurate, step-by-step examples with test cases and expected results.

### Numeric field from 1 through 1000

Assume integer values in `[1,1000]` are accepted. Show BVA cases for:

- lower boundary: `0`, `1`, `2`;
- upper boundary: `999`, `1000`, `1001`.

State which values should be accepted and which should be rejected. Explain that malformed values such as letters are not boundary values and require EP or negative/format testing.

### String length

Use a requirement such as “a username must contain 6–15 characters inclusive.” Show the lower and upper boundary values and their neighboring values. State the unit being counted (characters, bytes, or another measure) and address whitespace, Unicode, and normalization when relevant.

### Airline baggage pricing

Use the corrected interval definitions:

- `t > 24`: discount;
- `3 < t <= 24`: basic tariff;
- `0 < t <= 3`: surcharge;
- `t <= 0`: invalid if payment is prohibited after departure.

Show boundary tests around `24`, `3`, and `0`, including the smallest meaningful time increment. Map every case to the expected tariff or validation behavior.

### Date/time boundary

Include an example where the rule depends on a date or time boundary. State the time zone, precision, reference time/date, and whether the endpoint is included. Test immediately before, at, and immediately after the boundary.

## 6. BVA when to use and limitations

Explain that BVA is especially useful when:

- requirements contain minimum or maximum values;
- an input has a lower or upper limit;
- behavior changes at an interval edge;
- string length, file size, item count, date, or time limits exist;
- defects caused by incorrect comparison operators are likely.

Explain that BVA alone is insufficient for:

- malformed formats and non-numeric input;
- values well inside a partition;
- combinations of multiple conditions;
- state transitions and lifecycle rules;
- unordered sets and categories without meaningful ordering;
- security, performance, usability, accessibility, compatibility, or exploratory risks.

## 7. BVA output structure

The guide must recommend a structured output containing:

1. Scope and requirement assumptions
2. Identified equivalence partitions
3. Boundary inventory
4. Selected BVA variant (2-value or 3-value)
5. Boundary test cases
6. Boundary coverage summary
7. Uncovered risks and complementary techniques
8. Verification checklist

The boundary inventory should include:

- Boundary ID
- Input/condition
- Adjacent partitions
- Formal boundary definition
- Endpoint ownership
- Unit and precision
- Values selected
- Expected behavior on each side
- Requirement/assumption status

The BVA test-case table should include:

- Test case ID
- Boundary ID
- Title/objective
- Priority
- Preconditions/setup
- Input value
- Steps
- Expected result/oracle
- Covered boundary position (`below`, `at`, `above`)
- Related partition IDs
- Assumptions/notes

## 8. BVA complementary techniques

Explain that BVA should be combined with:

- **Equivalence Partitioning** to identify the partitions and representative interior values;
- **Decision tables** for combinations of rules;
- **Pairwise testing** for interactions among multiple parameters;
- **State-transition testing** for state-dependent boundaries;
- **Condition/cause-effect coverage** for complex Boolean logic;
- **Error guessing and risk-based testing** for malformed, unusual, or historically problematic values.

---

# Quality and acceptance criteria

Before considering the documentation complete, verify that:

- Both techniques have standalone English guides.
- Each guide contains a definition, purpose, examples, application procedure, applicability guidance, limitations, complementary techniques, a reusable test-case structure, and a checklist.
- ISTQB terminology is used accurately and consistently.
- EP explicitly covers representative values and at least one test per identified partition.
- BVA explicitly covers boundaries and states whether 2-value or 3-value BVA is being applied.
- All source contradictions and incorrect representatives are fixed.
- All intervals have explicit endpoint ownership.
- Every representative belongs to its stated partition.
- Every boundary test has a defined `below`, `at`, or `above` position where applicable.
- Discrete increments and decimal/date/time precision are stated.
- Expected results are precise and observable.
- Invalid and malformed inputs are not incorrectly presented as boundary values.
- EP coverage is distinguished from combination coverage.
- BVA coverage is distinguished from partition coverage.
- The relationship between EP and BVA is clear.
- Assumptions, questions/TBDs, and residual risks are visible.
- No Ukrainian text, duplicated headings, stray placeholders, broken tables, or unsupported claims remain in the final English documentation.
- Markdown renders correctly and has no avoidable spelling, grammar, or formatting errors.
- The project-local `equivalence-partitioning` skill continues to reference the project guide and can use the finalized documentation when generating test cases.
