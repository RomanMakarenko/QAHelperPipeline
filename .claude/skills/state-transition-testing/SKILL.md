---
name: state-transition-testing
description: Apply ISTQB-aligned State-Transition Testing to a supplied requirement, acceptance criterion, lifecycle, workflow, existing state diagram, transition table, event-driven rule, retry/timeout rule, or state-transition test case. Trigger when the user asks to model states and events, derive valid or invalid transitions, review lifecycle behavior, prepare sequence/path tests, or calculate state-transition coverage.
version: 0.1.0
---

# State-Transition Testing Design

Apply this skill when the user provides a requirement, acceptance criterion, lifecycle, workflow, finite-state process, existing State & Transition Diagram, transition table, event contract, retry/timeout rule, or state-transition test case and asks to model behavior or prepare tests.

Use this project's detailed guide as the primary terminology, workflow, examples, templates, and coverage reference:

`docs/TestDesignAndSoftwareTestingTechniques/BlackBox/stateTransition.md`

If the guide is unavailable, use the rules in this skill and ISTQB-consistent black-box, specification-based terminology as a usable fallback. Never invent product states, events, guards, actions, timing, precedence, or oracles because the supplied model is incomplete. Preserve requested Gherkin, JSON, CSV, test-management, or other output schemas.

## Scope and core principle

Treat State-Transition Testing as a black-box, specification-based technique for testing how one modeled object or explicitly bounded lifecycle changes state in response to events or triggers. A State & Transition Diagram is a visual artifact; a State-Transition Model is the complete state/event/guard/action/constraint rule set; State-Transition Testing derives and executes tests from that model.

Model one domain object or lifecycle at a time. A state represents stable lifecycle behavior, invariants, available or forbidden operations, processing, data, or observable outcomes—not merely a screen, URL, button, or transient display. A transition is not automatically a test case. An executable case needs setup, complete input/context, event steps, guard evidence, action/effect, exact oracle, destination state, side effects, priority, and traceability.

## Terminology and notation

Use these definitions when the guide is unavailable:

- **Modeled object/system** — one domain entity or explicitly bounded lifecycle under test.
- **State** — a stable condition with defined invariants and available or forbidden behavior.
- **State invariant** — a property that must hold while the object is in a state.
- **Initial state** — the starting state for the declared scope.
- **Final/terminal state** — a lifecycle-complete state with no further in-scope transition expected.
- **Error/recovery state** — a failed, rejected, suspended, or recoverable condition.
- **Event/trigger** — a user, system, timer, scheduled, external, callback, or data-driven occurrence that may cause a transition.
- **Guard/condition** — a predicate that enables a transition when true.
- **Transition** — a directed source-to-destination state relation caused by an event and, where applicable, enabled by a guard.
- **Action/effect** — an observable operation such as persistence, calculation, notification, external invocation, or audit logging.
- **Self-transition/loop** — a transition whose source and destination are the same state.
- **Valid transition** — a permitted transition from a reachable state with satisfied preconditions and guards.
- **Invalid transition** — an attempted event from a reachable state that is not permitted or fails a guard and has defined rejection, no-op, error, or recovery behavior.
- **Forbidden event** — an externally possible event or payload prohibited by the current state or contract.
- **Impossible/unreachable element** — a state or transition that cannot be reached from the initial state under declared constraints; document the rationale and exclude it from valid denominators.
- **Unknown behavior** — behavior not specified well enough to classify; label it `Question/TBD`.
- **State diagram** — a graphical representation of state nodes and directed labeled transitions.
- **State-transition table** — a tabular representation of source, event, guard, action, destination, status, and oracle.
- **Transition sequence** — an ordered list of events and resulting transitions.
- **Transition pair/switch** — two transitions executed consecutively where the first destination is the second source.
- **Path** — a selected route through states and transitions from an initial or defined starting state.
- **Dead end** — a non-terminal state with no permitted outgoing transition when one is expected.
- **Test oracle** — the exact observable result used to determine pass or fail.
- **State coverage** — coverage of required reachable State IDs.
- **Valid-transition coverage** — coverage of required valid Transition IDs.
- **Event/trigger coverage** — coverage of modeled required event IDs or trigger sources.
- **Guard/condition coverage** — coverage of selected true/false guard outcomes or branches.
- **Invalid-transition coverage** — coverage of selected prohibited events or failed guards, reported separately from valid transitions.
- **Transition-pair/sequence coverage** — coverage of selected consecutive transition pairs or sequences.
- **Path coverage** — coverage of explicitly selected paths, not an implicit claim to cover every possible path.

Recommended label notation is `T-001: event | [guard] | action | Source -> Destination`. Use stable state and transition IDs, a marked initial state, terminal markers, and a legend for event, guard, action, validity, and status notation. If diagram rendering is unavailable, provide a plain-text diagram and an authoritative transition table. State whether the table is exhaustive, reduced, positive-only, or augmented-invalid; preserve guards, constraints, exact oracles, exclusions, and covered case IDs in the table.

## Input contract

Accept:

- a complete requirement or acceptance criterion;
- a workflow, lifecycle, finite-state process, API contract, or event-driven rule;
- an existing State & Transition Diagram or transition/state table;
- a user/system/timer/external/callback/scheduled/data-driven event rule;
- a role, authorization, timeout, retry, expiration, reset, reopen, recovery, or concurrency requirement;
- existing state-transition cases or a request for positive, negative, pair, sequence, path, or risk-focused tests;
- a requested output format such as Gherkin, JSON, CSV, or a test-management schema.

Extract when available:

- modeled object and lifecycle boundary;
- initial, intermediate, terminal, error, recovery, and dead-end states;
- state invariants and allowed/forbidden operations;
- event IDs, sources, payloads, ordering, idempotency, and retry behavior;
- guard predicates and role, account, data, time, environment, and dependency context;
- action/effect IDs, persistence, notifications, external calls, audit entries, and exact outcomes;
- constraint IDs and legal, forbidden, conditional, implication, mutual-exclusion, timing, retry, and composition rules;
- valid, invalid, forbidden, impossible, unreachable, and unknown transitions;
- timing clock, timezone, precision, timeout, expiration, backoff, and eventual-consistency rules;
- requirement references, priority, defect history, model version, and selected coverage scope.

Do not treat a GUI navigation map as a lifecycle model without evidence that domain behavior changes. Flag multiple objects unless the composition is explicit; model separate linked state machines or clearly defined orthogonal/composite dimensions.

## Clarifications and status labels

Ask only questions that block safe modeling or make the expected oracle unknowable. Prioritize unclear object or lifecycle boundaries, state definitions, event source, guard, destination, timing, retry limit, ordering, persistence, side effects, exact rejection behavior, or feasibility.

Use these labels:

- **Confirmed** — stated directly in the requirement, contract, approved model, or domain rule.
- **Assumption** — introduced for a provisional model or teaching example; confirm before production use.
- **Question/TBD** — unresolved behavior requiring confirmation.
- **Residual risk** — meaningful behavior outside selected scope or coverage.

Never silently convert unknown behavior into invalid or impossible behavior. Never present an Assumption as Confirmed.

## State, event, guard, action, and transition modeling

For every state record a stable ID, meaning, entry criteria, invariant, allowed events/actions, forbidden events and exact oracle, persistence/context, outgoing transitions, terminal/dead-end status, requirement reference, and status.

For every event record a stable ID, name, source (`user`, `system`, `timer`, `scheduled`, `external`, `callback`, or `data-driven`), payload, applicable and forbidden states, duplicate/idempotency behavior, ordering, retry semantics, and status.

For every guard record a stable ID, formal predicate, dependencies, true and false behavior, role/data/time/context boundaries, failed-guard result, and status. Use separate guard or transition IDs when outcomes differ materially.

For every action/effect record a stable ID, operation, calculation and precision, persistence, notification, invocation, audit, state change, dependencies, mutual exclusions, exact oracle, and status.

For every transition record a stable ID, source and destination state, event, guard or explicit unguarded status, actor/source, preconditions, action/effect, exact response and side-effect oracle, validity classification, loop/retry/timeout/terminal/recovery attribute, requirement reference, and status. A self-loop has identical source and destination IDs.

Use Equivalence Partitioning for behaviorally distinct state, role, payload, and guard classes; Boundary Value Analysis for timeout, expiration, retry-count, and other thresholds; Decision Tables for guard combinations; and Pairwise for mostly independent environment or context parameters. Do not invent elements to complete a visually attractive diagram.

## Constraints and dependencies

Before classifying feasibility or reachability, record stable Constraint IDs for legal, forbidden, conditional, implication, mutual-exclusion, role, timing, ordering, retry, and composition rules. For each constraint, state affected states/events/guards, legal or prohibited combinations, and exact observable consequences. Enumerate or partition candidate combinations and classify each as valid, invalid, forbidden, impossible/unreachable, or `Question/TBD`; document exclusion rationales. Do not treat an omitted combination as impossible without evidence.

| Constraint ID | Formal rule | Affected states/events/guards | Legal/forbidden consequence | Exact observable consequence | Requirement reference | Status |
| --- | --- | --- | --- | --- | --- | --- |
| `CONSTRAINT-1` |  |  |  |  |  | Confirmed / Assumption / Question/TBD |

Use Constraint IDs in transition inventories and tables so feasibility and reachability decisions are reproducible.

## Valid, invalid, forbidden, impossible, unreachable, and unknown behavior

- **Valid:** reachable source state, satisfied preconditions and guards, permitted transition.
- **Invalid:** an attempted event or transition from a reachable state that is not permitted or fails a guard, with defined rejection/no-op/error/recovery behavior.
- **Forbidden:** an externally possible event or payload prohibited by state or contract; selected negative tests remain separate from valid coverage.
- **Impossible/unreachable:** cannot occur from the initial state under declared constraints; document the rationale and exclude it from valid executable denominators.
- **Unknown:** unspecified behavior; label `Question/TBD` and ask when it blocks safe modeling.

Every invalid case must include reachable source state, complete event/context, violated state rule or guard ID, exact status/message/no-op/error, resulting state, persistence, notification, audit, invocation, and side-effect expectations. Include duplicate submissions, terminal-state events, stale callbacks, out-of-order events, retry after success/failure, and recovery where relevant. Do not assume a rejection has no side effects unless the requirement says so.

## Timing, retries, asynchronous events, and concurrency

Record clock source, UTC/timezone, precision, reference time, unit, boundary rule, scheduler, queue, and eventual-consistency behavior. If unspecified, label `Question/TBD` or make a clearly labeled Assumption and limit dependent cases.

Model callbacks, timers, queues, polling, delayed jobs, and provider messages as event sources. Record duplicate and idempotency behavior, attempt count, retry trigger, backoff, maximum attempts, and terminal failure. Use BVA around timeout and retry boundaries.

Do not force concurrent dimensions into an undocumented flat state. Use separate linked state machines or explicit orthogonal/composite regions, declare legal combinations, and model lock/version-conflict or competing-event oracles. Report event ordering, replay, clock skew, queue delivery, and concurrency as residual risks when not selected.

## Repeatable workflow

1. Define scope, modeled object, lifecycle boundary, requirements, setup, and exact oracle.
2. Choose one object or explicitly bounded process and flag GUI-only or mixed-object scope.
3. Identify initial, active, intermediate, terminal, error, recovery, and dead-end states.
4. Check state invariants, mutual exclusivity, behavioral distinction, exhaustiveness, and equivalent-state merging.
5. Identify events and all meaningful trigger sources.
6. Formalize guards, roles, data, time, state, environment, dependencies, and boundaries.
7. Identify actions, side effects, persistence, notifications, invocations, audits, and exact outcomes.
8. Record constraints and dependencies before feasibility/reachability classification.
9. Build diagram and authoritative transition inventory/table with stable IDs and legend.
10. Classify transitions as valid, invalid, forbidden, impossible/unreachable, or Question/TBD, with rationales.
11. Check completeness for valid/invalid events, terminal behavior, loops, retries, timeouts, and recovery.
12. Check consistency for missing IDs, ambiguous destinations, overlap, nondeterminism, dead ends, and contradictory oracles.
13. Select metrics and define finite deduplicated denominators.
14. Derive complete executable cases with source state, context, event, guards, actions, destination, oracle, priority, and traceability.
15. Exercise selected invalid, terminal, duplicate, stale, ordering, timeout, retry, recovery, and concurrency cases.
16. Compare all observed responses, state, persistence, notifications, audits, invocations, and side effects with the oracle.
17. Recalculate coverage independently using stable IDs and report gaps, exclusions, assumptions, Questions/TBD, and residual risks.
18. Revise the model when evidence shows different behavior, undocumented outcomes, or a missing state/transition.

## Default output format

When no format is specified, produce Markdown with exactly these sections:

```markdown
## Scope, modeled object, requirement basis, and exact oracle

## Clarifications, assumptions, questions, and residual risks

## State model and invariants

## Event, guard, and action model

## Constraints, dependencies, timing, retries, and composition

## State diagram and transition inventory

## Selected coverage objectives

## Executable test cases

## Coverage summary and gaps

## Uncovered risks and complementary techniques

## Verification checklist
```

Use stable IDs and keep the transition table authoritative when a diagram is unavailable or dense. Every sequence case must record all intermediate states and transition IDs, not only its final state.

## Reusable templates

### State model

| State ID | State name/meaning | Entry criteria | State invariant | Allowed events/actions | Forbidden events and exact oracle | Persistence/context | Terminal/dead-end status | Requirement reference | Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `STATE-1` |  |  |  |  |  |  |  |  | Confirmed / Assumption / Question/TBD |

### Event, guard, and action model

| Element ID | Type | Meaning/source or formal predicate | Applicable states/context | True/false or expected influence/oracle | Payload/dependencies/timing | Requirement reference | Status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `EVENT-1` | Event / Guard / Action |  |  |  |  |  | Confirmed / Assumption / Question/TBD |

### Transition inventory

| Transition ID | Source state | Event/trigger | Guard | Action/effect | Destination state | Validity/status | Exact observable oracle | Retry/timeout/loop/terminal attribute | Requirement reference | Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `TRANS-1` |  |  |  |  |  | Valid / invalid / forbidden / impossible / TBD |  |  |  | Confirmed / Assumption / Question/TBD |

### Transition table

State the orientation and whether it is exhaustive, reduced, positive-only, or augmented-invalid.

| Transition ID | Source state | Trigger/event | Guard/preconditions | Action/effect | Destination state | Valid/invalid/impossible status | Constraint IDs | Expected response/side effects | Covered cases |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `TRANS-1` |  |  |  |  |  |  |  |  |  |

### Executable test case

| Test case ID | Transition/sequence ID | Title/objective | Requirement reference | Priority | Preconditions/setup | Initial/source state | Complete input and context | Event/steps | Guard/precondition evidence | Exact expected response/action/oracle | Expected destination state | Side effects/persistence/notifications | Covered state/event/guard/action/transition IDs | Technique tags | Assumptions/notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `ST-001` | `TRANS-1` |  |  | High/Medium/Low |  |  |  | 1.  2.  3.  |  |  |  |  |  | State-Transition / EP / BVA / Decision Table / negative |  |

### Coverage and gaps

| Coverage ID | Metric | Required denominator | Exercised numerator | Percentage | Uncovered items | Exclusions and rationale | Evidence/notes |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `COV-1` | Reachable-state coverage |  |  |  |  |  |  |

### Residual risks

| Risk ID | Uncovered or weakly modeled area | Reason not covered | Impact/priority | Complementary technique or follow-up | Status |
| --- | --- | --- | --- | --- | --- |
| `RISK-1` |  |  |  |  | Residual risk / Question/TBD |

## Coverage and review rules

Report separate, deduplicated stable-ID metrics for reachable states; valid transitions; events/triggers and sources; guard true/false outcomes; selected invalid/forbidden attempts; terminal states and invariants; selected transition pairs; selected sequences/paths; timing, retry, duplicate, ordering, and concurrency scenarios; and case/row counts plus model, requirement, environment, clock, event-order, and generation metadata.

Define every denominator before execution. Exclude impossible/unreachable elements from valid denominators and list their rationales. Do not count invalid attempts in valid-transition coverage. Distinguish state visitation from state behavior, one event from that event in every relevant state, transition execution from all enabling guard/data paths, pair coverage from longer sequence coverage, path representation from path execution, and model coverage from requirement, branch, data, concurrency, and non-functional coverage.

Always list uncovered reachable states and valid transitions; untested events, guard outcomes, terminal states, pairs, sequences, and paths; impossible/unreachable elements with rationales; unselected invalid events; dead ends, overlap, nondeterminism, missing guards, and contradictory outcomes; timing/retry/duplicate/stale/order/persistence/concurrency risks; assumptions, Questions/TBD, and residual risks; and complementary techniques.

A result with 100% coverage is meaningful only relative to the declared model and metric. It does not prove every value, event sequence, guard/data combination, path, implementation branch, concurrent interleaving, requirement, or non-functional property.

## Manual verification checklist

Before presenting a result, verify:

- [ ] Scope, modeled object, lifecycle boundary, requirement basis, setup, and exact oracle are documented.
- [ ] Diagram, State-Transition Model, and testing technique are distinguished.
- [ ] One object or explicitly bounded lifecycle is modeled; concurrent dimensions have explicit composition.
- [ ] States have stable IDs, invariants, entry criteria, behaviorally distinct meanings, allowed/forbidden actions, and status labels.
- [ ] States are mutually exclusive and exhaustive for the declared scope, or gaps are visible.
- [ ] Initial, terminal, error, recovery, and dead-end states are identified where relevant.
- [ ] GUI screens and incidental actions are not incorrectly presented as domain states; equivalent states are merged or justified.
- [ ] Events have stable IDs, sources, payload/context, applicable states, ordering, duplicate, and retry semantics.
- [ ] Guards have stable IDs, predicates, true/false outcomes, dependencies, boundaries, and failed-guard oracles.
- [ ] Actions and transitions have stable IDs, exact effects, source/destination, validity, and requirement traceability.
- [ ] Constraints and dependencies have stable IDs, formal rules, affected elements, and feasibility/reachability consequences.
- [ ] User, system, timer, external, callback, scheduled, and data-driven events are included where relevant.
- [ ] Self-loops, retries, timeout, expiration, reset/reopen, recovery, duplicate, stale, out-of-order, and terminal events are considered.
- [ ] Valid, invalid, forbidden, impossible, unreachable, and unknown behavior are distinct.
- [ ] Invalid cases identify violated rules and exact state, response, persistence, notification, audit, and side-effect oracles.
- [ ] Impossible/unreachable items have exclusion rationales and are not valid denominator items.
- [ ] Clock, timezone, precision, reference time, ordering, eventual consistency, retry, and concurrency assumptions are explicit.
- [ ] Diagram notation, markers, labels, legend, table authority, and table semantics are clear.
- [ ] Cases include setup, complete context, source state, event, guard, action, exact oracle, destination, priority, and traceability.
- [ ] Sequence cases record all intermediate states and transition IDs.
- [ ] State, transition, event, guard, invalid, terminal, invariant, pair, sequence/path, and timing metrics are separate where selected.
- [ ] Coverage uses deduplicated IDs and independent arithmetic.
- [ ] Uncovered items, excluded elements, assumptions, Questions/TBD, residual risks, and complementary techniques are visible.
- [ ] EP, BVA, Decision Tables, Pairwise, scenario, condition/cause-effect, model-based, error-guessing, risk-based, exploratory, and non-functional follow-ups are considered.
- [ ] Markdown diagrams and tables render correctly; no source/article scaffolds, duplicate headings, vague oracles, or non-English text remain.
