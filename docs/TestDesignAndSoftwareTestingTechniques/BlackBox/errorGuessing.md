# Error Guessing Test Design

## Scope and purpose

Error Guessing is a black-box, experience-based test-design technique. A tester uses knowledge of the product, domain, users, technology, previous failures, defect history, and common error patterns to predict where defects may occur and designs targeted checks to expose them.

The technique is useful when specifications are incomplete, time is limited, a feature has recently changed, or historical evidence points to a fragile area. It is heuristic rather than exhaustive. A passing guessed scenario does not prove that other values, paths, environments, requirements, or failure modes are correct. Error Guessing complements systematic techniques; it does not replace them.

This guide is a practical project supplement aligned with ISTQB-consistent black-box and experience-based terminology. It is not a replacement for the current official ISTQB syllabus, product requirements, risk analysis, defect-management process, or specialist security and non-functional testing.

## Status labels and scope rules

Use the following labels throughout a design:

- **Confirmed** — stated in a requirement or contract, observed product behavior, approved rule, or verified defect evidence.
- **Assumption** — introduced to make a self-contained example or provisional design possible.
- **Question/TBD** — unresolved information that requires confirmation.
- **Residual risk** — meaningful behavior or evidence outside the selected scope or not sufficiently verified.

An error hypothesis is not a defect or requirement. Keep a hypothesis unconfirmed until execution or authoritative evidence supports it. Do not silently classify unknown behavior as invalid, impossible, or a defect.

## Definition and terminology

| Term | Meaning |
| --- | --- |
| Error Guessing | An experience-based technique for predicting likely defects and designing targeted tests. |
| Error guess | An initial intuition or observed pattern suggesting where a failure may occur. |
| Defect hypothesis | A falsifiable statement about a possible defect, its trigger, and predicted observable consequence. |
| Error/mistake | A human action or decision that can introduce an imperfection. |
| Defect | An imperfection in a work product that may cause incorrect behavior. |
| Failure | An incorrect behavior observed when the software is executed. |
| Symptom | An observable indication that may point to a failure or defect. |
| Evidence source | A requirement, defect record, incident, test result, review, checklist, risk report, or knowledge source supporting a hypothesis. |
| Historical defect | A previously observed and documented defect used for targeted testing or regression. |
| Defect pattern | A recurring class of mistakes, such as missing validation, type coercion, off-by-one logic, incorrect ordering, stale data, or incomplete recovery. |
| Error-prone area | A feature, path, input, integration, rule, environment, or recent change with elevated suspected risk. |
| Checklist heuristic | A reusable list of common risks used to generate hypotheses; it is not proof that every item applies. |
| Negative/error-prone scenario | A selected case targeting invalid, unusual, malformed, boundary, failure, recovery, or historically risky behavior. |
| Regression seed | A known defect, incident, or prior failure used to create a repeatable regression check. |
| Predicted failure | The specific incorrect result a hypothesis expects if the suspected defect exists. |
| Test oracle | The exact observable result used to determine pass, fail, blocked, or unresolved status. |
| Execution evidence | A response, state, data, event, log, trace, screenshot, or other permitted artifact recorded during execution. |
| Reproducibility | The ability of another tester to recreate the setup, inputs, environment, timing, and outcome. |
| Priority/risk | The documented basis for selecting and ordering a hypothesis or case. |
| Hypothesis coverage | The proportion of selected hypotheses exercised and given a recorded result. |
| Defect yield | Findings per stated denominator, such as executed scenarios; it is not a completeness measure. |
| Residual risk | A meaningful untested, weakly evidenced, blocked, or out-of-scope possibility. |

A good hypothesis is specific and falsifiable:

> Given [setup and trigger], the system may [predicted failure] because [evidence], while the requirement expects [exact oracle].

“Something may break” and “try strange data” are not executable hypotheses until the data, context, action, expected behavior, and evidence are defined.

## Distinction from related techniques

- **Equivalence Partitioning (EP)** identifies behaviorally equivalent valid and invalid classes. Error Guessing may add an unusual or historically risky representative but does not establish complete partitions.
- **Boundary Value Analysis (BVA)** systematically targets values at and around boundaries. Error Guessing may suggest a forgotten endpoint; it does not replace boundary identification.
- **Decision Tables** enumerate condition combinations and expected actions. Error Guessing may target a missing rule or precedence error but does not prove rule completeness.
- **Pairwise or higher `t`-way testing** covers selected interactions systematically. Error Guessing may target a known problematic combination but does not provide pair or tuple coverage.
- **State-Transition Testing** models states, events, guards, and paths. Error Guessing may add duplicate, stale, terminal, retry, or recovery checks but does not prove lifecycle coverage.
- **Exploratory testing** combines learning, test design, and execution within a mission or charter. Error Guessing is a heuristic source of targeted ideas and may be used during exploratory testing, but the techniques are not synonyms.
- **Ad-hoc testing** is informal and minimally structured. Error Guessing can start informally, but reusable results should record hypotheses, oracles, evidence, and traceability.
- **Attack-based/security testing** deliberately provokes failures, especially security failures, using attacker-oriented methods. Security testing needs threat models and specialist controls; Error Guessing alone is not security assurance.
- **Risk-based testing** prioritizes testing by risk. Error Guessing supplies candidates; risk analysis may prioritize them alongside systematic cases.
- **Regression testing** checks known behavior after change. An Error Guessing case becomes a regression case when a known failure or approved regression obligation is recorded.
- **Model-based and branch/condition coverage** define explicit behavioral or structural obligations. Error Guessing has no universal denominator for all values, branches, paths, or requirements.
- **Negative testing** focuses on invalid or failure behavior. Error Guessing can generate negative tests but also targets positive-path defects such as wrong sorting, rounding, persistence, or browser behavior.

## Evidence and input model

Gather evidence before or alongside hypothesis creation. Useful sources include:

- requirements, acceptance criteria, and API contracts;
- previous release defects, production incidents, customer tickets, and support trends;
- lessons learned, retrospectives, test failures, flaky patterns, and review findings;
- risk registers, checklists, domain rules, and knowledge of similar systems;
- tester, developer, user, architecture, technology, and AUT knowledge;
- recent code or configuration changes, complex logic, high-churn or high-impact paths;
- data types, formats, boundaries, encodings, locales, time zones, and UI patterns;
- browsers, devices, integrations, queues, persistence, retries, and recovery behavior.

Every evidence source should record a stable ID, source/reference, observed fact or lesson, affected area, date or version when known, confidence and relevance, related requirement or risk, and status. Historical and checklist evidence generates candidates; it is not a universal rule that every listed risk exists in every product.

### Evidence inventory

| Evidence ID | Source/type | Observation or defect pattern | Affected area | Requirement/risk reference | Confidence/relevance | Status | Follow-up |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `EG-EVID-001` |  |  |  |  |  | Confirmed / Assumption / Question/TBD / Residual risk |  |

## Error-prone areas and hypotheses

Identify areas with complex logic, recent changes, frequent use, high business impact, fragile integrations, historical defects, unusual data, or difficult recovery. Record why an area is considered risky rather than relying on intuition alone.

### Error-prone area inventory

| Area ID | Feature/component | Why error-prone | Evidence IDs | Change/use/impact | Candidate scenarios | Priority | Status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `EG-AREA-001` |  |  |  |  |  | High/Medium/Low | Confirmed / Assumption / Question/TBD / Residual risk |

### Hypothesis inventory

| Hypothesis ID | Area ID | Suspected failure mechanism | Trigger/context | Predicted failure | Exact expected oracle | Evidence IDs | Requirement/risk references | Impact/likelihood/detectability | Priority | Scenario IDs | Result/status | Residual risk |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `EG-HYP-001` |  |  |  |  |  |  |  |  | High/Medium/Low |  | Untested / Passed-disproved / Confirmed defect / Blocked / Unresolved |  |

## Repeatable workflow

1. Define the feature, operation, scope, requirement or evidence basis, and exact oracle.
2. Identify inputs, representations, roles, states, existing data, integrations, timing, and environments.
3. Collect and classify requirements, defect history, incidents, prior failures, checklists, risks, domain knowledge, and technology patterns.
4. Identify error-prone areas and likely user or implementation mistakes.
5. Formulate falsifiable hypotheses with stable IDs, predicted failures, evidence, and status.
6. Check duplicate guesses and link existing EP, BVA, decision-table, pairwise, state, or regression cases rather than duplicating them.
7. Prioritize by documented impact, likelihood, detectability, exposure, history, change risk, confidence, and execution cost.
8. Convert selected hypotheses into minimal but complete scenarios with deterministic setup, input, context, environment, action, oracle, evidence capture, and cleanup.
9. Consider applicable missing, malformed, boundary, representation, duplicate, ordering, permission, timeout, recovery, UI, persistence, and historical-regression risks.
10. Review reproducibility: build, versions, browser/device, locale, time zone, clock, feature flags, network, random seed, external dependencies, reset, and cleanup.
11. Execute and capture actual evidence; distinguish passed/disproved, confirmed defect, blocked, unresolved, and Question/TBD outcomes.
12. Triage unexpected behavior as a defect, requirement issue, environment issue, test-data issue, or unconfirmed hypothesis. Create or link a defect only when evidence supports it.
13. Convert confirmed defects into regression seeds when appropriate and reassess related hypotheses.
14. Calculate declared metrics with explicit denominators and list gaps, exclusions, and residual risks.
15. Add complementary formal, exploratory, security, reliability, performance, accessibility, usability, or compatibility tests as needed.
16. Update an approved checklist or lessons-learned inventory from reliable evidence.

## Scenario design and exact oracles

Select categories only when supported by the feature, risk, evidence, or environment. Candidate categories include:

- missing, null, empty, blank, whitespace-only, default, and partially supplied values;
- malformed, truncated, unsupported, oversized, encoded, Unicode, or type-coerced data;
- values below, at, and above numeric, length, date, time, count, quota, or size limits;
- date, time zone, daylight-saving, locale, collation, currency, rounding, precision, and normalization;
- sorting, filtering, pagination, mixed numeric/text values, duplicate keys, and stable ordering;
- permissions, ownership, account state, expired credentials, repeated codes, replay, and unauthorized actions;
- duplicate submissions, idempotency, retries, stale callbacks, delayed or out-of-order messages;
- timeouts, network loss, cancellation, reload, restart, partial completion, rollback, and recovery;
- browser, device, viewport, rendering, keyboard, JavaScript, and compatibility differences;
- cache invalidation, synchronization, persistence, notification, audit, calculation, external invocation, and transaction behavior;
- known historical defects and regression seeds.

Every case must include the exact expected behavior. Possible oracles are HTTP status and body fields, exact validation message, accepted/rejected state, persisted value, calculated amount and precision, item order, state transition, notification, audit record, event count, external call, processing path, no duplicate side effect, or explicit recovery result.

“Works correctly,” “handles gracefully,” and “an error occurs” are not oracles. If expected failure handling is unspecified, mark it `Question/TBD`; do not infer a confirmed defect from an unexpected result without sufficient evidence.

### Executable scenario template

| Test case ID | Hypothesis ID | Evidence IDs | Objective | Requirement/risk reference | Priority | Preconditions/setup | Complete input/context | Environment/clock/locale | Steps/actions | Exact expected oracle | Evidence to capture | Cleanup/repeatability | Result/status | Defect/result ID | Technique tags | Assumptions/notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `EG-TC-001` | `EG-HYP-001` | `EG-EVID-001` |  |  | High/Medium/Low |  |  |  | 1.  2.  3.  |  |  |  | Untested / Passed / Confirmed defect / Blocked / Unresolved / Question/TBD |  | Error Guessing / EP / BVA / negative / regression |  |

## Prioritization and reproducibility

Document the prioritization method and rating scales. Consider business, safety, security, and data-loss impact; likelihood and defect history; detectability; customer exposure; recent change and complexity; environment availability; confidence in the evidence; and execution cost. Separate hypothesis priority, test-case priority, defect severity, and business risk. A numeric score is a decision aid, not objective truth.

To reduce confirmation bias, review disconfirming evidence, involve another tester or domain expert when practical, and define the oracle before inspecting the result. Record build, data, role, environment, timing, locale, feature flags, random seed, ordering, external dependencies, and reset instructions so another tester can reproduce the case.

## Execution evidence and defect follow-up

| Result ID | Test case ID | Build/environment | Execution date/time | Observed response/state/data | Evidence artifact/reference | Oracle comparison | Outcome | Defect or incident ID | Reproducibility notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `EG-RES-001` | `EG-TC-001` |  |  |  |  |  | Passed / Confirmed defect / Blocked / Unresolved / Question/TBD |  |  |

A confirmed defect should have reproducible steps, complete data and context, actual evidence, expected oracle, impact, and an appropriate defect reference. A blocked or unresolved result is not a disproved hypothesis. If the requirement is ambiguous, report a requirement question separately from a product defect.

## Coverage, gaps, and limitations

Error Guessing has no universal formal coverage denominator. Declare an inventory before execution and report separate metrics such as:

- **Selected hypothesis coverage** = executed selected hypotheses with a recorded outcome / selected executable hypotheses × 100%.
- **Scenario coverage** = executed selected scenario IDs / selected executable scenario IDs × 100%.
- **Evidence-source coverage** = selected relevant evidence sources represented by scenarios / selected relevant evidence sources × 100%.
- **Error-prone-area coverage** = selected areas exercised / selected areas × 100%.
- **Historical-regression coverage** = executed selected regression seeds / selected regression seeds × 100%.
- **Negative-category coverage** = executed selected negative/error-prone categories / selected applicable categories × 100%.
- **Risk-priority coverage** = execution reported separately for high, medium, and low priority items.
- **Reproducibility coverage** = cases with complete repeatability metadata / executed cases × 100%.
- **Defect yield** = confirmed defects / executed scenarios, with the denominator and period stated.

Also report passed/disproved hypotheses, confirmed defects, blocked cases, unresolved cases, Question/TBD items, untested high-risk items, evidence and areas not represented, historical defects not regressed, excluded categories, and residual risks. Do not combine Error Guessing, formal, exploratory, security, regression, or non-functional counts into one quality claim.

100% selected-hypothesis coverage means only that every selected hypothesis was exercised and assigned an outcome. It does not prove that all defects, values, requirements, branches, states, combinations, sequences, security properties, or non-functional characteristics are correct. Defect yield is an observation about selected checks, not an estimate of all undiscovered defects.

### Coverage and gap template

| Coverage ID | Metric | Declared denominator | Exercised numerator | Percentage/result | Uncovered/excluded items | Evidence/notes |
| --- | --- | --- | --- | --- | --- | --- |
| `EG-COV-001` | Selected hypothesis coverage |  |  |  |  |  |

### Residual-risk template

| Risk ID | Uncovered or weakly evidenced area | Reason not covered | Impact/likelihood | Complementary technique/follow-up | Status |
| --- | --- | --- | --- | --- | --- |
| `EG-RISK-001` |  |  |  |  | Residual risk / Question/TBD |

## Worked examples

The examples below are teaching models. Their product rules are **Assumptions** unless a real requirement is supplied. Each uses a small declared inventory; none claims feature-wide coverage.

### Example 1: Verification-code validation

**Requirement assumption `REQ-EG-01` (Assumption):** A six-digit code is valid for five minutes, may be used once, and returns HTTP `204` on success. The service clock is UTC at one-second precision; expiration occurs when elapsed time is `>= 5:00`, so a code at exactly `t=5:00` is expired. Missing or blank codes return HTTP `400` with `CODE_REQUIRED`; malformed codes return HTTP `400` with `CODE_INVALID`; an expired code returns HTTP `400` with `CODE_EXPIRED`; a replay returns HTTP `409` with `CODE_REPLAY`. A wrong code is initially treated as `HTTP 400 CODE_INVALID`; wrong-code attempts one and two use that response, the third wrong-code attempt returns HTTP `423` with `VERIFICATION_SUSPENDED` and starts a ten-minute suspension, and further attempts during suspension return the same `423` response. A successful verification creates one audit event and marks the account verified. The reference instant is code issuance, and no verification or audit side effect occurs for rejected attempts.

**Evidence:** `EG-EVID-01` (Assumption), a prior validation defect involving blank input; `EG-EVID-02` (Assumption), a checklist item for expiry and replay; `EG-EVID-09` (Assumption), a rate-limit checklist item for failed-attempt suspension. **Area:** `EG-AREA-01`, code validation and attempt state. **Hypotheses:**

| Hypothesis | Trigger | Predicted failure | Exact oracle | Priority |
| --- | --- | --- | --- | --- |
| `EG-HYP-01` | blank or whitespace code | validator accepts a non-code or returns a server error | HTTP `400`, body `code=CODE_REQUIRED`, no audit event, account remains unverified | High |
| `EG-HYP-02` | valid code at `t=5:00` | expiry boundary is handled incorrectly | HTTP `400`, body `code=CODE_EXPIRED`; no verification or audit event | High |
| `EG-HYP-03` | submit the valid code twice | replay creates a second verification or audit event | first request `204`; second request `409`, `code=CODE_REPLAY`; exactly one audit event | High |
| `EG-HYP-04` | three sequential wrong codes followed by a fourth attempt during suspension | failed-attempt counter or suspension precedence is wrong | first and second responses `400 CODE_INVALID`; third response `423 VERIFICATION_SUSPENDED`; account status `suspended`; fourth attempt also returns `423 VERIFICATION_SUSPENDED`; no verification or audit event | Medium |

**Setup and cases:** Create account `acct-100`, code `482731`, freeze the test clock, and use a unique request ID per submission. Submit exact blank, expiry-boundary, duplicate, and wrong-code requests. Capture response status/body, account state, audit-event count, and timestamps. Reset the account and clock after each case.

For the four selected hypotheses, 4/4 hypotheses were executed = **100% selected-hypothesis coverage** and 4/4 scenarios executed = **100% scenario coverage**. This does not cover all code formats, clocks, rate-limit races, delivery failures, roles, or security threats. If the five-minute boundary or suspension policy is not confirmed, keep the relevant result `Question/TBD`.

### Example 2: Sorting and locale handling

**Requirement assumption `REQ-EG-02` (Assumption):** A catalog API sorts `price` numerically in ascending order, preserves source order for equal prices, and uses locale `en-US` for names. Size labels use the domain order `XXS < XS < S < M < L < XL`, not lexical order. The web UI must display the API order in the declared supported browsers: Chrome 128 and Safari 18. This size ordering is an explicit teaching-model assumption, not a rule inferred from the evidence.

**Evidence:** `EG-EVID-03` (Assumption), a historical defect where strings `1`, `10`, and `2` were sorted lexicographically; `EG-EVID-04` (Assumption), a support report about size labels. **Area:** `EG-AREA-02`, catalog ordering. **Hypotheses:**

- `EG-HYP-05` predicts that numeric-looking strings are compared as text.
- `EG-HYP-06` predicts that equal-price records are reordered rather than stable.
- `EG-HYP-07` predicts that size labels are sorted alphabetically instead of by domain order.

Use data `["1", "10", "2"]`, equal-price records with IDs `A` then `B`, and sizes `["XXS", "XS", "S", "M", "L", "XL"]`. In `en-US`, expect numeric order `1, 2, 10`; equal prices retain `A, B`; size order is `XXS, XS, S, M, L, XL` under `REQ-EG-02`. Compare API JSON order and UI order in Chrome 128 and Safari 18 and capture locale, browser build, request, response, and rendered order.

Three selected hypotheses and three scenarios produce 3/3 = **100% selected-hypothesis coverage** and 3/3 = **100% scenario coverage** for this small inventory. One area and two evidence sources are represented, so area coverage is 1/1 = **100%** and evidence-source coverage is 2/2 = **100%**. This does not prove other locales, browsers, pagination, filters, nulls, dates, or all catalog data. Use EP for data classes, BVA for numeric limits, Pairwise for browser/locale combinations, and compatibility testing for the supported matrix.

### Example 3: API duplicate and timeout handling

**Requirement assumption `REQ-EG-03` (Assumption):** `POST /payments` accepts an idempotency key and creates one payment. A valid new request returns `201` with payment ID and creates one ledger entry. Repeating the same key returns the same payment ID and does not create another entry. A timeout may occur after the provider accepts the payment; the client must safely retry and eventually observe one completed payment. For the timeout fixture, the client timeout is `5s`, polling is allowed for `60s`, the provider receives exactly two calls, the first call charges once and delays its response, and the retry returns the original payment ID without another charge. The final payment status is `Succeeded`.

**Evidence:** `EG-EVID-05` (Assumption), a prior incident involving duplicate charges; `EG-EVID-06` (Assumption), an integration timeout risk; `EG-EVID-08` (Assumption), an API validation checklist item for malformed amounts. **Area:** `EG-AREA-03`, payment boundary and retry handling. **Hypotheses:** `EG-HYP-08` predicts duplicate ledger entries after a retry; `EG-HYP-09` predicts a timeout retry creates a second provider charge; `EG-HYP-11` predicts malformed amounts reach the provider or create local payment data.

Create order `ord-200`, amount `19.95`, key `idem-200`, and a provider stub matching `REQ-EG-03`. Send the request, record the client timeout at `5s`, retry with the same key, and poll for at most `60s`; then query payment status and the ledger. The exact oracle is: final payment status `Succeeded`; the same payment ID is returned for the original and retry requests; provider call count `2`; provider charge count `1`; exactly one ledger entry for `19.95`; one persisted payment record; and no duplicate notification or audit side effect. For `EG-TC-11`, submit amount `19.9x` with a new idempotency key: HTTP `400`, body `code=AMOUNT_INVALID`, provider call count `0`, payment-record count `0`, and ledger-entry count `0`.

Three selected cases (`EG-TC-08` duplicate request, `EG-TC-09` timeout retry, and `EG-TC-11` malformed input) executed with outcomes give 3/3 = **100% scenario coverage**; the three selected hypotheses `EG-HYP-08`, `EG-HYP-09`, and `EG-HYP-11` give 3/3 = **100% hypothesis coverage**. Negative coverage is 1/1 for the selected malformed category and regression-seed coverage is 1/1 for `EG-REG-01` (the duplicate-charge incident). These metrics do not cover concurrent keys, provider outages, all currencies, network partitions, or every payload representation.

### Example 4: Historical production-defect regression

**Evidence `EG-EVID-07` (Assumption):** Release `2.4.0` accepted phone input `+380991234567` only when its length was counted before normalization, returning a generic server error. The approved requirement `REQ-EG-04` (Assumption) says accepted international phone input returns HTTP `200` and is normalized to E.164; invalid input receives HTTP `400` with `PHONE_INVALID`. Define regression seed `EG-REG-01`, area `EG-AREA-04`, hypothesis `EG-HYP-10`, scenario `EG-TC-10`, and result `EG-RES-10` for this historical defect.

Traceability:

```text
EG-EVID-07 → EG-AREA-04 phone normalization → EG-HYP-10 length checked before normalization
→ EG-REG-01 historical regression seed → EG-TC-10 fixed regression input
→ EG-RES-10 observed response → regression result
```

In the fixed build, create a test account, submit `+380991234567`, and verify HTTP `200`, normalized persisted value `+380991234567`, and no server error. This is the exact oracle for `EG-HYP-10`; the expected-versus-actual comparison is recorded in `EG-RES-10`. Submit `380 99 123 45 67` as a related scenario only if the requirement confirms whitespace normalization; otherwise assign it a new `Question/TBD` hypothesis rather than treating it as part of `EG-TC-10`. Capture build, request, response, database value, and defect reference. If the real record is unavailable, all release and behavior details remain Assumptions until confirmed.

One selected regression seed executed gives 1/1 = **100% historical-regression coverage** and one selected hypothesis executed gives 1/1 = **100% hypothesis coverage**. It covers this seed only, not every phone format, country, encoding, client, or normalization defect. Add EP/BVA cases for format and length classes and compatibility cases for supported clients.

## When to use Error Guessing

Use it when specifications are incomplete or evolving, time is constrained, relevant product or domain history exists, a feature is recently changed or complex, production evidence identifies risk, or likely user/implementation mistakes deserve targeted checks. It is particularly valuable as a focused supplement after basic behavior is understood and alongside formal test design.

Do not use it as the only approach for large, complex, safety-critical, security-critical, regulated, or highly concurrent systems. It is insufficient for systematic value, boundary, combination, lifecycle, branch, security, performance, reliability, accessibility, usability, or compatibility coverage.

## Limitations and common mistakes

Error Guessing is subjective, depends on experience and domain knowledge, can miss unfamiliar defect classes, is difficult to make exhaustive, and is vulnerable to confirmation bias. Common mistakes are:

1. Treating intuition, a checklist, or an old defect as confirmed current behavior.
2. Writing a vague hypothesis without a trigger, predicted failure, evidence, or oracle.
3. Calling an unexpected result a defect without checking the requirement, environment, data, and reproducibility.
4. Omitting exact messages, status codes, persistence, state, side effects, notifications, or calculations.
5. Testing only familiar invalid values while ignoring valid-path defects.
6. Failing to distinguish missing, blank, null, malformed, unsupported, and boundary inputs.
7. Omitting build, locale, browser, clock, timing, retry, or cleanup data.
8. Duplicating formal EP, BVA, decision-table, pairwise, state, or regression cases without recording the additional heuristic rationale.
9. Counting test cases or found defects as completeness.
10. Ignoring blocked, unresolved, untested high-risk, or non-reproducible cases.
11. Failing to turn confirmed defects into regression seeds.
12. Claiming that a passing checklist or 100% selected-hypothesis metric proves quality.

## Complementary techniques

Combine Error Guessing with:

- **EP** for behaviorally distinct valid and invalid classes;
- **BVA** for numeric, length, date/time, timeout, count, quota, and size thresholds;
- **Decision Tables** for explicit condition/action combinations and precedence;
- **Pairwise** for mostly independent browser, device, locale, role, flag, and environment combinations;
- **State-Transition Testing** for lifecycle, duplicate events, retries, expiration, and recovery;
- **use-case/scenario testing** for complete actor goals and end-to-end flows;
- **condition/cause-effect analysis** for complex Boolean logic;
- **model-based testing** for generated state/path sequences;
- **risk-based testing** for selection and prioritization;
- **exploratory testing** for investigation outside the initial model;
- **attack/security testing** for threat-focused failure induction;
- **regression testing** for confirmed historical and newly discovered defects;
- **reliability, performance, accessibility, usability, and compatibility testing** for non-functional risks.

## Verification checklist

Before presenting an Error Guessing design, verify:

- [ ] Scope, requirement or evidence basis, operation, and exact oracle are documented.
- [ ] Evidence sources have stable IDs, references, relevance, confidence, and status.
- [ ] Error-prone areas and their rationale are explicit.
- [ ] Hypotheses are falsifiable, stable-IDed, evidence-linked, and distinct from defects.
- [ ] Confirmed, Assumption, Question/TBD, and Residual risk labels are used consistently.
- [ ] Historical defects and checklists generate targeted candidates rather than universal rules.
- [ ] Priority distinguishes impact, likelihood, detectability, exposure, history, change risk, confidence, and execution cost where available.
- [ ] Applicable negative, unusual, boundary, representation, duplicate, replay, retry, ordering, permission, timeout, recovery, UI, environment, persistence, and historical cases were considered.
- [ ] Every case has deterministic setup, complete data/context, environment/timing, steps, exact oracle, evidence capture, cleanup, priority, and traceability.
- [ ] Actual outcomes distinguish passed/disproved, confirmed defect, blocked, unresolved, and Question/TBD.
- [ ] Defect reports contain reproducible evidence and expected-versus-actual comparison.
- [ ] Selected hypothesis, scenario, evidence-source, area, priority, negative, regression, reproducibility, and defect-yield metrics are separate.
- [ ] Denominators, numerators, exclusions, untested items, and residual risks are visible.
- [ ] EP, BVA, Decision Tables, Pairwise, State-Transition, exploratory, security/attack, risk-based, regression, and non-functional follow-ups are identified.
- [ ] No metric or defect count is presented as proof of complete coverage.
- [ ] No vague oracle such as “the system works correctly” remains.
- [ ] Markdown tables render correctly; there are no duplicate headings, placeholders, article metadata, source-language fragments, malformed content, or stray source text.
