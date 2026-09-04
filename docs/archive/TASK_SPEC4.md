# Task Specification 4: State-Transition Testing

## Goal

Prepare a clear, structured, English-language documentation guide and a reusable project-local skill for **State-Transition Testing** (also called State & Transition Testing). The guide must turn the supplied State & Transition Diagram source article into a precise, practical test-design reference. The accompanying skill must make the same modeling, test-derivation, and coverage workflow reusable when a future user supplies a requirement, acceptance criterion, lifecycle, workflow, existing state diagram, transition table, or test case.

State-Transition Testing is a black-box, specification-based technique for testing how an object or system changes state in response to events, triggers, guards, and actions. It is useful for lifecycle behavior, event ordering, retries, timeouts, permissions, and invalid events. It does not prove that all data values, event sequences, implementation branches, concurrency interleavings, requirements, or non-functional properties are correct.

The documentation must correct the source article's informal statements where necessary. In particular, an arrow in a diagram is a modeled transition, not automatically an executable test case: a test case also needs preconditions, setup, complete inputs, event/action steps, guards, an exact oracle, and traceability.

## Target files

The future implementation has two targets:

1. **State-Transition guide**
   `docs/TestDesignAndSoftwareTestingTechniques/BlackBox/stateTransition.md`
2. **Project-local State-Transition skill**
   `.claude/skills/state-transition-testing/SKILL.md`

The State-Transition guide currently contains Russian source material rather than a complete English testing guide. Rewrite and structure it in place. Remove article metadata, promotional text, blank image/table scaffolds, and non-English prose while preserving and formalizing useful concepts from the source. Do not claim that the source was already a complete English guide.

The future implementation must not modify unrelated guides, skills, source documents, IDE files, or configuration files.

---

# Requirements for both deliverables

## 1. Language, structure, and audience

Both future deliverables must:

- be written entirely in English;
- use a consistent Markdown heading hierarchy;
- use correctly rendered Markdown tables;
- be readable by a junior QA engineer while maintaining senior-QA precision;
- explain the technique and its model before presenting examples;
- state assumptions explicitly when an example requirement is incomplete;
- use stable IDs and preserve traceability from requirements to states, events, guards, transitions, actions, tests, and results;
- avoid placeholders, duplicate headings, broken tables, unsupported claims, vague expected results, and informal article filler;
- distinguish a diagram or transition table as a model from an executable test suite;
- use precise observable oracles such as response codes, messages, accepted/rejected events, persisted state, emitted events, notifications, calculated values, audit records, invoked operations, or processing paths.

## 2. Shared quality requirements

The guide and skill must explain:

- what State-Transition Testing is;
- why it is used and what lifecycle/event-order risks it addresses;
- how to model states, events, guards, transitions, and actions;
- how to distinguish valid, invalid, forbidden, and unreachable behavior;
- how to derive executable test cases from a transition model;
- how to calculate state, transition, event, guard, invalid-transition, sequence, and path coverage;
- when the technique is appropriate;
- when it is insufficient;
- how to combine it with other test-design techniques;
- how to record assumptions, questions/TBDs, residual risks, and model scope.

Every worked example must include:

- an explicit requirement assumption or confirmed requirement basis;
- a clearly identified modeled object or lifecycle;
- stable state, event, guard, action, transition, constraint, and test-case IDs as applicable;
- initial, intermediate, terminal, error, and unreachable states where relevant;
- preconditions and setup data;
- complete input/context data;
- event or trigger actions;
- guard conditions and their status;
- exact source-state and destination-state expectations;
- exact observable response, persistence, notification, invocation, or error oracles;
- traceability to the requirement and model elements;
- independently checked coverage arithmetic;
- visible assumptions, Question/TBD items, and residual risks.

The guide must include:

- reusable state-model, transition-inventory, transition-table, test-case, coverage/gap, and residual-risk templates;
- a practical checklist for reviewing a State-Transition design;
- limitations and common mistakes;
- explicit labels for **Confirmed**, **Assumption**, **Question/TBD**, and **Residual risk**;
- a warning that 100% of a declared state or transition metric does not prove all values, all event sequences, all paths, all implementation branches, all concurrency interleavings, or all non-functional properties;
- guidance for both a diagram representation and a tabular representation, with a fallback when diagram rendering is unavailable.

---

# State-Transition Testing requirements

## 1. Definition and ISTQB alignment

Explain State-Transition Testing as a black-box, specification-based test-design technique in which the behavior of an object or system is represented as states and transitions caused by events or triggers. Explain that tests exercise states, valid and invalid transitions, guards, event sequences, and observable outcomes.

Use ISTQB-consistent terminology and principles without inventing a version-specific syllabus section or citation. State that the guide is a practical project supplement and not a replacement for the current official ISTQB syllabus, product requirements, workflow rules, or domain-specific constraints.

Clarify the relationship between the names:

- **State & Transition Diagram** is a visual modeling artifact.
- **State-Transition Model** is the states, events, guards, actions, and transition rules represented by a diagram, table, or other notation.
- **State-Transition Testing** is the test-design technique that derives and executes tests from that model.

Do not repeat the source article's claim that every arrow is already a test case. A transition is a model element; it becomes an executable test case only after setup, input/context, event steps, guard conditions, oracle, priority, and traceability are defined.

## 2. Required terminology

Define all of the following terms in the guide and skill:

- **Modeled object/system** — the single domain entity or bounded lifecycle being modeled, such as an order, ticket, payment, session, job, document, or account process;
- **State** — a condition of the modeled object in which a defined set of events/actions is available or unavailable and specified behavior is expected to remain stable;
- **State invariant** — a property that must hold while the object is in a state;
- **Initial state** — the starting state for the declared test scope;
- **Final/terminal state** — a state from which no further in-scope transitions are expected, or whose lifecycle is complete;
- **Error/recovery state** — a state representing failed processing, rejection, suspension, or a defined recovery condition;
- **Event/trigger** — a user action, system action, timer, scheduled job, callback, external message, data change, or other occurrence that may cause a transition;
- **Guard/condition** — a predicate that must be true for a transition to be enabled;
- **Transition** — a directed relation from a source state to a destination state caused by an event and, where applicable, enabled by a guard;
- **Action/effect** — an observable operation performed during a transition, such as persistence, calculation, notification, external invocation, or audit logging;
- **Self-transition/loop** — a transition whose source and destination are the same state;
- **Valid transition** — a transition permitted by the requirements under its preconditions and guard;
- **Invalid transition** — an event or attempted transition that is not allowed from the current state or fails its guard and has specified rejection, no-op, error, or recovery behavior;
- **Forbidden event** — an event that can be supplied or occur but is prohibited in the current state or context;
- **Unreachable/impossible state or transition** — a model element that cannot be reached under the stated initial conditions and constraints; it must be documented and not silently counted as an uncovered executable behavior;
- **State-transition table** — a tabular representation of source state, event, guard, action, destination state, and oracle;
- **State diagram** — a graphical representation of state nodes and directed, labeled transitions;
- **Transition sequence** — an ordered list of events and resulting transitions;
- **Transition pair / switch** — two transitions executed consecutively where the destination of the first is the source of the second;
- **Path** — a selected route through states and transitions from an initial state or a defined starting state;
- **Dead end** — a non-terminal state with no permitted outgoing transition when the model expects one, or a state requiring explicit handling;
- **Test oracle** — the exact observable result used to determine pass or fail;
- **State coverage** — coverage of required reachable state IDs;
- **Transition coverage** — coverage of required valid transition IDs;
- **Event/trigger coverage** — coverage of modeled event types or trigger sources;
- **Guard/condition coverage** — coverage of guard outcomes or branches when the model declares them as coverage obligations;
- **Invalid-transition coverage** — coverage of intentionally selected prohibited events or failed guards, reported separately from valid transition coverage;
- **Transition-pair/sequence coverage** — coverage of selected consecutive transition pairs or sequences;
- **Path coverage** — coverage of explicitly selected paths, not an implicit claim to cover every possible path.

State explicitly that a state is not merely a screen, URL, button, or transient GUI display. A state should represent externally meaningful lifecycle behavior, invariants, and available/blocked operations. A GUI page may be a way to reach or observe a state, but a collection of pages is not automatically a state-transition model.

## 3. Core modeling principles

Require the guide and skill to apply these principles:

1. **Select one modeled object or bounded lifecycle at a time.** Prefer a domain object or process entity with a meaningful lifecycle, such as an order, ticket, payment, session, or job. If several objects interact, model each object separately and document the interaction or use another technique for the cross-object rule.
2. **Define states by behavior and invariants.** States must represent materially different available actions, restrictions, data, processing, or observable outcomes.
3. **Make states mutually exclusive for the declared model.** An object must not silently be in two ordinary states at once. If concurrent or orthogonal dimensions are required, model them explicitly as separate regions or linked state machines and state the composition rules.
4. **Cover the relevant domain.** For the declared scope, states should be collectively exhaustive or any unknown/unmodeled region must be labeled as a gap, Question/TBD, or Residual risk.
5. **Do not create a new state for every incidental action.** Searching for an object, opening a GUI page, entering an unchanged form value, moving a pin, or viewing a record is not a new object state unless the object's lifecycle behavior changes.
6. **Merge behaviorally equivalent states.** States that expose the same relevant actions, restrictions, invariants, transitions, and oracles should be merged unless a documented requirement distinguishes them.
7. **Label every transition.** Show the event/trigger and, where relevant, guard, action, actor/source, and destination state.
8. **Include all meaningful trigger sources.** User, system, timer, scheduled job, external service, callback, and data-driven triggers must be modeled when they can change behavior.
9. **Distinguish no-op, rejection, error, and recovery.** A prohibited event that leaves the state unchanged is not the same oracle as an event that returns an error, moves to an error state, or schedules recovery.
10. **Model state-dependent event behavior.** The same event may be valid in one state, invalid in another, or guarded by role, data, time, or account context.
11. **Model initial, terminal, retry, timeout, and recovery behavior where in scope.** Do not stop at the happy-path states.
12. **Keep diagrams reviewable.** If a map becomes dense, split it into linked submodels with clear scope and references. Do not remove important transitions solely to make a diagram smaller.
13. **Prioritize meaningful transitions.** Do not model every browser close, network outage, or implementation detail unless the requirement makes it a relevant state or event risk. Record excluded environmental failures as residual risks or cover them with a dedicated reliability model.

## 4. Diagram and transition-table notation

The guide must explain and use a notation that can be reviewed without a proprietary drawing tool:

- rounded rectangles or clearly labeled nodes for states;
- directed arrows or rows for transitions;
- arrow/row labels containing event/trigger and optional guard/action;
- a clearly marked initial state;
- clearly marked terminal/final states;
- stable IDs for every state and transition;
- a legend for event, guard, action, invalid transition, and status notation;
- a transition table as the authoritative fallback when a diagram is unavailable or too dense.

A recommended transition label format is:

```text
T-001: submit | [valid data] | persist order | Draft → Submitted
```

The implementation may use Mermaid or another plain-text diagram notation, but it must also provide enough tabular information to preserve all model semantics. Do not rely on an image placeholder or an unrendered external diagram as the only source of truth.

The guide must require the author to state whether the table is exhaustive for the declared event/state domain, reduced, positive-only, or augmented with selected invalid events.

## 5. State, event, guard, action, and dependency model

For every state, document:

- stable State ID;
- semantic meaning and requirement reference;
- entry criteria and state invariant;
- allowed events/actions;
- forbidden or invalid events and their expected behavior;
- data/persistence expectations while in the state;
- outgoing transitions and terminal/dead-end status;
- role, account, locale, time, environment, or existing-data context;
- status as Confirmed, Assumption, or Question/TBD.

For every event/trigger, document:

- stable Event ID;
- event name and source (user/system/timer/external/data-driven);
- representation and required payload where applicable;
- states in which it is available or forbidden;
- idempotency, duplicate-delivery, ordering, and retry assumptions;
- status as Confirmed, Assumption, or Question/TBD.

For every guard, document:

- stable Guard ID;
- formal predicate and all input/context dependencies;
- true and false behavior;
- role, state, time zone, reference time, data, and boundary assumptions;
- whether a failed guard rejects, no-ops, retries, or enters an error/recovery state;
- status as Confirmed, Assumption, or Question/TBD.

For every transition, document:

- stable Transition ID;
- source state and destination state;
- trigger/event ID;
- guard ID or explicit unguarded status;
- actor/source and preconditions;
- action/effect and exact oracle;
- valid, invalid, forbidden, impossible, or Question/TBD classification;
- terminal, loop, retry, timeout, or recovery attributes;
- requirement reference and status.

For every action/effect, document:

- stable Action ID;
- operation, calculation, persistence, notification, external invocation, audit record, or state change;
- exact expected output and precision where applicable;
- dependencies and mutual exclusions;
- status as Confirmed, Assumption, or Question/TBD.

Use EP to identify behaviorally distinct input and context classes, BVA for threshold or timeout values, Decision Tables for guard/action combinations, and Pairwise for mostly independent environmental parameters. Do not invent a state or event merely to fill a diagram.

## 6. Valid, invalid, forbidden, and unreachable behavior

The guide and skill must distinguish:

- **Legal/valid transition:** executable from a reachable source state with all guards and preconditions satisfied.
- **Invalid event/transition:** an event that can be attempted in a reachable state but is not permitted; it needs a defined rejection, no-op, message, status, audit, or state outcome when in scope.
- **Forbidden external input:** a payload or event that violates a contract and may require a separate negative test with an exact validation oracle.
- **Impossible/unreachable transition:** cannot occur under the declared initial state and constraints; document the rationale and do not count it as an uncovered valid transition.
- **Missing/unknown behavior:** not safe to classify as invalid or impossible; label it Question/TBD and ask for clarification when it blocks the model.

Require an invalid-transition test to include:

- the reachable source state;
- the attempted event and complete context/data;
- the violated state rule or guard ID;
- the exact response/status/message or no-op behavior;
- the expected state after the attempt;
- persistence, notification, audit, and side-effect expectations;
- separate reporting from valid transition coverage.

Require explicit handling for duplicate events, repeated submissions, stale callbacks, out-of-order messages, retry after success, retry after failure, and events received in a terminal state when these risks are relevant.

## 7. Context, timing, asynchronous behavior, and composition

The guide must explain how to model behavior that depends on:

- user role or authorization;
- account, order, ticket, payment, or session data;
- current lifecycle state and event history;
- timeouts, expiration, schedules, timers, time zones, daylight-saving behavior, and reference clocks;
- asynchronous callbacks, queues, polling, delayed jobs, and eventual persistence;
- duplicate or out-of-order external events;
- retries, backoff, maximum attempts, and terminal failure;
- reset, restart, reopen, cancel, resume, and recovery paths;
- concurrency, locking, optimistic version conflicts, and competing events;
- multiple independent state dimensions or orthogonal regions.

If the system has concurrent state dimensions, do not falsely force them into one mutually exclusive flat state without documenting the Cartesian/composition rule. Prefer separate linked models or explicit composite-state notation, and state which combinations are legal.

If the requirement does not define timing precision, clock source, timezone, event ordering, or eventual-consistency guarantees, record a Question/TBD and only provide cases that do not depend on the unknown behavior, or label a provisional choice as an Assumption.

## 8. Input and clarification policy

Accept:

- a complete requirement or acceptance criterion;
- an existing State & Transition Diagram;
- an existing transition/state table;
- a workflow, lifecycle, finite-state process, API contract, or event-driven rule;
- an existing test case or sequence needing state-transition review;
- a role-, timer-, retry-, timeout-, or asynchronous-event requirement;
- a request for positive, negative, transition-pair, sequence, path, or risk-focused tests.

Extract, when available:

- modeled object and lifecycle scope;
- initial, intermediate, terminal, error, and recovery states;
- state invariants and available/forbidden operations;
- events/triggers and their sources;
- guards, actions, side effects, and exact outcomes;
- valid, invalid, forbidden, and impossible transitions;
- event ordering, idempotency, retries, timeouts, and concurrency rules;
- role, account, data, locale, timezone, environment, and reference-clock context;
- requirement references, priority, severity, and known defect history;
- desired coverage scope and whether a model is exhaustive or reduced.

Ask only questions that block safe modeling or make the expected oracle unknowable. Prioritize:

- unclear modeled object or mixing of multiple objects;
- ambiguous state boundaries or state invariants;
- missing initial or terminal-state definition;
- missing event source, event ordering, or trigger semantics;
- unknown guard, role, time, data, or context conditions;
- unclear invalid-event behavior, no-op versus rejection, or side effects;
- unknown timeout, retry, duplicate, stale, out-of-order, or concurrency behavior;
- unclear destination state, persistence, response, notification, or exact oracle;
- unclear time zone, reference clock, precision, or asynchronous completion semantics;
- uncertain whether a state or transition is unreachable or simply unmodeled.

Use these labels:

- **Confirmed** — stated directly in the requirement, contract, approved model, or domain rule;
- **Assumption** — introduced to make a self-contained example or provisional model possible;
- **Question/TBD** — unresolved and requiring confirmation;
- **Residual risk** — a meaningful behavior outside the selected model or coverage.

Never present invented states, events, guards, actions, precedence, timing, or oracles as Confirmed. Do not silently convert an unknown transition into an invalid or impossible transition.

## 9. Repeatable State-Transition workflow

Use this workflow in both the guide and the skill:

1. **Define scope and oracle.** Identify the operation, modeled object, lifecycle boundary, requirement references, preconditions, and exact observable outcomes.
2. **Choose one object or bounded process.** Reject GUI-only, multi-object, or unrelated-scope models unless the composition is explicit and justified.
3. **Identify states.** Extract initial, active, intermediate, terminal, error, recovery, and dead-end states from behavior, invariants, available actions, and restrictions.
4. **Check state semantics.** Ensure states are non-overlapping and behaviorally distinct; merge equivalent states and record unresolved gaps.
5. **Identify events and sources.** List user, system, timer, external, callback, scheduled, and data-driven triggers that can change the modeled state.
6. **Identify guards and context.** Formalize role, data, state, time, account, environment, dependency, and boundary conditions that enable or block each event.
7. **Identify actions and oracles.** Record state changes, status/messages, persistence, calculations, notifications, external invocations, audit entries, and processing paths.
8. **Build the state diagram and transition inventory.** Assign stable State, Event, Guard, Action, and Transition IDs. State the diagram notation and table authority.
9. **Classify feasibility.** Mark each modeled transition as valid, invalid, forbidden, impossible/unreachable, or Question/TBD. Document constraints and exclusion rationales.
10. **Check completeness.** For every reachable state, review relevant events, valid paths, invalid events, terminal behavior, loops, timeouts, retries, and recovery. Record intentional omissions.
11. **Check consistency.** Detect duplicate states, ambiguous destinations, conflicting transitions, missing guards, overlapping transitions, nondeterminism, dead ends, unreachable elements, and mismatched actions/oracles.
12. **Select coverage objectives.** Explicitly choose state, valid-transition, event, guard/outcome, selected-invalid, terminal, transition-pair/sequence, path, or exhaustive scope. Define every denominator.
13. **Derive executable cases.** Add setup, complete input/context, source state, event, guard, action, steps, expected response/side effects, destination state, priority, and traceability.
14. **Test invalid and exceptional behavior.** Separately exercise selected forbidden events, failed guards, duplicate/out-of-order events, retries, timeout, terminal-state events, and recovery behavior when relevant.
15. **Execute and compare with the oracle.** Verify source state, accepted/rejected event, action/effect, persisted state, destination state, response, notification, audit, and side effects.
16. **Calculate coverage independently.** Recalculate state, valid-transition, event, guard, invalid, terminal, sequence/pair, and selected-path metrics using stable deduplicated IDs.
17. **Report gaps and risks.** List uncovered states/transitions/events/guards/sequences, unreachable elements, untested timing/concurrency, assumptions, Question/TBD items, and residual risks.
18. **Revise from evidence.** If supposedly equivalent states behave differently or an event has multiple undocumented outcomes, split the state/transition, document the defect, and update the model.

## 10. Coverage requirements

The guide and skill must report coverage metrics separately and define their denominators. At minimum require:

1. **Reachable-state coverage** —
   `visited required reachable State IDs / total required reachable State IDs × 100%`.
2. **Valid-transition coverage** —
   `exercised required valid Transition IDs / total required valid Transition IDs × 100%`.
3. **Event/trigger coverage** —
   `exercised required Event IDs / total modeled required Event IDs × 100%`, with trigger sources separated where useful.
4. **Guard/condition outcome coverage** —
   `exercised required guard outcomes or branches / total required guard outcomes or branches × 100%`, only when guards are modeled as coverage obligations.
5. **Selected invalid-transition coverage** —
   `exercised selected invalid/forbidden transition attempts / selected invalid/forbidden attempts × 100%`, reported separately from valid transitions.
6. **Terminal-state coverage** —
   `visited required terminal State IDs / total required terminal State IDs × 100%`.
7. **Transition-pair or switch coverage** —
   `exercised required consecutive Transition-ID pairs / total selected required pairs × 100%`, only when pair scope is explicitly selected.
8. **Sequence/path coverage** —
   `exercised selected required sequence or path IDs / total selected sequence or path IDs × 100%`; never imply all possible paths unless exhaustive path coverage is actually feasible and attempted.
9. **State-invariant coverage** — when invariants or state-specific observable properties are declared, report the exercised invariants separately from state visitation.
10. **Timing/retry/concurrency coverage** — report selected timeout, retry, duplicate, ordering, or interleaving scenarios separately when they are in scope.
11. **Model and execution metadata** — record model version, source requirement version, generated/manual method, tool/version if used, environment, clock configuration, event ordering, selected scope, and excluded elements.

The guide must distinguish:

- visiting a state from verifying all behavior in that state;
- exercising a transition from covering every guard/data/role path that can enable it;
- covering a single event from covering the event in every relevant source state;
- covering transition pairs from covering longer sequences;
- representing a path from executing it;
- valid coverage from selected invalid-transition coverage;
- model coverage from requirement, branch, data, concurrency, and non-functional coverage.

Do not count impossible or unreachable elements as uncovered required executable behavior. List them separately with their rationale. If a transition has multiple materially different guards or outcomes, split it into stable transition or guard IDs rather than hiding the distinction in one coverage item.

Always list:

- uncovered reachable states and valid transitions;
- untested events, guard outcomes, terminal states, transition pairs, sequences, or paths;
- excluded/impossible/unreachable states and transitions with rationale;
- invalid events not selected and why;
- timing, retry, duplicate, ordering, concurrency, and persistence risks;
- assumptions, Question/TBD items, and residual risks;
- complementary techniques required for behavior outside the model.

A passing suite with 100% declared transition coverage is not proof that all values, all state sequences, all guards, all event orderings, all implementation branches, all concurrent interleavings, all requirements, or any non-functional property is correct.

## 11. Recommended output structure

When no project-specific format is supplied, use:

1. Scope, modeled object, requirement basis, and exact oracle
2. Clarifications, assumptions, questions, and residual risks
3. State model and invariants
4. Event/trigger, guard, and action model
5. Constraints, dependencies, timing, retries, and composition
6. State diagram and transition inventory/table
7. Selected coverage objectives
8. Executable test cases
9. Coverage summary and gap inventory
10. Uncovered risks and complementary techniques
11. Verification checklist

### State model table

| State ID | State name/meaning | Entry criteria | State invariant | Allowed events/actions | Forbidden events and oracle | Terminal/dead-end status | Requirement reference | Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `STATE-1` |  |  |  |  |  |  |  | Confirmed / Assumption / Question/TBD |

### Event, guard, and action model

| Element ID | Type | Meaning/source or formal predicate | Applicable states/context | Expected influence/oracle | Dependencies/timing | Requirement reference | Status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `EVENT-1` | Event / Guard / Action |  |  |  |  |  | Confirmed / Assumption / Question/TBD |

### Transition inventory

| Transition ID | Source state | Event/trigger | Guard | Action/effect | Destination state | Validity/status | Exact observable oracle | Requirement reference | Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `TRANS-1` |  |  |  |  |  | Valid / invalid / forbidden / impossible / TBD |  |  | Confirmed / Assumption / Question/TBD |

### Transition table

State the orientation and notation before the table. A row-oriented execution-friendly table is:

| Transition ID | Source state | Trigger/event | Guard/preconditions | Action/effect | Destination state | Valid/invalid/impossible status | Constraint IDs | Expected response/side effects | Covered cases |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `TRANS-1` |  |  |  |  |  |  |  |  |  |

A conventional diagram or column-oriented table may be used instead, but all IDs and meanings must remain unambiguous. A self-transition must show the same source and destination state. An invalid event must show the expected state after the attempt, not merely “not allowed.”

### Executable State-Transition test case

| Test case ID | Transition/sequence ID | Title/objective | Requirement reference | Priority | Preconditions/setup | Initial/source state | Complete input and context | Event/steps | Guard/precondition evidence | Exact expected response/action/oracle | Expected destination state | Side effects/persistence/notifications | Covered state/event/guard IDs | Technique tags | Assumptions/notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `ST-001` | `TRANS-1` |  |  | High/Medium/Low |  |  |  | 1.  2.  3.  |  |  |  |  |  | State-Transition / EP / BVA / Decision Table / negative |  |

Every positive case must begin from a reachable state and use a valid, complete context. Every invalid case must identify the violated state rule or guard and exact rejection/no-op/error oracle. For a sequence case, record every intermediate state and transition ID, not only the final state.

### Coverage and gap table

| Coverage ID | Metric | Required denominator | Exercised numerator | Percentage | Exclusions/uncovered items | Evidence/notes |
| --- | --- | --- | --- | --- | --- | --- |
| `COV-1` | Reachable-state coverage |  |  |  |  |  |

### Residual-risk inventory

| Risk ID | Uncovered or weakly modeled area | Reason not covered | Impact/priority | Complementary technique or follow-up | Status |
| --- | --- | --- | --- | --- | --- |
| `RISK-1` |  |  |  |  | Residual risk / Question/TBD |

## 12. Required worked examples

Every example must be self-contained, explicitly labeled, and independently checked. The implementation may use Mermaid or a text diagram, but the transition table and executable cases must remain authoritative.

### Example 1: Order or ticket lifecycle

Use a realistic single object, such as an order or support ticket. Include at least:

- an initial state such as `Draft` or `New`;
- processing states such as `Submitted`, `In Progress`, or `Awaiting Payment`;
- a successful terminal state such as `Completed` or `Resolved`;
- a cancellation or closure terminal state;
- at least one invalid event from a reachable state;
- a user-triggered and a system-triggered transition;
- stable state and transition IDs;
- a state diagram and transition table;
- executable cases for the happy path, alternate path, terminal-state event, and invalid event;
- exact status/message/persistence/destination-state oracles;
- state and valid-transition coverage arithmetic;
- a statement of which sequences, values, roles, and non-functional risks remain outside scope.

The example must show that a state is defined by lifecycle behavior and allowed actions rather than by pages such as “form open” or “results screen.”

### Example 2: Guarded role- or context-dependent workflow

Use an approval, password-reset, ticket, document, or access workflow with explicit guards. Include at least:

- two or more roles or context values;
- a guard such as ownership, approval authority, token validity, account state, or expiration;
- valid transitions when the guard is true;
- invalid or forbidden event attempts when the guard is false;
- exact rejection/status/message and no-state-change or recovery oracle;
- a transition table with guard IDs and constraint IDs;
- at least one state in which the same event is valid for one role/context and invalid for another;
- preconditions, complete data, event actions, destination states, and traceability;
- separate valid-transition, guard-outcome, selected-invalid, and state coverage arithmetic;
- an explicit note that EP/BVA or Decision Tables may be required for guard input partitions and combinations.

Do not use an unspecified role or invented authorization policy as Confirmed behavior. Label the example rules as Assumptions unless supplied by a requirement.

### Example 3: Asynchronous payment, session, or job with timeout and retry

Use an event-driven lifecycle such as payment processing, an expiring session, or a background job. Include at least:

- asynchronous system/external events;
- `Pending`/processing behavior;
- success and terminal failure states;
- a timeout or expiration event with a stated clock, unit, precision, and reference time;
- bounded retry behavior with a maximum attempt count;
- duplicate, stale, or out-of-order event behavior where relevant;
- terminal-state events and their exact no-op/rejection oracle;
- executable cases for success, timeout, retry, exhausted retries, duplicate callback, and a late callback;
- state, transition, event, retry/timeout, and selected sequence coverage arithmetic;
- explicit concurrency/event-ordering residual risks when exhaustive interleavings are not attempted.

If the requirement does not define eventual-consistency timing or callback ordering, mark it Question/TBD and do not invent a confirmed oracle.

### Example 4: State coverage versus transition and sequence coverage

Include a compact model that demonstrates the difference between coverage metrics. It must show, with checked arithmetic, that:

- a test suite can visit every state while missing one or more transitions;
- a suite can cover every valid transition while missing selected consecutive transition pairs or longer sequences;
- invalid-transition coverage is not added to valid-transition coverage;
- path/sequence coverage is reported against explicitly selected paths, not all mathematically possible paths unless exhaustive execution was intended;
- 100% of one metric does not imply 100% of the others.

Use stable IDs and a mapping from cases to states, transitions, pairs, and sequences. Explain what additional tests close each demonstrated gap.

## 13. When to use State-Transition Testing

State-Transition Testing is especially useful when:

- an object or process has a meaningful lifecycle;
- behavior depends on the current state or event history;
- user, system, timer, external, or asynchronous events cause observable state changes;
- retries, timeouts, expiration, cancellation, recovery, reopening, or terminal states matter;
- the same event has different behavior in different states or contexts;
- invalid or out-of-order events are high-risk;
- the team needs traceability from lifecycle requirements to executable sequences;
- a finite-state or partitionable state model is understandable and maintainable.

Use it after the modeled object's identity, state boundaries, events, and exact outcomes are understood. Split a dense model by lifecycle scope or use linked submodels when separate dimensions have different ownership or oracles.

Do not force this technique onto:

- a GUI navigation map where no domain object's behavior changes;
- a single unordered input with no lifecycle or event semantics;
- a purely numeric threshold problem better served by BVA;
- a large set of mostly independent configuration dimensions better served by Pairwise;
- a rule table with combinations and actions better served by Decision Tables;
- complex actor goals and end-to-end flows better represented by use-case/scenario testing;
- implementation details not observable or required by the specification.

## 14. Limitations and common mistakes

State-Transition Testing alone is insufficient for:

- values, formats, and input partitions not modeled in event guards or actions;
- numeric, length, date/time, size, count, or quota boundaries unless BVA representatives are added;
- combinations of many mostly independent parameters unless Pairwise or exhaustive testing is added;
- complex Boolean guard logic unless Decision Tables or condition/cause-effect analysis is added;
- all possible event sequences, transition pairs, paths, or concurrency interleavings;
- implementation branches not reflected in the model;
- security, performance, reliability, usability, accessibility, compatibility, and exploratory risks;
- multiple interacting objects unless their composition and consistency are modeled explicitly.

Avoid these mistakes:

1. **Modeling GUI screens instead of an object lifecycle.** A screen is not a state unless it corresponds to materially different object behavior.
2. **Mixing several objects without composition rules.** Choose one object or split the model; do not combine an order, payment, customer, and page as if they were one state machine.
3. **Creating a new state for every incidental action.** Merge states whose available behavior and invariants are equivalent.
4. **Leaving state boundaries implicit.** Define invariants, entry criteria, available actions, and restrictions.
5. **Treating an arrow as an executable test case.** Add setup, complete data, event, guard, action, oracle, destination state, and traceability.
6. **Omitting system, timer, external, or asynchronous triggers.** Model all meaningful event sources.
7. **Ignoring invalid events.** Test prohibited events, failed guards, terminal-state events, duplicates, stale callbacks, and out-of-order events when relevant.
8. **Confusing invalid with impossible.** An externally attempted forbidden event may need a negative test; an unreachable transition needs an exclusion rationale.
9. **Treating rejection, no-op, error, and recovery as the same behavior.** Define the exact response, side effects, and resulting state.
10. **Leaving guard context unspecified.** Record roles, data, time, state, environment, and reference-clock assumptions.
11. **Ignoring self-loops, retries, timeout, reset, reopen, or terminal behavior.** These often contain lifecycle defects.
12. **Assuming one path covers a state.** A state may have several entry paths and different history-dependent behavior.
13. **Counting state coverage as transition coverage.** Visiting every node does not exercise every arrow.
14. **Counting transition coverage as sequence/path coverage.** Individual transitions can be covered in separate tests while important adjacent pairs or longer sequences remain untested.
15. **Claiming all paths without defining a finite selected scope.** Loops and retries may make the path space unbounded.
16. **Using rule order as precedence without a requirement.** Resolve overlapping or nondeterministic transitions explicitly.
17. **Forcing concurrent dimensions into a false flat model.** Use composite or orthogonal modeling with legal-combination rules.
18. **Using vague oracles.** State exact response, message, persistence, notification, invocation, and destination-state outcomes.
19. **Changing multiple unrelated contexts without traceability.** Record intentional role, timing, data, or environment combinations and use complementary techniques.
20. **Trusting a visually attractive diagram without independent review.** Validate the transition table, reachability, feasibility, and coverage arithmetic separately.
21. **Removing high-risk transitions only to reduce diagram density.** Split the model or link detailed submodels instead.
22. **Leaving asynchronous timing unrepeatable.** Fix clock source, time zone, precision, event ordering, and eventual-consistency assumptions.
23. **Assuming a state model proves non-functional behavior.** Add security, performance, reliability, accessibility, usability, compatibility, and exploratory testing as needed.

## 15. Complementary techniques

Explain how State-Transition Testing works with:

- **Equivalence Partitioning (EP)** — identifies behaviorally distinct input, role, payload, and guard classes;
- **Boundary Value Analysis (BVA)** — selects values around expiration, timeout, retry-count, age, quota, and other state-changing thresholds;
- **Decision Tables** — models combinations of guards and actions when a transition depends on several conditions;
- **Pairwise testing** — covers interactions among mostly independent platforms, environments, feature flags, roles, and event parameters;
- **Condition/cause-effect coverage** — analyzes complex Boolean guard relationships;
- **Use-case/scenario testing** — covers actor goals and end-to-end flows that traverse the state model;
- **Model-based testing** — may generate sequences from a formal state model, but automation is not required and the model still needs requirement validation;
- **Error guessing** — adds duplicate, stale, out-of-order, replay, reset, crash-recovery, and historically defective event cases;
- **Risk-based testing** — prioritizes financial, security, safety, authorization, data-loss, and high-impact transitions;
- **Exploratory testing** — investigates behavior outside the declared model;
- **Security, performance, reliability, accessibility, usability, and compatibility testing** — address non-functional risks not proved by state coverage.

A practical sequence is to identify behaviorally meaningful states and guard classes with EP, target timeout and threshold edges with BVA, model explicit guard/action combinations with Decision Tables, use State-Transition Testing for lifecycle and event sequences, use Pairwise for independent context combinations, and add risk-based, error-guessing, model-based, and exploratory tests for residual risks.

## 16. Verification checklist

Before approving a State-Transition design, confirm:

- [ ] Scope, modeled object/lifecycle, requirement basis, preconditions, and exact oracle are documented.
- [ ] The guide distinguishes State-Transition Testing from a State & Transition Diagram as an artifact.
- [ ] One object or one explicitly bounded process is modeled; multiple objects have explicit composition or separate models.
- [ ] States have stable IDs, behaviorally distinct meanings, entry criteria, invariants, and available/forbidden actions.
- [ ] States are mutually exclusive and collectively exhaustive for the declared scope, or gaps are visible.
- [ ] Initial, terminal, error, recovery, and dead-end states are identified where relevant.
- [ ] GUI screens and incidental actions are not incorrectly presented as domain states.
- [ ] Behaviorally equivalent states are merged or their distinction is justified.
- [ ] Events/triggers have stable IDs, sources, applicable states, payload/context assumptions, and ordering/retry semantics.
- [ ] Guards have stable IDs, formal predicates, true/false behavior, context, and exact failed-guard oracle.
- [ ] Transitions have stable IDs, source/destination states, triggers, guards, actions, validity, and exact outcomes.
- [ ] User, system, timer, external, callback, scheduled, and data-driven events are included when relevant.
- [ ] Self-transitions, loops, retries, timeouts, reset/reopen, recovery, duplicate, stale, and terminal-state events are considered.
- [ ] Invalid, forbidden, impossible, unreachable, and unknown behavior are distinguished.
- [ ] Invalid-transition tests identify the violated rule and exact rejection/no-op/error/side-effect oracle.
- [ ] Impossible or unreachable elements have exclusion rationales and are not counted as uncovered valid behavior.
- [ ] Timing precision, clock source, time zone, reference time, event ordering, and eventual-consistency assumptions are explicit where relevant.
- [ ] Concurrent or orthogonal state dimensions are modeled explicitly rather than flattened without legal-combination rules.
- [ ] The diagram notation, initial/final markers, transition labels, and table authority are clear.
- [ ] Dense models are split into linked submodels without deleting important risk transitions.
- [ ] Every selected transition maps to an executable case with setup, complete context/data, event steps, guard evidence, exact oracle, destination state, priority, and traceability.
- [ ] Sequence cases record every intermediate state and transition ID.
- [ ] State, valid-transition, event, guard, selected-invalid, terminal, transition-pair, sequence/path, invariant, and timing/retry metrics are reported separately where selected.
- [ ] Coverage arithmetic uses deduplicated stable IDs and explicit denominators.
- [ ] State coverage is not presented as transition or path coverage.
- [ ] Transition coverage is not presented as all-sequence, all-guard, all-data, concurrency, branch, requirement, or non-functional coverage.
- [ ] Uncovered states/transitions/events/guards/pairs/sequences/paths, excluded elements, assumptions, Question/TBD items, and residual risks are visible.
- [ ] EP, BVA, Decision Tables, Pairwise, use-case/scenario, condition/cause-effect, model-based, error-guessing, risk-based, and exploratory follow-ups are identified where appropriate.
- [ ] No vague oracle such as “the system works correctly” remains.
- [ ] Markdown diagrams and tables render correctly, with no blank image scaffolds, duplicate headings, placeholders, or non-English text.

---

# Project-local State-Transition skill requirements

Create `.claude/skills/state-transition-testing/SKILL.md` following the existing project skill convention.

## 1. Frontmatter and guide reference

Use YAML frontmatter exactly in this form:

```yaml
---
name: state-transition-testing
description: Apply ISTQB-aligned State-Transition Testing to a supplied requirement, acceptance criterion, lifecycle, workflow, existing state diagram, transition table, event-driven rule, retry/timeout rule, or state-transition test case. Trigger when the user asks to model states and events, derive valid or invalid transitions, review lifecycle behavior, prepare sequence/path tests, or calculate state-transition coverage.
version: 0.1.0
---
```

Reference the detailed project guide exactly as:

`docs/TestDesignAndSoftwareTestingTechniques/BlackBox/stateTransition.md`

The skill must state that the guide is the primary terminology, workflow, examples, templates, and coverage reference. If the guide is unavailable, provide a usable fallback based on the rules in the skill and ISTQB-consistent black-box specification-based terminology. Do not invent product behavior because a diagram or requirement is incomplete.

## 2. Skill input and behavior

The skill must:

- accept requirements, acceptance criteria, lifecycle/workflow descriptions, existing state diagrams, transition tables, event contracts, and existing state-transition test cases;
- identify one modeled object or explicitly bounded lifecycle and flag GUI-only or multi-object scope problems;
- model states, invariants, events/triggers, guards, actions/effects, initial/final/error/recovery states, and stable IDs independently;
- distinguish state behavior from GUI screens and incidental user-interface actions;
- formalize valid, invalid, forbidden, impossible, unreachable, and unknown behavior;
- model user, system, timer, external, callback, scheduled, and data-driven triggers where relevant;
- handle self-loops, retries, timeouts, expiration, reset/reopen/recovery, duplicate/stale/out-of-order events, terminal-state events, and concurrency assumptions where relevant;
- never invent states, events, guards, actions, timing, precedence, or expected behavior;
- ask only questions that block safe modeling or make the oracle unknowable;
- label information as Confirmed, Assumption, Question/TBD, or Residual risk;
- require exact source/destination state, response, side-effect, persistence, notification, and error oracles;
- preserve a user-requested output format such as Gherkin, JSON, CSV, or a test-management schema;
- independently verify reachability, feasibility, transition consistency, coverage arithmetic, and excluded elements;
- derive executable cases with complete setup, input/context, events, guards, actions, expected outcomes, destination states, priorities, and traceability.

## 3. Default skill output

When no format is specified, the skill must produce Markdown with these sections:

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

The skill must include reusable templates for state modeling, event/guard/action modeling, transition inventory, transition tables, executable cases, coverage/gaps, and residual risks. The executable-case template must include transition or sequence ID, requirement reference, priority, preconditions/setup, initial/source state, complete input/context, event/steps, guard evidence, exact oracle, expected destination state, side effects/persistence/notifications, covered model IDs, technique tags, and assumptions/notes.

## 4. Skill coverage and review rules

The skill must require separate reporting for:

- reachable-state coverage;
- valid-transition coverage;
- event/trigger coverage;
- guard/condition outcome coverage;
- selected invalid/forbidden-transition coverage;
- terminal-state and state-invariant coverage where applicable;
- transition-pair/switch coverage when explicitly selected;
- selected sequence/path coverage;
- timing, retry, duplicate, ordering, and concurrency coverage when in scope;
- row/case counts, model version, environment, clock/event-order settings, and generation metadata when applicable.

It must require explicit lists of:

- uncovered reachable states and valid transitions;
- untested events, guard outcomes, terminal states, transition pairs, sequences, and paths;
- impossible/unreachable elements and exclusion rationales;
- invalid events not selected and their rationale;
- dead ends, ambiguous transitions, overlap/nondeterminism, missing guards, and contradictory outcomes;
- untested timing, retry, duplicate, stale, ordering, persistence, and concurrency behavior;
- assumptions, Question/TBD items, and residual risks.

It must warn that 100% State-Transition coverage is meaningful only relative to the declared model and metric. It does not prove every value, event sequence, guard/data combination, path, branch, concurrent interleaving, requirement, or non-functional property.

## 5. Skill manual checklist

Before presenting a result, the skill must verify:

- scope, modeled object, requirement, setup, and exact oracle;
- one-object/lifecycle scope and explicit composition of any concurrent dimensions;
- stable state IDs, invariants, initial/terminal/error/recovery states, and behaviorally distinct state definitions;
- stable event, guard, action, and transition IDs with complete meanings and contexts;
- explicit diagram notation and transition-table semantics;
- user/system/timer/external/asynchronous trigger coverage where relevant;
- valid, invalid, forbidden, impossible, unreachable, and unknown classification;
- exact invalid-event behavior including resulting state and side effects;
- explicit timing, clock, timezone, precision, retry, ordering, duplicate, and concurrency assumptions;
- complete valid cases with source state, event, guard, action, destination state, exact oracle, and traceability;
- separate state, transition, event, guard, invalid, terminal, pair, sequence/path, invariant, and timing metrics;
- correct deduplicated coverage arithmetic and excluded-element rationale;
- detection of duplicate/equivalent states, missing transitions, dead ends, overlap, nondeterminism, and contradictory outcomes;
- uncovered items, assumptions, Question/TBD items, residual risks, and complementary techniques;
- appropriate EP, BVA, Decision Table, Pairwise, use-case/scenario, condition/cause-effect, model-based, error-guessing, risk-based, and exploratory follow-ups;
- valid Markdown diagrams/tables, no placeholders, no duplicate headings, no vague oracles, and English-only output.

---

# Quality and acceptance criteria

The future implementation is complete only when all of the following are true:

1. `docs/TestDesignAndSoftwareTestingTechniques/BlackBox/stateTransition.md` is a standalone English State-Transition Testing guide rather than a Russian article, article metadata, promotional prose, or blank diagram scaffold.
2. `.claude/skills/state-transition-testing/SKILL.md` exists, uses the required frontmatter, references the exact project-relative guide path, and provides reusable State-Transition instructions.
3. Both deliverables are junior-readable while preserving precise black-box, specification-based, lifecycle, event, and coverage terminology.
4. The guide and skill define modeled object, state, invariant, initial/final/terminal/error/recovery state, event/trigger, guard, action/effect, transition, self-loop, valid, invalid, forbidden, impossible/unreachable, sequence, pair, path, oracle, state coverage, transition coverage, and related metrics.
5. The source concepts are formalized: one-object scope, mutually exclusive behaviorally distinct states, state as lifecycle behavior rather than GUI, event-labeled transitions, meaningful system/timer/external triggers, merged equivalent states, and decomposition of dense diagrams.
6. The guide explicitly corrects the claim that every arrow is automatically a test case and requires executable setup, events, guards, actions, oracles, destination states, and traceability.
7. The workflow covers scope, state/event/guard/action extraction, model construction, reachability/feasibility, completeness, consistency, invalid behavior, timing/retry/concurrency, executable cases, independent coverage verification, and risk reporting.
8. The guide includes diagram and transition-table representations with stable IDs and a plain-text/tabular fallback.
9. The guide contains at least four self-contained worked examples: an object lifecycle; a guarded role/context workflow; asynchronous timeout/retry behavior; and a metric comparison demonstrating state versus transition versus sequence/path coverage.
10. Every worked example states assumptions, preconditions, complete inputs/context, event actions, exact oracles, source/destination states, stable IDs, traceability, and independently checked coverage arithmetic.
11. Coverage separates reachable states, valid transitions, events, guards, selected invalid transitions, terminal states, invariants, transition pairs, sequences/paths, and timing/retry/concurrency objectives where applicable.
12. Impossible or unreachable elements are excluded from valid coverage denominators with explicit rationales; intentional invalid cases have violated rule/guard IDs and exact negative oracles.
13. The reusable templates contain the required state, event, guard, action, transition, test-case, coverage/gap, and residual-risk fields.
14. Limitations, common mistakes, applicability guidance, complementary techniques, assumptions, questions/TBDs, and residual risks are visible.
15. The guide and skill clearly distinguish State-Transition Testing from EP, BVA, Decision Tables, Pairwise, use-case/scenario testing, condition/cause-effect analysis, model-based testing, error guessing, risk-based testing, and non-functional testing.
16. No unsupported claim says that 100% state, transition, event, pair, sequence, or path coverage proves all values, all sequences, all guards, all branches, all requirements, concurrency, or non-functional behavior.
17. No vague oracle such as “the system works correctly,” no unmarked invented behavior, no duplicate headings, no arbitrary placeholders, no malformed tables, and no non-English text remain.
18. Only the two requested future deliverables are modified during their implementation; unrelated files remain unchanged.

## Required final verification for the future implementation

Before considering the future implementation complete:

- read both deliverables end-to-end;
- verify English-only content and consistent heading hierarchy;
- parse every Markdown table and confirm matching header, separator, and body column counts;
- check the skill frontmatter fields and exact guide reference;
- verify diagram notation, transition-table authority, stable IDs, and source/destination consistency;
- independently recalculate all worked-example state, transition, event, guard, invalid, terminal, pair, sequence/path, and timing coverage arithmetic;
- verify reachability and exclusion rationales for impossible/unreachable elements;
- verify each valid and invalid case's source state, event, guard, action, exact oracle, destination state, side effects, and traceability;
- inspect timing, retry, duplicate, stale, out-of-order, terminal, reset, recovery, and concurrency claims against their assumptions;
- check that GUI pages, multiple objects, duplicate states, and incidental events are handled according to the modeling rules;
- check that no claim conflates state coverage, transition coverage, sequence/path coverage, or non-functional coverage;
- run `git diff --check`;
- inspect `git diff --name-only` and confirm only the two intended deliverables changed during their implementation;
- report any pre-existing unrelated working-tree changes separately;
- do not commit or push unless explicitly requested.
