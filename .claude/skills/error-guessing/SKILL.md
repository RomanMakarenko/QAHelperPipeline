---
name: error-guessing
description: Apply ISTQB-aligned Error Guessing to a supplied requirement, acceptance criterion, existing test case, defect history, production issue, risk report, checklist, or feature description. Trigger when the user asks to predict likely defects, identify error-prone areas, design experience-based negative or regression scenarios, analyze historical failures, or prepare risk-focused test cases.
version: 0.1.0
---

# Error Guessing Test Design

Apply this skill when the user provides a requirement, acceptance criterion, feature description, existing test case, defect or incident history, production issue, support trend, risk report, checklist, recent change, or domain/technology context and asks to predict likely defects or design targeted tests.

Use this project's detailed guide as the primary terminology, workflow, examples, templates, and coverage reference:

`docs/TestDesignAndSoftwareTestingTechniques/BlackBox/errorGuessing.md`

If the guide is unavailable, use the rules in this skill and ISTQB-consistent black-box, experience-based terminology as a usable fallback. Never invent product behavior, requirements, defect history, states, timing, supported environments, or oracles because the supplied information is incomplete. Preserve requested Gherkin, JSON, CSV, test-management, or other output schemas.

## Scope and core principle

Treat Error Guessing as a black-box, experience-based and heuristic test-design technique. Use tester, product, domain, technology, user, defect-history, production, checklist, risk, and prior-failure knowledge to predict likely defects and design targeted checks.

An error guess is a candidate intuition or pattern. Convert it into a falsifiable defect hypothesis with an affected area, evidence basis, trigger and complete context, suspected failure mechanism, predicted failure, exact oracle, priority, status, linked requirement or risk, and reproducible scenario. A hypothesis is not a confirmed defect. A passing targeted check does not prove that the feature is correct or defect-free.

Error Guessing complements Equivalence Partitioning, Boundary Value Analysis, Decision Tables, Pairwise Testing, State-Transition Testing, exploratory, security, regression, risk-based, model-based, branch/condition, and non-functional testing. Do not report a heuristic sample as exhaustive value, branch, state, interaction, security, reliability, or non-functional coverage.

## Input and evidence contract

Accept:

- a complete or partial requirement, acceptance criterion, API contract, or user story;
- an existing test case, test failure, defect, incident, support ticket, or production symptom;
- a risk register, review checklist, lessons learned, retrospective, or prior release result;
- a feature description, recent change, complex rule, integration, data format, UI, or environment matrix;
- a request for negative, edge, regression, malformed-input, retry, timeout, duplicate, recovery, sorting, locale, browser, or risk-focused tests;
- a requested Gherkin, JSON, CSV, test-management, or other output format.

Extract when available:

- scope, operation, actors, roles, permissions, preconditions, data, and exact requirement oracle;
- evidence source, stable reference, date/build/version, observation, relevance, confidence, and status;
- error-prone area, recent change, complexity, exposure, business impact, defect pattern, and affected environment;
- input representation, null/blank/malformed behavior, boundaries, encoding, locale, time zone, clock, precision, ordering, and normalization;
- duplicate, idempotency, replay, stale/out-of-order, retry, timeout, network, reload, partial-completion, rollback, and recovery behavior;
- persistence, calculation, notification, audit, queue/event, external invocation, and no-duplicate-side-effect expectations;
- linked requirements, risks, known-defect/regression seeds, selected coverage objectives, and execution constraints.

Useful evidence sources include past releases, historical defects, production/customer/support tickets, lessons learned, code/domain/AUT knowledge, reviews/checklists, risk reports, prior failures, recent changes, complexity, user behavior, UI/data patterns, and environment differences. Record the source and strength of each hypothesis. Historical evidence and checklists generate candidates; they are not universal rules and do not establish current behavior without supporting evidence.

## Clarifications and status labels

Ask only questions that block safe scenario design, make the expected oracle unknowable, or materially change priority or feasibility. Prioritize ambiguity about scope, requirement, input validity, roles, environment, timing, retry, persistence, side effects, external dependencies, exact rejection behavior, or reproducibility.

Use these labels consistently:

- **Confirmed** — stated in a requirement or contract, observed behavior, approved rule, or verified defect evidence.
- **Assumption** — introduced for a provisional design or teaching example; confirm before production use.
- **Question/TBD** — unresolved behavior requiring confirmation.
- **Residual risk** — meaningful behavior outside selected scope, not selected, blocked, or weakly evidenced.

Never silently turn intuition into confirmed behavior, a historical pattern into a current defect, or unknown behavior into invalid/impossible behavior. If a result is unexpected but the oracle is not confirmed, keep it `Question/TBD` or `Unresolved` and explain the follow-up.

## Hypothesis and area modeling

Identify error-prone areas with stable IDs and a rationale based on evidence, complexity, change, exposure, impact, environment, or defect history. For each hypothesis, record:

- stable hypothesis and area IDs;
- evidence IDs and source strength/confidence;
- affected feature, data, path, integration, role, state, or environment;
- suspected error or failure mechanism;
- trigger, complete input, preconditions, role, timing, and context;
- predicted failure and exact expected oracle;
- requirement, risk, defect, incident, or regression-seed references;
- impact, likelihood, detectability, exposure, history, change risk, confidence, and execution cost as applicable;
- priority and selection rationale;
- linked executable scenario IDs, result IDs, status, and residual risk.

A useful hypothesis form is:

> Given [setup and trigger], the system may [predicted failure] because [evidence], while the requirement expects [exact oracle].

“Something may break” and “try strange data” are not executable hypotheses. The oracle must identify observable status, message, value, ordering, state, persistence, calculation, event, notification, external call, side-effect count, timeout, retry, rollback, or recovery behavior as applicable.

Recommended inventories:

| Area ID | Feature/component | Why error-prone | Evidence IDs | Change/use/impact | Candidate scenarios | Priority | Status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `EG-AREA-001` |  |  |  |  |  | High/Medium/Low | Confirmed / Assumption / Question/TBD / Residual risk |

| Hypothesis ID | Area ID | Suspected mechanism | Trigger/context | Predicted failure | Exact oracle | Evidence IDs | Requirement/risk references | Priority | Scenario IDs | Result/status | Residual risk |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `EG-HYP-001` | `EG-AREA-001` |  |  |  |  |  |  | High/Medium/Low |  | Untested / Passed-disproved / Confirmed defect / Blocked / Unresolved |  |

## Repeatable workflow

1. Define the scope, requirement or evidence basis, operation, actors, context, and exact oracle.
2. Collect and classify relevant requirements, defect history, incidents, checklists, risks, prior failures, domain knowledge, technology patterns, and recent changes.
3. Identify error-prone areas and record the evidence and rationale for each.
4. Formulate falsifiable hypotheses with stable IDs, predicted failures, exact oracles, priorities, statuses, and traceability.
5. Remove duplicates or link the additional heuristic rationale to existing EP, BVA, Decision Table, Pairwise, State-Transition, exploratory, or regression cases.
6. Select and rank hypotheses using a documented method based on impact, likelihood, detectability, defect history, change risk, exposure, confidence, and execution cost.
7. Map selected hypotheses to concrete missing, malformed, boundary, representation, duplicate, permission, timing, recovery, UI, environment, or historical-regression scenarios when applicable.
8. Make setup deterministic: record data, roles, permissions, build/version, browser/device, locale, encoding, time zone, clock, feature flags, timing, network, dependencies, randomness/seed, and reset/cleanup state as relevant.
9. Execute and capture actual response, state, persistence, calculation, event, notification, audit, external invocation, logs/traces, and other permitted evidence.
10. Compare observations with the exact oracle; classify the result as Passed/Disproved, Confirmed defect, Blocked, Unresolved, or Question/TBD.
11. Report defects only when expected-versus-actual evidence and reproducible steps support them; distinguish requirement ambiguity, environment failure, data issue, and product failure.
12. Convert confirmed historical or newly found defects into regression seeds when appropriate and reassess related hypotheses.
13. Calculate separately declared hypothesis, scenario, evidence-source, risk-area, priority, negative, regression, reproducibility, and defect-yield metrics with explicit denominators.
14. List uncovered hypotheses, untested high-risk areas, excluded categories, blocked and unresolved cases, assumptions, Questions/TBD, and residual risks.
15. Add complementary systematic, exploratory, security/attack, regression, reliability, performance, accessibility, usability, or compatibility testing as needed.
16. Revise hypotheses and checklists when execution or reliable new evidence shows a different failure pattern.

## Scenario selection and quality

Select categories based on applicability, evidence, requirement, risk, or environment. Do not assert that every feature needs every category. Consider, where relevant:

- missing, null, empty, blank, whitespace-only, default, and partially supplied values;
- malformed, truncated, unsupported, oversized, encoded, Unicode, normalized, or type-coerced data;
- values below, at, and above numeric, length, date, time, count, quota, or size boundaries;
- locale, collation, time zone, daylight-saving, currency, rounding, precision, formatting, and sorting;
- duplicate submissions, idempotency, retries, stale callbacks, replay, delayed/out-of-order messages, and concurrency;
- roles, permissions, ownership, expired credentials, account states, and unauthorized actions;
- timeout, network loss, cancellation, reload, restart, partial completion, rollback, and recovery;
- browser, device, viewport, rendering, keyboard, encoding, and supported-client differences;
- cache, synchronization, persistence, calculation, notification, audit, queue/event, and external-integration behavior;
- known historical defects, incidents, support patterns, and regression seeds.

Every executable case must include complete setup and data, actor/role/permission, environment/version, locale/encoding/time zone/clock/timing/network conditions where relevant, exact actions, exact expected oracle, evidence to capture, cleanup, reproducibility instructions, priority, hypothesis/evidence/requirement traceability, technique tags, and assumptions/notes. “Works correctly,” “handles gracefully,” and “an error occurs” are not exact oracles.

## Default output

When no output format is specified, produce Markdown with these sections:

```markdown
## Scope, requirement basis, and exact oracle

## Evidence, clarifications, assumptions, questions, and residual risks

## Error-prone area and hypothesis inventory

## Scenario design and prioritization

## Executable test cases

## Execution evidence and defect follow-up

## Coverage, gaps, and limitations

## Complementary techniques

## Verification checklist
```

Use stable IDs and preserve the user's requested format when one is supplied. Include a worked example only when useful; label all invented product rules as `Assumption`.

## Reusable test-case and result templates

### Executable test case

| Test case ID | Hypothesis ID | Evidence IDs | Objective | Requirement/risk reference | Priority | Preconditions/setup | Complete input/context | Environment/clock/locale | Steps/actions | Exact expected oracle | Evidence to capture | Cleanup/repeatability | Result/status | Defect/result ID | Technique tags | Assumptions/notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `EG-TC-001` | `EG-HYP-001` | `EG-EVID-001` |  |  | High/Medium/Low |  |  |  | 1.  2.  3.  |  |  |  | Untested / Passed / Confirmed defect / Blocked / Unresolved / Question/TBD |  | Error Guessing / EP / BVA / negative / regression |  |

### Execution result

| Result ID | Test case ID | Build/environment | Execution date/time | Observed response/state/data | Evidence artifact/reference | Oracle comparison | Outcome | Defect or incident ID | Reproducibility notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `EG-RES-001` | `EG-TC-001` |  |  |  |  |  | Passed / Confirmed defect / Blocked / Unresolved / Question/TBD |  |  |

### Coverage and gaps

| Coverage ID | Metric | Declared denominator | Exercised numerator | Percentage/result | Uncovered/excluded items | Evidence/notes |
| --- | --- | --- | --- | --- | --- | --- |
| `EG-COV-001` | Selected hypothesis coverage |  |  |  |  |  |

Use, when applicable:

- selected hypothesis coverage = executed selected hypotheses with a recorded outcome / selected executable hypotheses × 100%;
- scenario coverage = executed selected scenario IDs / selected executable scenario IDs × 100%;
- evidence-source coverage = relevant selected evidence sources represented / relevant selected evidence sources × 100%;
- risk-area coverage = selected risk areas exercised / selected risk areas × 100%;
- historical-regression coverage = executed selected regression seeds / selected regression seeds × 100%;
- reproducibility coverage = executed cases with complete repeatability metadata / executed cases × 100%;
- defect yield = confirmed defects / executed scenarios, with denominator and period stated.

Report high-, medium-, and low-priority execution separately. Do not combine Error Guessing metrics with formal technique or non-functional metrics. A 100% result is only a claim about the declared selected inventory, never proof of complete coverage or absence of defects.

## Complementary techniques and hand-offs

- Use **Equivalence Partitioning** for behaviorally distinct valid and invalid input classes.
- Use **Boundary Value Analysis** for numeric, length, date/time, timeout, retry, count, quota, and size thresholds.
- Use **Decision Tables** for condition/action combinations, precedence, and missing rules.
- Use **Pairwise or `t`-way testing** for selected mostly independent environment and context interactions.
- Use **State-Transition Testing** for lifecycle, terminal, duplicate, retry, expiration, and recovery behavior.
- Use **exploratory testing** for investigation and learning outside the initial hypothesis inventory.
- Use **attack-based/security testing** for threat-focused security properties; Error Guessing is not security assurance.
- Use **risk-based testing** to prioritize beyond experience-based candidates.
- Use **regression testing** for confirmed historical and newly discovered defect seeds.
- Use **model-based, branch/condition, reliability, performance, accessibility, usability, and compatibility testing** for their respective explicit obligations.

## Verification checklist

Before presenting a design or result, verify:

- [ ] Scope, requirement/evidence basis, actors, context, and exact oracle are documented.
- [ ] Evidence sources have stable IDs, references, relevance/strength, confidence, and status.
- [ ] Error-prone areas have stable IDs and evidence-based rationales.
- [ ] Hypotheses are falsifiable, distinct from defects, linked to evidence and requirements, and assigned statuses.
- [ ] `Confirmed`, `Assumption`, `Question/TBD`, and `Residual risk` labels are used consistently.
- [ ] Historical defects and checklists generate targeted candidates rather than universal rules.
- [ ] Applicable missing, malformed, boundary, format, encoding, coercion, duplicate, permission, ordering, timeout, recovery, UI/environment, persistence, and historical cases were considered.
- [ ] Every case has complete setup/data/context, role, environment, timing, actions, exact oracle, evidence capture, cleanup, priority, and traceability.
- [ ] Results distinguish Passed/Disproved, Confirmed defect, Blocked, Unresolved, and Question/TBD.
- [ ] Defect follow-up contains reproducible steps and expected-versus-actual evidence.
- [ ] Prioritization criteria and the selected method are documented.
- [ ] Metrics have stable deduplicated IDs, declared denominators, independent arithmetic, and separate categories.
- [ ] Uncovered hypotheses, high-risk areas, exclusions, blocked/unresolved cases, assumptions, Questions/TBD, and residual risks are visible.
- [ ] Complementary EP, BVA, Decision Tables, Pairwise, State-Transition, exploratory, security/attack, risk-based, regression, model-based, branch/condition, and non-functional follow-ups are identified.
- [ ] No test count, checklist pass, defect count, or 100% selected metric is presented as proof of complete quality or defect absence.
- [ ] Requested output format is preserved and no product behavior is invented.
