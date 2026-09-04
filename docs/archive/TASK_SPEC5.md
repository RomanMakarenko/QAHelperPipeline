# Task Specification 5: Error Guessing Test Design

## Goal

Prepare a clear, structured, English-language documentation guide and a reusable project-local skill for **Error Guessing**.

The guide must turn the existing Error Guessing source material into a precise, practical reference for a junior QA engineer while preserving senior-QA rigor. A reader should be able to replace the examples with a real requirement, use available experience and defect evidence to formulate falsifiable defect hypotheses, derive reproducible targeted checks, execute them with exact oracles, record evidence, and report what was and was not covered.

Error Guessing is an experience-based, heuristic, black-box test-design technique. The tester predicts likely defects from knowledge of the product, domain, users, technology, development history, previous failures, and common error patterns, then designs tests aimed at exposing those defects. It is useful for finding defects that formal techniques may miss, especially under time pressure or when specifications are incomplete. It is subjective and selective: it cannot guarantee completeness, prove that a feature is defect-free, or replace systematic test-design techniques.

The guide must explicitly distinguish:

- a requirement or observed fact from a defect hypothesis;
- a predicted failure from an actual observed failure;
- an experience-based check from exploratory, ad-hoc, attack-based, security, regression, or formal coverage testing;
- a tested hypothesis from coverage of all product behavior;
- a historical defect pattern from a universal rule about the product.

The accompanying skill must make the same evidence collection, hypothesis formulation, scenario design, execution, and review workflow reusable when a future user supplies a requirement, acceptance criterion, existing test case, defect history, production issue, risk report, checklist, or partially specified feature.

The implementation must be honest about the technique's limits. Intuition is a source of a test idea, not proof of a requirement. A discovered defect does not demonstrate that all similar defects were found, and a passing guessed scenario does not demonstrate that other values, paths, environments, or failure modes are correct.

## Target files

The future implementation has two targets:

1. **Error Guessing guide**
   `docs/TestDesignAndSoftwareTestingTechniques/BlackBox/errorGuessing.md`
2. **Project-local Error Guessing skill**
   `.claude/skills/error-guessing/SKILL.md`

The current task specification is:

- `TASK_SPEC5.md`

The existing Error Guessing guide is legacy source material rather than a complete project guide. Rewrite it in place during the future implementation. It combines Russian, Ukrainian, and English fragments; an ISTQB-style definition; factor lists; article metadata and promotional text; repeated headings; a malformed `w14124124` line; and informal examples involving sorting, browsers, and phone-number formats. Preserve useful concepts, but remove non-English prose, article/site metadata, navigation or promotional filler, unsupported claims, duplicate headings, malformed content, and vague oracles. Do not claim that the source was already a complete English guide.

No README, documentation index, navigation file, or skill registry exists in the repository. Do not add or update one for this task.

The future implementation must not modify unrelated guides, skills, task specifications, IDE files, configuration files, or source files. It must not commit or push unless explicitly requested.

---

# Requirements for both deliverables

## 1. Language, structure, and audience

Both future deliverables must:

- be written entirely in English;
- use a consistent Markdown heading hierarchy;
- use correctly rendered Markdown tables;
- be readable by a junior QA engineer while maintaining precise black-box and test-design terminology;
- explain Error Guessing before presenting worked examples;
- state requirement assumptions explicitly;
- use stable IDs and preserve traceability from evidence and hypotheses to scenarios, results, and risks;
- distinguish Confirmed, Assumption, Question/TBD, and Residual risk information;
- avoid placeholders, duplicate headings, broken tables, unsupported claims, article scaffolding, and vague expected results;
- preserve a requested output format such as Gherkin, JSON, CSV, or a test-management schema when a user supplies one;
- use exact observable oracles such as response codes, validation messages, acceptance/rejection, calculated values, persisted values, state changes, notifications, invoked operations, audit records, or processing paths.

Do not state or imply that Error Guessing makes an application error-free. Do not present a tester's intuition, a common developer mistake, an old defect, or a production ticket as a confirmed current product behavior unless the supplied evidence actually establishes that behavior.

## 2. Reproducibility and executable checks

Every selected scenario must be executable and reproducible enough for another tester to repeat it. Depending on the feature, document:

- preconditions and setup data;
- complete input and context data, including missing, blank, null, malformed, or unsupported representations when applicable;
- user role, permissions, account state, lifecycle state, and existing records;
- application build, API version, browser, device, operating system, locale, encoding, time zone, feature flags, and relevant integrations;
- clock, timeout, retry, network, queue, or eventual-consistency conditions;
- random seed, generated data, ordering, or sorting configuration when applicable;
- reset, cleanup, and isolation requirements;
- action steps and the intended failure mechanism being challenged;
- exact expected response, status, message, persistence, state, side effects, notification, invocation, or audit outcome;
- actual observed result, logs or screenshots where permitted, timestamps, and defect evidence.

A test idea such as “try strange data” is not an executable case until the data, context, action, expected oracle, and evidence to capture are defined.

## 3. Status labels

Both deliverables must use these labels consistently:

- **Confirmed** — stated directly in the requirement, contract, approved rule, observed product behavior, or verified defect evidence;
- **Assumption** — introduced to make a self-contained example or provisional design possible;
- **Question/TBD** — unresolved behavior or evidence that requires confirmation;
- **Residual risk** — meaningful behavior, evidence, or scenario outside the selected scope or not sufficiently verified.

An error hypothesis is not automatically a defect. Mark its status as a hypothesis until execution or authoritative evidence confirms or disproves it. Do not silently classify unknown behavior as invalid, impossible, or a defect.

## 4. Required traceability

The guide and skill must preserve links among:

- requirement or scope ID;
- evidence-source ID and evidence strength;
- error-prone area ID;
- error-hypothesis ID;
- risk or priority ID;
- test-scenario ID;
- execution/result or defect ID;
- complementary technique and follow-up ID.

If no requirement, defect record, or other authoritative evidence is supplied, mark the basis as an Assumption or Question/TBD rather than inventing a confirmed requirement.

---

# Error Guessing requirements

## 1. Definition and ISTQB alignment

Explain Error Guessing as a black-box, specification-based and experience-based test-design technique in which the tester uses knowledge, intuition, defect history, domain understanding, and common error patterns to predict where defects may exist and designs targeted tests to reveal them.

Use ISTQB-consistent terminology and principles without inventing a version-specific syllabus section or citation. Explain that the guide is a practical project supplement, not a replacement for the current official ISTQB syllabus, product requirements, risk analysis, defect-management process, or security-testing standard.

The guide must make these principles explicit:

- Error Guessing is a heuristic for selecting valuable checks; it is not a formal proof technique.
- A useful guess should become a falsifiable hypothesis with a predicted failure, triggering conditions, and an observable oracle.
- Experience may come from the tester, domain experts, developers, support staff, users, defect history, prior releases, or analogous systems.
- Historical evidence should guide targeted checks but must not be treated as proof that the same defect exists now or that other defects cannot exist.
- A hypothesis can be confirmed, disproved, unresolved, or superseded by evidence; it must not be represented as a requirement merely because it is plausible.
- Error Guessing can be used at different test levels and during time-constrained testing, but its scope, evidence, and limitations must remain visible.
- Formal techniques should establish systematic coverage where possible; Error Guessing adds risk-focused checks for likely or previously observed failure modes.

State that the practical value of Error Guessing depends on the quality and diversity of the available experience and evidence. A tester with limited product or domain knowledge may miss important hypotheses or over-focus on familiar defects.

## 2. Required terminology

Define all of the following terms in the guide and skill:

- **Error Guessing** — the experience-based technique of predicting likely defects and designing tests to expose them;
- **Error guess** — an initial intuition or observed pattern suggesting where a failure may occur;
- **Defect hypothesis** — a falsifiable statement about a possible defect, its triggering conditions, and its predicted observable consequence;
- **Error, defect, failure, and symptom** — distinguish a human mistake or incorrect action, an imperfection in a work product, an incorrect observed behavior during execution, and an observable indication of that failure;
- **Error-prone area** — a feature, input, path, integration, rule, environment, or recently changed component with an elevated suspected defect risk;
- **Defect pattern** — a recurring class of mistakes such as type coercion, off-by-one handling, missing validation, stale data, incorrect ordering, or incomplete error recovery;
- **Evidence source** — a requirement, defect record, production ticket, test result, release lesson, checklist, review finding, risk report, domain rule, user report, or other basis for a hypothesis;
- **Historical defect** — a previously observed and documented defect that can seed a regression or related-risk hypothesis;
- **Heuristic** — an experience-based rule of thumb used to select or prioritize a test, not a guaranteed rule about implementation behavior;
- **Checklist heuristic** — a reusable list of common risks or failure modes used to prompt hypotheses;
- **Negative/error-prone scenario** — an intentionally selected case targeting invalid, unusual, malformed, boundary, failure, recovery, or historically risky behavior;
- **Regression seed** — a known defect, prior failure, or production incident used to define a repeatable regression check;
- **Expected failure/predicted consequence** — the specific incorrect behavior the hypothesis predicts, not the desired result of the feature;
- **Test oracle** — the exact observable result used to decide whether execution passes, fails, or remains unresolved;
- **Evidence of execution** — the observed response, state, data, log, event, screenshot, trace, or other permitted artifact supporting the result;
- **Reproducibility** — the degree to which another tester can recreate the setup, inputs, environment, timing, and outcome;
- **Risk/priority** — the documented basis for selecting and ordering a hypothesis or scenario, including impact, likelihood, detectability, exposure, change risk, history, and cost;
- **Hypothesis coverage** — the proportion of selected hypotheses exercised and assigned a result, not coverage of all possible defects;
- **Evidence-source coverage** — the proportion of selected relevant evidence categories or sources represented in the design;
- **Risk-area coverage** — the proportion of selected error-prone areas or risk categories exercised;
- **Defect yield** — the number or rate of actionable findings from selected checks, reported as an observation rather than a completeness measure;
- **Unresolved hypothesis** — a hypothesis for which execution or evidence is insufficient to confirm or disprove the predicted behavior;
- **Residual risk** — a meaningful untested, weakly evidenced, or out-of-scope failure possibility;
- **Status label** — Confirmed, Assumption, Question/TBD, or Residual risk as defined in this specification.

The guide must clarify that “error guessing” does not mean randomly entering arbitrary values, testing without an oracle, or replacing all structured test design with intuition.

## 3. Distinctions from related techniques

Clearly distinguish Error Guessing from:

- **Equivalence Partitioning (EP):** EP systematically identifies behaviorally equivalent valid and invalid classes and selects representatives. Error Guessing may add a historically risky or unusual value, but it does not establish complete partitions.
- **Boundary Value Analysis (BVA):** BVA targets values at and around defined boundaries. Error Guessing may suggest a boundary risk or a forgotten endpoint, but it does not replace formal boundary identification and coverage.
- **Decision Tables:** Decision Tables enumerate condition combinations and expected actions for explicit business rules. Error Guessing may target a missing rule, precedence conflict, or unusual condition, but it does not prove rule completeness.
- **Pairwise or higher `t`-way testing:** Pairwise systematically covers selected interactions among parameters. Error Guessing may target a known problematic combination, but it does not establish pair or tuple coverage.
- **State-Transition Testing:** State-Transition Testing models states, events, guards, and paths. Error Guessing may add duplicate, stale, terminal, retry, or recovery events, but it does not prove lifecycle coverage.
- **Exploratory testing:** Exploratory testing uses simultaneous learning, test design, and execution within a charter or mission. Error Guessing is a heuristic for selecting likely failure checks and may be used inside or outside an exploratory session; the two techniques are not synonyms.
- **Ad-hoc testing:** Ad-hoc testing is informal and minimally structured. Error Guessing can be informal at idea generation, but the guide requires hypotheses, oracles, evidence, and traceability for reusable results.
- **Attack-based/security testing:** Attack testing intentionally provokes failures, especially security failures, using attacker-oriented techniques. Error Guessing may identify a security hypothesis, but security testing requires appropriate threat models, authorization, safety controls, and specialized methods.
- **Risk-based testing:** Risk-based testing prioritizes testing by risk. Error Guessing supplies experience-based candidates; risk-based analysis may prioritize them and other systematic tests.
- **Regression testing:** Regression testing checks that known behavior remains correct after change. Error Guessing can create regression seeds from historical failures, but a guessed case is not automatically a regression test.
- **Model-based and branch/condition coverage:** Formal models and implementation coverage define explicit structural or behavioral obligations. Error Guessing does not provide a complete denominator for every value, branch, condition, path, or requirement.
- **Negative testing:** Negative testing focuses on invalid or failure-oriented behavior. Error Guessing may generate negative tests, but it also covers likely positive-path defects such as wrong sorting, wrong rounding, missing persistence, or incorrect browser behavior.

Error Guessing is a complement to, not a replacement for, systematic functional, security, reliability, performance, accessibility, compatibility, and usability testing.

## 4. Evidence, input contract, and clarification policy

Accept:

- a complete requirement or acceptance criterion;
- an incomplete requirement with an existing feature or build;
- existing test cases that need risk-focused expansion;
- defect reports, production tickets, support trends, incident reports, or release retrospectives;
- prior test failures, flaky behavior, code-review findings, risk reports, or checklists;
- domain rules, user workflows, API contracts, data models, UI behavior, configuration matrices, or integration descriptions;
- a request for negative, unusual, regression, exploratory, compatibility, recovery, or risk-focused cases.

Extract, when available:

- operation, feature, requirement reference, and exact observable outcomes;
- tester, domain, user, developer, architecture, technology, and AUT knowledge;
- defect history, production evidence, lessons learned, prior failures, and regression obligations;
- recently changed, complex, frequently used, high-impact, or historically unstable areas;
- inputs, representations, formats, boundaries, encodings, locale, time, state, role, environment, and existing data;
- error-prone patterns such as missing validation, type coercion, off-by-one handling, incorrect ordering, rounding, stale data, duplicate processing, partial failure, retry, timeout, or recovery gaps;
- exact valid, invalid, rejected, accepted, persisted, state, notification, invocation, and error behavior;
- priority, severity, likelihood, detectability, exposure, change risk, evidence strength, and execution cost;
- requested output format and required traceability fields.

Ask only questions that block safe scenario design or make the expected oracle unknowable. Prioritize:

- unclear feature scope, requirement basis, or target behavior;
- unknown expected response, message, calculation, persistence, state, notification, invocation, or failure handling;
- missing setup data, role, permissions, state, time, environment, or external dependency needed to reproduce a scenario;
- uncertainty about whether a historical defect is fixed, still relevant, or permitted as a regression obligation;
- ambiguous input representation, endpoint, encoding, locale, time zone, retry, timeout, ordering, or idempotency semantics;
- a hypothesis whose predicted failure cannot be observed or distinguished from an environment problem.

If clarification is not essential, proceed with explicit status labels. Never invent a product rule, historical fact, defect, or expected oracle merely to make a hypothesis sound specific.

## 5. Evidence and hypothesis modeling

Require an evidence model before or alongside hypothesis creation. For every evidence source, document:

- stable Evidence ID;
- source type and location or reference;
- concise observed fact or lesson;
- affected feature, area, input, environment, or workflow;
- date/version and relevance where known;
- evidence strength or confidence and why it is considered useful;
- related requirement, defect, risk, or release reference;
- status as Confirmed, Assumption, Question/TBD, or Residual risk;
- confidentiality and permitted evidence-capture restrictions where applicable.

Evidence sources may include past releases, historical defects, production or customer tickets, support trends, lessons learned, prior test failures, flaky-test reports, code or architecture knowledge, domain rules, user behavior, developer or reviewer knowledge, checklists, risk reports, recent changes, complex logic, data characteristics, UI patterns, browser/device differences, and known integration behavior.

Require an error-hypothesis inventory. Every hypothesis should include:

- stable Hypothesis ID such as `EG-HYP-001`;
- affected area or feature ID;
- defect pattern or guessed failure mechanism;
- triggering input, action, state, role, data, timing, environment, or sequence;
- predicted incorrect behavior or failure;
- exact expected behavior and test oracle;
- evidence-source IDs and rationale;
- requirement, defect, risk, or change references;
- impact, likelihood, detectability, exposure, change risk, evidence confidence, and execution cost where used;
- selected priority and prioritization rationale;
- linked scenario IDs and result/defect IDs;
- status and result: untested, selected, passed/disproved, failed/confirmed, blocked, or unresolved;
- assumptions, questions/TBDs, and residual risks.

A hypothesis must be falsifiable. “Something may be wrong” is not sufficient. Prefer a form such as: “Given [setup and trigger], the system may [predicted defect] because [evidence], while the requirement expects [exact oracle].”

Historical evidence and checklists must generate targeted candidates, not automatically require every listed scenario in every project. Mark each candidate as applicable, not applicable with rationale, or Question/TBD.

## 6. Repeatable Error Guessing workflow

Use this workflow in both the guide and the skill:

1. **Define scope and oracle.** Identify the feature, operation, requirement or evidence basis, risks, and exact observable outcomes.
2. **Collect and classify evidence.** Review requirements, defect history, production reports, prior failures, lessons learned, risks, checklists, domain knowledge, user behavior, recent changes, and environment information.
3. **Identify error-prone areas.** Focus on complex logic, recent changes, high-impact paths, frequent use, integrations, persistence, failure handling, unusual representations, and historically unstable behavior.
4. **Formulate falsifiable hypotheses.** State the suspected defect mechanism, triggering conditions, predicted failure, expected behavior, evidence basis, and status.
5. **Check novelty and overlap.** Merge duplicate guesses, link to existing formal tests, and avoid calling an EP, BVA, decision-table, pairwise, state, or regression case “Error Guessing” without documenting the additional heuristic rationale.
6. **Assess and prioritize risk.** Apply a documented method using impact, likelihood, detectability, exposure, history, change risk, confidence, and cost. Preserve critical low-frequency risks even when execution cost is high.
7. **Derive concrete scenarios.** Convert selected hypotheses into complete cases with deterministic setup, data, context, action, exact oracle, evidence to capture, and cleanup.
8. **Add applicable negative and error-prone checks.** Consider missing/null/blank, malformed/unsupported, boundaries, formats, encoding, coercion, duplicates, replay, idempotency, stale or out-of-order events, permissions, timeout, network, reload, partial failure, recovery, sorting, locale, browser, UI, and persistence risks when supported by the scope or evidence.
9. **Review reproducibility.** Record build, versions, environment, locale, time zone, clock, timing, random seed, feature flags, external dependencies, reset, and cleanup requirements.
10. **Execute and capture evidence.** Perform the scenario, record actual response and permitted artifacts, and separate pass, confirmed defect, blocked, and unresolved outcomes.
11. **Triage and regress.** Report confirmed defects with reproduction evidence; convert fixed historical or newly discovered risks into regression seeds when appropriate; do not treat an unresolved hypothesis as disproved.
12. **Calculate declared metrics.** Report selected hypothesis, scenario, evidence-source, area, priority/risk, regression, and negative-case metrics using explicit denominators. Do not use the number of tests or defects as a completeness claim.
13. **Add complementary coverage.** Apply EP, BVA, Decision Tables, Pairwise, State-Transition, formal regression, exploratory, security, performance, accessibility, compatibility, reliability, or usability testing where the hypothesis scope requires it.
14. **Update the learning base.** Record useful patterns in an approved checklist, defect taxonomy, or lessons-learned store, and revise the hypothesis model when evidence shows that supposedly similar cases behave differently.
15. **Report gaps and residual risks.** List untested hypotheses, weak evidence, blocked cases, excluded categories, untested environments, unresolved questions, and meaningful risks outside scope.

## 7. Scenario design and oracle rules

The guide must require scenarios to state the hypothesis being tested and the behavior that would confirm or disprove it. Where applicable, include cases for:

- missing, null, empty, blank, whitespace-only, default, and partially supplied input;
- malformed, unsupported, truncated, oversized, duplicate, encoded, Unicode, or type-coerced data;
- just-below, at, and just-above limits when a boundary hypothesis exists;
- date, time, locale, time-zone, daylight-saving, rounding, currency, numeric-string, and precision behavior;
- sorting, filtering, pagination, case sensitivity, collation, stable ordering, and mixed data types;
- permissions, ownership, account state, expired credentials, repeated codes, replay, and unauthorized operations;
- duplicate submission, retry, idempotency, stale callbacks, out-of-order events, and partial success;
- timeout, network interruption, reload, cancellation, crash recovery, queue delay, and eventual persistence;
- browser, device, operating-system, viewport, rendering, UI state, keyboard, and accessibility behavior when relevant;
- persistence, cache invalidation, synchronization, calculation, notification, audit, external invocation, and transaction rollback;
- known historical defects and regression seeds.

Do not require an item solely because it is a common checklist entry. Record why it is relevant or label it not applicable with a rationale.

Every case must define an exact oracle. Examples include:

- HTTP status and response body fields;
- exact validation message and field state;
- accepted or rejected input and resulting persisted value;
- exact calculated amount, rounding, order, or count;
- source and destination lifecycle state;
- notification, audit record, event, external invocation, or processing path;
- no state change, no duplicate side effect, or explicit retry/error response;
- exact recovery behavior after timeout, reload, or partial failure.

“Works correctly,” “handles gracefully,” “no issues,” or “the page behaves normally” is not an adequate oracle. If the requirement does not define expected failure handling, mark the result Question/TBD and do not convert an unexpected response into a confirmed defect without sufficient evidence.

## 8. Prioritization and independence

Require a documented prioritization approach. It may be qualitative or quantitative, but it must state:

- dimensions considered, such as impact, likelihood, detectability, exposure, defect history, recent change, complexity, customer harm, security or safety impact, and execution cost;
- rating scales or decision rules;
- how evidence confidence affects selection;
- tie-breaking and escalation rules;
- which high-risk cases are mandatory even if their likelihood is uncertain;
- which low-priority or duplicate cases were deferred and why;
- who or what approved assumptions and risk decisions when the project process requires it.

Do not use a numeric risk score as objective truth. Explain the inputs and retain the underlying rationale. Avoid confirmation bias by reviewing hypotheses against disconfirming evidence, involving another tester or domain expert where practical, and checking that the predicted oracle is not written after seeing the result.

## 9. Coverage, evidence, and results

Define denominators before execution and report separate metrics. At minimum require:

1. **Selected hypothesis coverage** —
   `hypotheses executed with a recorded outcome / selected executable hypotheses × 100%`.
2. **Scenario coverage** —
   `executed selected scenario IDs / selected executable scenario IDs × 100%`.
3. **Evidence-source coverage** —
   `selected relevant evidence-source IDs represented by at least one scenario / selected relevant evidence-source IDs × 100%`.
4. **Error-prone-area coverage** —
   `selected error-prone areas exercised / selected error-prone areas × 100%`.
5. **Risk-priority coverage** —
   report execution of selected high, medium, and low priority hypotheses separately; do not hide unexecuted high-risk items in an overall percentage.
6. **Negative/error-prone scenario coverage** —
   `executed selected negative/error-prone scenario IDs / selected negative/error-prone scenario IDs × 100%`, with categories reported separately where useful.
7. **Known-defect/regression coverage** —
   `executed selected regression seed IDs / selected regression seed IDs × 100%`.
8. **Oracle-result distribution** —
   counts of passed/disproved hypotheses, confirmed defects, blocked cases, unresolved cases, and Question/TBD outcomes.
9. **Defect yield** —
   report discovered defects per selected scenario, session, area, or time period only with its denominator and context; never interpret yield as completeness.
10. **Reproducibility evidence** —
    report cases with complete setup/environment/evidence metadata and list cases that could not be repeated.
11. **Metadata** —
    record product version, source/evidence versions, environment, tools, test date, tester or role, data source, clock/time zone, random seed, and selected scope as permitted.

Do not combine invalid, exploratory, regression, formal-technique, and Error Guessing counts into one claimed coverage number without preserving their separate identities. Do not claim that 100% selected-hypothesis or scenario coverage proves all defects, values, requirements, branches, states, interactions, security properties, or non-functional behavior.

Always list:

- untested selected hypotheses and scenarios;
- blocked and unresolved cases;
- evidence sources and error-prone areas not represented;
- high-risk items not executed;
- excluded or not-applicable checklist categories with rationale;
- assumptions and Question/TBD items;
- historical defects that need regression but were not executed;
- complementary techniques needed to establish systematic coverage;
- residual risks and their proposed owners or follow-up where applicable.

## 10. Required worked examples

Every worked example must be self-contained, explicitly labeled, and independently checked. It must identify requirement or evidence basis, assumptions, evidence-source IDs, error-hypothesis IDs, setup, complete input/context, steps, exact oracle, priority, result/evidence fields, and declared metric arithmetic. The examples must not present invented product behavior as Confirmed.

### Example 1: Login or verification-code validation

Use a login, one-time-code, or verification feature with an explicit assumed contract. Include hypotheses and cases for relevant combinations such as:

- missing, blank, whitespace-only, malformed, wrong, expired, and valid code;
- repeated submission, lockout/rate limit, replay, or idempotency behavior;
- incorrect response status/message, account state, audit event, or notification;
- exact accepted/rejected behavior and persistence/state oracle;
- deterministic account, code, clock, expiry, and attempt-count setup.

Show evidence such as prior validation defects, a checklist, or domain knowledge as the rationale for the guesses. Separate confirmed requirements from assumptions. Calculate selected hypothesis and scenario coverage and report unresolved expiry or security behavior separately when not specified.

### Example 2: Sorting, locale, and representation handling

Use a list or catalog with numeric-looking strings, mixed case, sizes, dates, or localized text. Include a concrete hypothesis such as lexicographic sorting of numeric values, alphabetical sorting of clothing sizes, unstable ordering of equal keys, incorrect locale collation, or browser-dependent rendering.

The example must include:

- complete data with values that expose the predicted defect;
- locale, collation, browser, and expected-order assumptions;
- exact expected sequence, tie-breaking, displayed labels, or API ordering oracle;
- stable evidence and hypothesis IDs;
- a distinction between a targeted Error Guessing scenario and systematic BVA, EP, Pairwise, or compatibility coverage;
- independently checked area, hypothesis, and scenario metrics.

### Example 3: API malformed input, duplicate, retry, or timeout

Use an API operation with an explicit assumed contract and a durable side effect. Include selected hypotheses for malformed/null input, type coercion, duplicate request, retry after timeout, partial failure, stale response, or idempotency failure where relevant.

The example must specify:

- request IDs, payloads, authentication/context, pre-existing data, and network/clock conditions;
- exact status codes, response body, persistence, side effects, event count, and retry oracle;
- how a timeout is reproduced or simulated and how eventual completion is observed;
- the distinction between a rejected request, a successful retry, a duplicate side effect, and an unresolved asynchronous result;
- separate negative, regression, and selected-hypothesis coverage arithmetic;
- residual risks for concurrency, provider behavior, or untested payload classes.

### Example 4: Historical production defect regression

Use a concrete historical defect record or an explicitly labeled assumption representing one. Show the chain:

`evidence source → error-prone area → hypothesis → regression scenario → execution result → defect or regression status`.

Include the original trigger, affected version, expected behavior, exact failure evidence, fixed-version preconditions, regression steps, exact oracle, and any related hypotheses that were deliberately not assumed to be fixed. State that reproducing the historical case covers that selected regression seed only; it does not prove that all related defects or production conditions are covered.

If no real defect record is available, label all details as Assumptions and explain what evidence would need confirmation before production use.

Every example must show at least one untested or unresolved residual risk. A passing example must not be written as proof that the whole feature is correct.

## 11. Recommended guide output structure

When no project-specific format is supplied, the guide should recommend:

1. Scope, requirement basis, and exact oracle
2. Evidence, clarifications, assumptions, questions, and residual risks
3. Error-prone area and hypothesis inventory
4. Scenario design and prioritization
5. Executable test cases
6. Execution evidence and defect follow-up
7. Coverage, gaps, and limitations
8. Complementary techniques
9. Reusable templates
10. Worked examples
11. Verification checklist

### Evidence-source table

| Evidence ID | Source type/reference | Observed fact or lesson | Affected area | Version/date | Strength/rationale | Related requirements/risks | Status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `EG-EVID-001` |  |  |  |  |  |  | Confirmed / Assumption / Question/TBD / Residual risk |

### Error-prone-area table

| Area ID | Feature/component/workflow | Suspected risk or pattern | Evidence IDs | Recent change/complexity/exposure | Applicable scenarios | Priority | Status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `EG-AREA-001` |  |  |  |  |  | High/Medium/Low | Confirmed / Assumption / Question/TBD / Residual risk |

### Hypothesis inventory

| Hypothesis ID | Area ID | Defect hypothesis/failure mechanism | Trigger and context | Predicted failure | Exact expected oracle | Evidence IDs | Requirement/risk references | Impact/likelihood/detectability | Priority | Scenario IDs | Result/status | Residual risk |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `EG-HYP-001` |  |  |  |  |  |  |  |  | High/Medium/Low |  | Untested / Passed / Confirmed defect / Blocked / Unresolved |  |

### Executable Error Guessing test-case table

| Test case ID | Hypothesis ID | Evidence IDs | Title/objective | Requirement reference | Priority | Preconditions/setup | Complete input/context | Environment/timing | Steps/actions | Exact expected result/oracle | Evidence to capture | Actual result/status | Defect/result ID | Technique tags | Assumptions/notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `EG-TC-001` | `EG-HYP-001` | `EG-EVID-001` |  |  | High/Medium/Low |  |  |  | 1.  2.  3.  |  |  |  |  | Error Guessing / EP / BVA / negative / regression |  |

### Execution and defect-evidence table

| Result ID | Test case ID | Build/environment | Execution date/time | Observed response/state/data | Evidence artifact/reference | Oracle comparison | Outcome | Defect ID or follow-up | Reproducibility notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `EG-RES-001` | `EG-TC-001` |  |  |  |  |  | Passed / Confirmed defect / Blocked / Unresolved / Question/TBD |  |  |

### Coverage and gap table

| Coverage ID | Metric | Required denominator | Exercised numerator | Percentage/result | Uncovered or excluded items | Evidence/notes |
| --- | --- | --- | --- | --- | --- | --- |
| `EG-COV-001` | Selected hypothesis coverage |  |  |  |  |  |

### Residual-risk table

| Risk ID | Uncovered or weakly evidenced area | Reason not covered | Impact/priority | Complementary technique or follow-up | Owner/status |
| --- | --- | --- | --- | --- | --- |
| `EG-RISK-001` |  |  |  |  | Residual risk / Question/TBD |

## 12. Project-local Error Guessing skill requirements

Create `.claude/skills/error-guessing/SKILL.md` following the existing project skill convention.

### 12.1 Frontmatter and guide reference

Use YAML frontmatter with:

```yaml
---
name: error-guessing
description: Apply ISTQB-aligned Error Guessing to a supplied requirement, acceptance criterion, existing test case, defect history, production issue, risk report, checklist, or feature description. Trigger when the user asks to predict likely defects, identify error-prone areas, design experience-based negative or regression scenarios, analyze historical failures, or prepare risk-focused test cases.
version: 0.1.0
---
```

Reference the detailed project guide exactly as:

`docs/TestDesignAndSoftwareTestingTechniques/BlackBox/errorGuessing.md`

The skill must state that the guide is the primary terminology, workflow, examples, templates, and coverage reference. If the guide is unavailable, provide a usable fallback based on the rules in the skill and ISTQB-consistent black-box, experience-based terminology. Do not invent product behavior because the requirement, defect history, or evidence is incomplete.

### 12.2 Skill input and behavior

The skill must:

- accept requirements, acceptance criteria, existing cases, defect reports, production tickets, prior failures, lessons learned, checklists, risk reports, domain knowledge, and feature descriptions;
- identify scope, operation, requirement/evidence basis, exact oracle, environment, and reproducibility constraints;
- collect and label evidence sources rather than treating intuition as confirmed fact;
- identify error-prone areas and model hypotheses with stable IDs independently from executable cases;
- distinguish error guesses, hypotheses, defects, failures, symptoms, results, and residual risks;
- formulate falsifiable hypotheses with predicted failure, trigger, expected behavior, exact oracle, evidence, and status;
- select and prioritize hypotheses using documented impact, likelihood, detectability, exposure, history, change risk, confidence, and execution-cost reasoning;
- consider relevant missing, null, blank, malformed, unsupported, boundary, format, encoding, coercion, duplicate, retry, stale, ordering, permission, timeout, network, reload, recovery, sorting, locale, browser, UI, persistence, and historical-regression scenarios without asserting that all apply;
- derive reproducible executable cases with complete setup, inputs, context, environment, timing, actions, exact oracle, evidence to capture, and cleanup;
- preserve requested output formats such as Gherkin, JSON, CSV, or test-management schemas;
- never invent requirements, defect history, constraints, expected behavior, or evidence;
- ask only questions that block safe design or make the oracle unknowable;
- label information as Confirmed, Assumption, Question/TBD, or Residual risk;
- separate passed/disproved hypotheses, confirmed defects, blocked cases, unresolved behavior, and Question/TBD results;
- independently verify declared metrics and list uncovered hypotheses, evidence sources, risk areas, and residual risks;
- recommend EP, BVA, Decision Tables, Pairwise, State-Transition, exploratory, security/attack, regression, and non-functional follow-ups where appropriate.

### 12.3 Default skill output

When no format is specified, the skill must produce Markdown with these sections:

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

The skill must include reusable evidence-source, error-prone-area, hypothesis, executable-case, execution/result, coverage/gap, and residual-risk templates. The executable-case template must include hypothesis and evidence IDs, requirement reference, priority, setup, complete data/context, environment/timing, steps, exact oracle, evidence to capture, actual result/status, defect/result ID, technique tags, and assumptions/notes.

### 12.4 Skill coverage and review rules

The skill must require separate reporting for:

- selected hypothesis coverage;
- selected scenario coverage;
- evidence-source coverage;
- error-prone-area coverage;
- high/medium/low priority or risk execution;
- negative/error-prone scenario coverage;
- known-defect/regression seed coverage;
- passed, disproved, confirmed-defect, blocked, unresolved, and Question/TBD outcomes;
- reproducibility metadata and missing evidence;
- defect yield with an explicit denominator when reported.

It must require explicit lists of:

- untested selected hypotheses and scenarios;
- evidence sources and areas not represented;
- high-risk items not executed;
- blocked and unresolved cases;
- excluded or not-applicable checklist categories and rationales;
- historical regressions not run;
- assumptions, Question/TBD items, and residual risks;
- formal and non-functional techniques needed to cover what Error Guessing does not establish.

It must warn that 100% selected-hypothesis or scenario coverage does not prove every defect, value, requirement, branch, condition, state, interaction, security property, or non-functional characteristic.

### 12.5 Skill manual checklist

Before presenting a result, the skill must verify:

- scope, requirement/evidence basis, operation, and exact oracle;
- evidence sources, strength, relevance, version/date, and status labels;
- stable area, hypothesis, scenario, result, defect, risk, and follow-up IDs;
- hypotheses are falsifiable and distinguish predicted failures from confirmed defects;
- no intuition, checklist item, historical pattern, or assumption is presented as a confirmed product rule without evidence;
- error-prone areas and prioritization rationale are explicit;
- applicable negative, unusual, boundary, representation, duplicate, retry, ordering, permission, timeout, recovery, UI/environment, persistence, and historical cases are considered;
- every case has deterministic preconditions, complete data/context, environment/timing, actions, exact oracle, evidence capture, and cleanup;
- expected results are precise and not vague “works correctly” statements;
- actual outcomes distinguish passed, disproved, confirmed defect, blocked, unresolved, and Question/TBD;
- selected hypothesis, scenario, evidence-source, area, priority, negative, regression, reproducibility, and defect-yield metrics are separate and arithmetically correct;
- uncovered items, excluded categories, blocked cases, assumptions, Question/TBD items, and residual risks are visible;
- EP, BVA, Decision Tables, Pairwise, State-Transition, exploratory, security/attack, risk-based, regression, and non-functional follow-ups are identified where appropriate;
- Markdown tables render correctly, with no placeholders, duplicate headings, malformed content, article metadata, stray source text, or non-English prose.

---

# Quality and acceptance criteria

The future implementation is complete only when all of the following are true:

1. `docs/TestDesignAndSoftwareTestingTechniques/BlackBox/errorGuessing.md` is a standalone English Error Guessing guide rather than a mixed-language article collection, source scrapbook, or set of blank scaffolds.
2. `.claude/skills/error-guessing/SKILL.md` exists, uses the required frontmatter, references the exact project-relative guide path, and provides reusable Error Guessing instructions and fallback behavior.
3. Both deliverables are junior-readable while preserving precise black-box, experience-based, heuristic, evidence, defect, oracle, and coverage terminology.
4. Both deliverables define Error Guessing, error guess, defect hypothesis, error-prone area, evidence source, defect pattern, checklist heuristic, negative scenario, regression seed, exact oracle, reproducibility, priority/risk, hypothesis coverage, defect yield, status labels, and residual risk.
5. The guide clearly states that Error Guessing is experience-based and heuristic, that hypotheses are not requirements, and that the technique cannot prove completeness or defect absence.
6. The guide and skill distinguish Error Guessing from EP, BVA, Decision Tables, Pairwise, State-Transition, exploratory, ad-hoc, attack/security, risk-based, regression, model-based, branch/condition, and non-functional testing.
7. Evidence inputs include relevant tester/domain/AUT knowledge, historical defects, production or customer tickets, lessons learned, prior failures, checklists, reviews, risk reports, recent changes, complexity, user behavior, UI/data patterns, and environment differences where applicable.
8. Evidence-source IDs, error-prone areas, hypothesis IDs, scenario IDs, result/defect IDs, and risk/follow-up IDs preserve traceability.
9. The workflow covers scope/oracle, evidence collection, error-prone areas, falsifiable hypotheses, overlap review, prioritization, scenario derivation, deterministic setup, execution/evidence capture, triage/regression, independent metrics, complementary coverage, learning updates, and residual-risk reporting.
10. Hypotheses include predicted failure mechanisms, triggers, exact expected behavior/oracles, evidence basis, risk/priority, status, linked cases, and results.
11. Relevant negative and error-prone categories are addressed without being asserted as universally applicable: missing/null/blank; malformed/unsupported; boundary/format/encoding/coercion; duplicate/replay/idempotency/retry; stale/out-of-order; permissions; timeout/network/reload/recovery; sorting/locale/browser/UI; persistence/transactions; and historical regression.
12. Scenarios are reproducible and include setup, complete input/context, environment/timing, action, exact oracle, evidence to capture, actual result, result status, and cleanup.
13. Prioritization documents impact, likelihood, detectability, exposure, change risk, defect history, confidence, and execution cost or clearly states which dimensions are not available.
14. The guide includes at least three self-contained worked examples and preferably four: login or code validation; sorting/locale/browser or UI behavior; API malformed/duplicate/retry/timeout behavior; and historical production-defect regression.
15. Every worked example states assumptions/evidence, stable IDs, complete setup and data, concrete steps, exact oracle, result/evidence fields, traceability, independently checked arithmetic, and at least one untested or unresolved residual risk.
16. Coverage reports selected hypotheses, scenarios, evidence sources, areas, risk priorities, negative scenarios, regression seeds, outcomes, reproducibility, and defect yield separately with explicit denominators.
17. The guide and skill do not present test count or defect yield as proof of completeness and do not claim that 100% selected-hypothesis coverage proves all values, defects, requirements, branches, states, interactions, security, reliability, or non-functional behavior.
18. Reusable templates exist for evidence, error-prone areas, hypotheses, executable scenarios, execution/defect evidence, coverage/gaps, and residual risks.
19. Limitations and common mistakes include subjectivity, confirmation bias, duplicate guesses, undocumented assumptions, vague oracles, non-reproducible environments, confusing hypotheses with defects, mistaking discovered defects for coverage, ignoring formal techniques, and overclaiming from a small sample.
20. Complementary techniques and appropriate follow-ups are visible, including EP, BVA, Decision Tables, Pairwise, State-Transition, exploratory, security/attack, risk-based, regression, reliability, performance, accessibility, compatibility, and usability testing as relevant.
21. The rewritten guide contains no Russian or Ukrainian prose, article metadata, promotional/navigation filler, duplicate headings, blank image/table scaffolds, unsupported “error-free” claims, stray `w14124124` text, placeholders, malformed tables, or vague “works correctly” oracles.
22. Only the two requested future deliverables are modified during their implementation; unrelated repository files remain unchanged.

## Required final verification for the future implementation

Before considering the future implementation complete:

- read both deliverables end-to-end;
- verify English-only content and consistent heading hierarchy;
- parse every Markdown table and confirm matching header, separator, and body column counts;
- check the skill frontmatter fields and exact guide reference;
- verify terminology distinguishes requirements, evidence, hypotheses, defects, failures, symptoms, results, and residual risks;
- verify every evidence source, area, hypothesis, scenario, result, and defect link uses stable IDs and consistent references;
- independently recalculate every worked-example hypothesis, scenario, evidence-source, area, priority, negative, regression, and other declared coverage metric;
- verify examples contain deterministic setup, complete inputs/context, environment/timing metadata, exact oracles, evidence capture, and result status;
- verify historical and checklist evidence is used as rationale rather than presented as universal product behavior;
- verify negative/error-prone scenarios are applicable, traceable, and separate from formal-technique or regression metrics where appropriate;
- inspect prioritization rationale, unresolved behavior, blocked cases, exclusions, assumptions, Question/TBD items, and residual risks;
- check that no vague oracles, unsupported claims, article metadata, duplicate headings, placeholders, source-language fragments, malformed tables, or stray source artifacts remain;
- verify the skill preserves user-requested output formats and does not invent requirements, evidence, or oracles;
- run `git diff --check`;
- inspect `git diff --name-only` and confirm that only `docs/TestDesignAndSoftwareTestingTechniques/BlackBox/errorGuessing.md` and `.claude/skills/error-guessing/SKILL.md` changed during implementation;
- report any pre-existing unrelated working-tree changes separately;
- do not commit or push unless explicitly requested.
