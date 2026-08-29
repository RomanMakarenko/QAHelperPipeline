# State-Transition Testing

## Scope, modeled object, requirement basis, and exact oracle

State-Transition Testing is a black-box, specification-based technique for checking how one modeled object or bounded process changes in response to events. It is useful when behavior depends on the current state, event history, role, time, retry count, or event ordering.

Start every design by recording:

| Field | What to record |
| --- | --- |
| Modeled object or lifecycle | One order, ticket, payment, document, session, job, or explicitly bounded process |
| Scope boundary | Included states, events, actors, data, time range, and excluded behavior |
| Requirement basis | Requirement, acceptance criterion, contract, approved diagram, or `Question/TBD` |
| Initial condition | How the object is created and the exact initial state |
| Exact oracle | Response code/message, accepted or rejected event, persisted state, emitted event, notification, calculation, audit record, invocation, and side effects |
| Priority | Risk or business priority for each transition or scenario |
| Model status | Confirmed, Assumption, Question/TBD, or Residual risk |

A diagram node or arrow is a model element, not automatically an executable test case. An executable case also needs setup, complete input and context, event steps, guard evidence, expected action/effect, exact oracle, expected destination state, side-effect expectations, priority, and traceability.

## Clarifications, assumptions, questions, and residual risks

Use these labels consistently:

- **Confirmed** — stated directly in a requirement, contract, approved model, or established domain rule.
- **Assumption** — introduced to make a teaching example or provisional model executable; it must be confirmed before production use.
- **Question/TBD** — behavior that is not specified and may change the model or oracle. Ask about it when it blocks safe modeling.
- **Residual risk** — meaningful behavior outside the selected scope or coverage, even when the current model is internally consistent.

Ask only questions that block safe modeling or make the expected oracle unknowable. Typical blocking questions concern the modeled object, state boundary, initial state, terminal behavior, event source, guard, destination state, timing, retry limit, ordering, persistence, or exact rejection behavior.

Never silently classify missing behavior as invalid or impossible. For example, if a callback after expiration is not specified, record `Question/TBD`; do not assume that it is rejected. An example may use a provisional rule, but it must label the rule as an Assumption.

## What State-Transition Testing is and ISTQB alignment

State-Transition Testing represents the behavior of an object or system as states and transitions caused by events or triggers. Tests exercise state entry and invariants, valid and invalid transitions, guards, actions, event sequences, and observable outcomes. This is consistent with the ISTQB use of state-transition models as specification-based test-design models; this guide is a practical project supplement, not a replacement for the current official syllabus, product requirements, or domain constraints.

The names describe different artifacts:

- **State & Transition Diagram** — a visual modeling artifact with state nodes and labeled directed transitions.
- **State-Transition Model** — the complete set of states, events, guards, actions, constraints, and transition rules. It may be represented by a diagram, table, or plain text.
- **State-Transition Testing** — the test-design technique that derives and executes tests from that model.

The model helps reveal missing paths, forbidden events, terminal-state mistakes, retry defects, and ordering problems. It does not prove every data value, implementation branch, event sequence, concurrency interleaving, requirement, or non-functional property.

## Terminology and notation

| Term | Practical meaning |
| --- | --- |
| Modeled object/system | The single domain entity or bounded lifecycle being modeled, such as an order, ticket, payment, session, job, document, or account process |
| State | A condition in which a defined set of events/actions is available or unavailable and specified behavior remains stable |
| State invariant | A property that must hold while the object is in a state |
| Initial state | The starting state for the declared test scope |
| Final/terminal state | A lifecycle-complete state from which no further in-scope transition is expected |
| Error/recovery state | A defined failed, rejected, suspended, or recoverable condition |
| Event/trigger | A user action, system action, timer, scheduled job, callback, external message, data change, or other occurrence that may cause a transition |
| Guard/condition | A predicate that must be true for a transition to be enabled |
| Transition | A directed relation from a source state to a destination state caused by an event and enabled, when applicable, by a guard |
| Action/effect | An observable operation during a transition, such as persistence, calculation, notification, external invocation, or audit logging |
| Self-transition/loop | A transition whose source and destination are the same state |
| Valid transition | A transition permitted under its preconditions and satisfied guards |
| Invalid transition | An attempted event or transition that is not allowed or fails its guard and has specified rejection, no-op, error, or recovery behavior |
| Forbidden event | An externally possible event that is prohibited in the current state or context |
| Impossible/unreachable element | A state or transition that cannot be reached from the initial state under stated constraints; it requires an exclusion rationale |
| Unknown behavior | Behavior that is not safe to classify as valid, invalid, forbidden, or impossible |
| State-transition table | A tabular source of truth for source state, event, guard, action, destination, status, and oracle |
| State diagram | A graphical representation of state nodes and directed, labeled transitions |
| Transition sequence | An ordered list of events and resulting transitions |
| Transition pair/switch | Two transitions executed consecutively where the first destination is the second source |
| Path | A selected route through states and transitions from an initial or defined starting state |
| Dead end | A non-terminal state with no permitted outgoing transition when the model expects one |
| Test oracle | The exact observable result used to determine pass or fail |
| Constraint/dependency | A rule that limits legal states, events, guards, combinations, ordering, timing, retries, or composition; assign a stable Constraint ID when it affects feasibility or reachability |
| State coverage | Coverage of required reachable State IDs |
| Valid-transition coverage | Coverage of required valid Transition IDs |
| Event/trigger coverage | Coverage of modeled required Event IDs or trigger sources |
| Guard/condition coverage | Coverage of selected true/false guard outcomes or branches |
| Invalid-transition coverage | Coverage of selected prohibited events or failed guards, reported separately from valid transitions |
| Transition-pair/sequence coverage | Coverage of selected consecutive transition pairs or sequences |
| Path coverage | Coverage of explicitly selected paths, not an implicit claim to cover every possible path |

Recommended label notation is:

```text
T-001: submit | [valid data] | persist order | Draft -> Submitted
```

For a self-loop, show the same source and destination. For an invalid event, show the expected state after the attempt, not only “not allowed.”

## Core modeling principles

1. **Select one object or bounded lifecycle.** Model an order, not simultaneously the order, payment, customer, cart, and web page. If several objects interact, model them separately and document the interaction or use explicit composition.
2. **Define states by behavior and invariants.** A state changes the available operations, restrictions, processing behavior, data guarantees, or observable outcomes.
3. **Make ordinary states mutually exclusive.** An object has one ordinary state at a time. Concurrent dimensions require explicit orthogonal or composite modeling.
4. **Cover the relevant domain.** States should be collectively exhaustive for the declared boundary, or the unmodeled region must be visible as a gap, Question/TBD, or Residual risk.
5. **Do not model every screen or incidental action.** A page, URL, button, or unchanged form value is not a state unless it represents different domain behavior. A GUI can help reach or observe a state.
6. **Merge equivalent states.** Merge states with identical relevant actions, restrictions, invariants, transitions, and oracles unless a requirement distinguishes them.
7. **Label every transition.** Include an event, optional guard and action, source, destination, actor or trigger source, and stable ID.
8. **Include meaningful trigger sources.** Consider user, system, timer, scheduled job, external service, callback, and data-driven triggers.
9. **Separate no-op, rejection, error, and recovery.** They may have different response, persistence, notification, audit, and destination-state oracles.
10. **Model state-dependent events.** The same event may be valid in one state, invalid in another, or guarded by role, data, time, or account context.
11. **Include lifecycle edges.** Model initial, terminal, error, retry, timeout, reset, reopen, cancel, resume, and recovery behavior when in scope.
12. **Keep diagrams reviewable.** Split dense diagrams into linked submodels without removing important high-risk transitions.
13. **Do not invent implementation detail.** Browser closure, a server crash, or a database column is not a modeled event unless the requirement makes it observable and relevant.

## State, event, guard, action, and dependency model

### State model

For every state record its ID, meaning, entry criteria, invariant, allowed and forbidden actions, persistence expectations, outgoing transitions, terminal/dead-end status, relevant context, requirement reference, and status label. Useful state questions are:

- What must be true on entry and while the object remains here?
- Which events are accepted, rejected, ignored, or routed to recovery?
- What response, persisted data, notification, audit record, or external call proves the behavior?
- Can the object leave, return, expire, or remain indefinitely?

A state is not merely “the form is open.” It may be “Draft” if the object can be edited and has not been submitted; opening several different screens does not create separate states when the object behavior is unchanged.

### Event and trigger model

For every event record an ID, source, payload or representation, applicable states, forbidden states, duplicate/idempotency behavior, ordering assumptions, retry semantics, and status. Source values include `user`, `system`, `timer`, `scheduled`, `external`, `callback`, and `data-driven`.

### Guard model

For every guard record an ID, formal predicate, dependencies, true and false behavior, role/data/time/context boundaries, and whether a failed guard rejects, no-ops, retries, or enters recovery. Use a separate guard ID when outcomes are materially different. A failed guard needs an exact oracle such as HTTP 403 with a stable error code and no state or audit mutation.

### Action/effect model

For every action record an ID, operation or calculation, exact output and precision, persistence, notification, external invocation, audit record, state change, dependencies, mutual exclusions, and status. Empty or absent effects must have a defined meaning such as “no persistence and no notification.”

### Transition model

A transition record must include:

- stable Transition ID;
- source and destination State IDs;
- Event/Trigger ID;
- Guard ID or explicit unguarded status;
- actor/source and preconditions;
- Action/Effect ID and exact oracle;
- valid, invalid, forbidden, impossible, or Question/TBD classification;
- loop, retry, timeout, terminal, or recovery attributes;
- requirement reference and status.

Use EP for behaviorally distinct state, role, payload, and context classes; BVA for timeout, expiration, retry-count, and other thresholds; Decision Tables for combinations of guards; and Pairwise for mostly independent environment or parameter dimensions.

## Valid, invalid, forbidden, impossible, unreachable, and unknown behavior

- **Valid/legal:** executable from a reachable state with satisfied preconditions and guards.
- **Invalid:** can be attempted from a reachable state but is not permitted or fails a guard; it needs a defined response, state, and side-effect oracle.
- **Forbidden:** an externally possible event or payload prohibited by a state or contract. It may be a selected negative test, separate from positive coverage.
- **Impossible/unreachable:** cannot occur under the declared initial state and constraints. Document why and exclude it from valid executable denominators.
- **Unknown:** not specified well enough to classify. Mark Question/TBD and ask for clarification when it blocks a safe test.

An invalid case must identify the reachable source state, complete event/context, violated state rule or guard ID, exact response/status/message or no-op, expected state after the attempt, and persistence/notification/audit expectations. Report invalid-transition coverage separately from valid-transition coverage.

Explicitly consider duplicate submissions, repeated events, stale callbacks, out-of-order messages, retry after success, retry after failure, and events received in terminal states whenever those risks are relevant. Do not assume that all such events are harmless or idempotent.

## Timing, asynchronous behavior, retries, ordering, concurrency, and composition

Record the clock source, time zone, reference time, unit, precision, boundary rule, scheduler behavior, and eventual-consistency expectation for timing rules. For example, “expires at elapsed >= 30 seconds using the UTC service clock at second precision” is repeatable; “expires after a while” is not.

For asynchronous behavior, model callbacks, queues, polling, delayed jobs, and timer events as trigger sources. Define whether an event is accepted once, idempotent, ignored, rejected, or sent to recovery. Define which persisted state and response are observable before and after eventual completion.

For retries, record the attempt number at each state, maximum attempts, backoff, retry trigger, increment action, and terminal behavior. Use BVA around attempt limits and timeout boundaries. Test success on the first attempt, success after retry, exhausted retries, and duplicate or late provider events.

For ordering and concurrency, record whether events can arrive concurrently, whether ordering is guaranteed, and how locks, optimistic versions, or conflicts are observed. If this is unspecified, use Question/TBD or a clearly labeled Assumption. Do not flatten independent dimensions into a false mutually exclusive state. Use separate linked state machines or explicit orthogonal/composite regions and declare which combinations are legal.

## Diagram and transition-table representations

A diagram should contain stable state IDs, a marked initial state, terminal markers, labeled directed transitions, and a legend. A table must remain authoritative if rendering fails or the diagram is dense. State whether the table is:

- **Exhaustive** for the declared state/event domain;
- **Reduced** with a documented equivalence or expansion rationale;
- **Positive-only** with invalid behavior recorded elsewhere; or
- **Augmented-invalid** with selected invalid or forbidden attempts.

Example plain-text diagram:

```text
[*] -> S1 Draft
S1 -- T1 submit / A1 persist --> S2 Submitted
S2 -- T2 start / A2 enqueue --> S3 InProgress
S3 -- T3 complete / A3 finalize --> S4 Completed
S3 -- T6 failure / A5 record error --> S6 Failed
S6 -- T7 retry / A6 increment attempt --> S3
S4 --> [*]
```

The table must preserve semantics that a diagram may hide: guards, payloads, timing, exact responses, side effects, exclusions, and case IDs. Every transition source and destination must exist in the state model.

## Repeatable State-Transition workflow

1. **Define scope and oracle.** Identify operation, modeled object, lifecycle boundary, requirements, preconditions, and exact observable outcomes.
2. **Choose one object or bounded process.** Reject GUI-only or mixed-object scope unless composition is explicit.
3. **Identify states.** Extract initial, active, intermediate, terminal, error, recovery, and dead-end states from behavior and restrictions.
4. **Check state semantics.** Make states non-overlapping and behaviorally distinct; merge equivalent states and record gaps.
5. **Identify events and sources.** List user, system, timer, external, callback, scheduled, and data-driven triggers.
6. **Identify guards and context.** Formalize role, account, data, time, state, environment, dependency, and boundary conditions.
7. **Identify actions and oracles.** Record response, persistence, calculation, notification, invocation, audit, processing path, and state change.
8. **Build the diagram and inventory.** Assign stable State, Event, Guard, Action, Constraint, and Transition IDs.
9. **Classify feasibility.** Mark valid, invalid, forbidden, impossible/unreachable, or Question/TBD and record rationales.
10. **Check completeness.** Review every reachable state for relevant valid and invalid events, loops, terminal behavior, timeouts, retries, and recovery.
11. **Check consistency.** Find duplicate states, missing IDs, ambiguous destinations, overlapping transitions, nondeterminism, dead ends, missing guards, and mismatched oracles.
12. **Select coverage objectives.** Choose state, transition, event, guard, invalid, terminal, invariant, pair, sequence, path, timing, retry, or concurrency metrics and define denominators.
13. **Derive executable cases.** Add setup, complete context, source state, event, guard evidence, action, exact oracle, destination, priority, and traceability.
14. **Test exceptional behavior.** Exercise selected failed guards, forbidden events, duplicate/stale/out-of-order events, retries, timeouts, terminal events, and recovery.
15. **Execute against the oracle.** Verify source, accepted/rejected event, action, state, response, persistence, notification, audit, and side effects.
16. **Calculate coverage independently.** Recalculate deduplicated IDs rather than trusting a generator or visual report.
17. **Report gaps and risks.** List uncovered items, exclusions, assumptions, Questions/TBD, and residual timing/concurrency/non-functional risk.
18. **Revise from evidence.** Split supposedly equivalent states or transitions when actual behavior differs; document the defect and update the model.

## Coverage definitions and reporting

Define a finite, stable denominator before execution. Use deduplicated IDs and report excluded impossible/unreachable elements separately.

| Metric | Formula and interpretation |
| --- | --- |
| Reachable-state coverage | visited required reachable State IDs / total required reachable State IDs × 100% |
| Valid-transition coverage | exercised required valid Transition IDs / total required valid Transition IDs × 100% |
| Event/trigger coverage | exercised required Event IDs / total modeled required Event IDs × 100%; separate trigger sources when useful |
| Guard outcome coverage | exercised required true/false outcomes or branches / total required guard outcomes or branches × 100%, only when selected |
| Selected invalid coverage | exercised selected invalid/forbidden attempts / selected invalid/forbidden attempts × 100%, separate from valid coverage |
| Terminal-state coverage | visited required terminal State IDs / total required terminal State IDs × 100% |
| State-invariant coverage | exercised declared state invariants / total selected state invariants × 100% |
| Transition-pair coverage | exercised selected consecutive Transition-ID pairs / total selected pairs × 100% |
| Sequence/path coverage | executed selected sequence/path IDs / total selected sequence/path IDs × 100%; not all mathematical paths by implication |
| Timing/retry/concurrency coverage | exercised selected timeout, boundary, retry, duplicate, ordering, or interleaving scenarios / selected scenarios × 100% |

Distinguish visiting a state from verifying all behavior in it; exercising a transition from covering every guard/data/role path that enables it; one event from that event in every relevant state; a pair from a longer sequence; representing a path from executing it; and valid coverage from invalid coverage.

Do not count impossible or unreachable elements as uncovered valid behavior. List each exclusion and its rationale. If one transition has materially different guards or outcomes, split it into separate Transition or Guard IDs.

Record model version, requirement version, manual or generated method, tool/version, environment, clock configuration, event-order setting, selected scope, excluded elements, case count, and execution result. A passing suite with 100% declared state or transition coverage does not prove all values, all event sequences, all paths, all guards, all implementation branches, all concurrent interleavings, all requirements, or any non-functional property.

## Required worked examples

### Example 1: Order lifecycle

**Requirement basis — Assumption.** An order can be submitted, started by the processing service, completed, cancelled before completion, or recoverably failed and retried. The exact API contracts below are teaching assumptions, not product facts.

**Modeled object:** one `Order` object. A web page, customer, inventory item, and payment are outside this model.

**States and invariants**

| State ID | Meaning and invariant | Entry/exit notes | Status |
| --- | --- | --- | --- |
| `S1` | `Draft` — editable order data is valid enough to submit; not yet submitted | Initial; `T1` exits | Assumption |
| `S2` | `Submitted` — order is immutable to the customer and awaits processing | `T1` enters; `T2` or `T4` exits | Assumption |
| `S3` | `InProgress` — processing has started and a worker owns the attempt | `T2` or `T7` enters; `T3`, `T5`, or `T6` exits | Assumption |
| `S4` | `Completed` — fulfillment result is finalized and immutable | Terminal; `T3` enters | Assumption |
| `S5` | `Cancelled` — cancellation is recorded and no processing may start | Terminal; `T4` or `T5` enters | Assumption |
| `S6` | `Failed` — processing failed with a recorded reason and retry is available | Recovery; `T6` enters and `T7` exits | Assumption |

**Events and actions**

| ID | Type/source | Meaning | Action/oracle | Status |
| --- | --- | --- | --- | --- |
| `E1` | User | Submit complete valid order | `A1` persists `status=Submitted` and returns HTTP 202 | Assumption |
| `E2` | System | Start processing | `A2` records a processing attempt and destination `InProgress` | Assumption |
| `E3` | System | Processing completes | `A3` persists final result and returns internal success | Assumption |
| `E4` | User | Cancel | `A4` persists cancellation and returns HTTP 200 | Assumption |
| `E5` | System | Processing failure | `A5` records error reason and makes retry available | Assumption |
| `E6` | User or system | Retry failed order | `A6` increments retry metadata and starts processing | Assumption |

**Guards**

| Guard ID | Predicate | True/false behavior | Status |
| --- | --- | --- | --- |
| `G1` | Order data is valid enough to submit | True enables `T1`; false is outside this positive-only example | Assumption |
| `G2` | Worker accepts the submitted order | True enables `T2`; false behavior is Question/TBD | Assumption |
| `G3` | Processing succeeds | True enables `T3`; false enables `T6` | Assumption |
| `G4` | Cancellation is allowed before completion | True enables `T4` or `T5`; false behavior is Question/TBD | Assumption |
| `G5` | Processing failure is detected | True enables `T6`; false means no failure transition | Assumption |
| `G6` | Retry is available | True enables `T7`; false behavior is Question/TBD | Assumption |

**Transition inventory and authoritative table** — positive-only table; selected invalid attempts follow it.

| Transition ID | Source | Event | Guard/precondition | Action | Destination | Status | Exact oracle |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `T1` | `S1` | `E1 submit` | `G1` valid order data | `A1` persist submission | `S2` | Valid | HTTP 202; persisted state is `Submitted`; one submission audit record |
| `T2` | `S2` | `E2 start` | `G2` worker accepts order | `A2` record attempt | `S3` | Valid | Worker acknowledgement; persisted state is `InProgress`; processing event emitted once |
| `T3` | `S3` | `E3 complete` | `G3` processing succeeds | `A3` finalize result | `S4` | Valid | Final result persisted; state is `Completed`; completion notification emitted once |
| `T4` | `S2` | `E4 cancel` | `G4` cancellation allowed before completion | `A4` record cancellation | `S5` | Valid | HTTP 200; state is `Cancelled`; no processing job emitted |
| `T5` | `S3` | `E4 cancel` | `G4` cancellation allowed before completion | `A4` record cancellation | `S5` | Valid | HTTP 200; state is `Cancelled`; no completion action afterward |
| `T6` | `S3` | `E5 failure` | `G5` processing failure is detected | `A5` record reason | `S6` | Valid | Error reason persisted; state is `Failed`; retry event available |
| `T7` | `S6` | `E6 retry` | `G6` retry is available | `A6` increment and restart | `S3` | Valid | Retry count increments exactly once; state is `InProgress`; one processing attempt emitted |

Model exclusions and negative behavior:

- `T-IMP-1`: direct `S1 Draft -> S4 Completed` is impossible under this requirement because completion requires `S2 -> S3`; it is excluded from the valid denominator, not reported as an uncovered valid transition.
- `T-TERM-1A`: cancellation received in `S4 Completed` is a terminal-state event. **Assumption:** HTTP 409 with code `ORDER_ALREADY_COMPLETED`, unchanged state, no result overwrite, no duplicate notification.
- `T-TERM-1B`: completion received in `S4 Completed` is a terminal-state event. **Assumption:** HTTP 409 with code `ORDER_ALREADY_COMPLETED`, unchanged state, no result overwrite, no duplicate notification.
- `T-INV-1`: completion from `S1 Draft` is an invalid attempt. **Assumption:** HTTP 409 with code `ORDER_NOT_IN_PROGRESS`, unchanged state, no completion audit or persistence.

**Constraint model**

| Constraint ID | Rule | Affected elements | Consequence | Status |
| --- | --- | --- | --- | --- |
| `C1` | Completion requires the order to be in `S3 InProgress` | `T3`, `T-INV-1`, `S1`, `S3` | Direct `S1 -> S4` completion is impossible; completion from `S1` is invalid | Assumption |
| `C2` | Cancellation is allowed in `S2` and `S3` but not after completion | `T4`, `T5`, `T-TERM-1A` | Cancellation from `S4` is a selected terminal-state negative case | Assumption |

**Diagram**

```text
[*] -> S1 Draft
S1 -- T1/E1 submit --> S2 Submitted
S2 -- T2/E2 start --> S3 InProgress
S2 -- T4/E4 cancel --> S5 Cancelled
S3 -- T3/E3 complete --> S4 Completed
S3 -- T5/E4 cancel --> S5 Cancelled
S3 -- T6/E5 failure --> S6 Failed
S6 -- T7/E6 retry --> S3 InProgress
S4, S5 are terminal
```

**Executable cases**

| Test ID | Transition/sequence | Setup and complete data | Steps | Exact oracle and destination | Traceability |
| --- | --- | --- | --- | --- | --- |
| `ST1-01` | `Q1=(T1,T2,T3)` | Create order `O-101` with valid SKU, quantity, address, and payment reference; state `S1` | Submit; dispatch start; return successful processing | HTTP 202 on submit; states `S1 -> S2 -> S3 -> S4`; result and one completion notification persisted | `REQ-ORDER-LIFECYCLE`, `S1-S4`, `E1-E3`, `A1-A3` |
| `ST1-02` | `T4` | Create valid `O-102` in `S2`; no worker start | Cancel order | HTTP 200; state `S5`; no processing event | `REQ-ORDER-LIFECYCLE`, `S2,S5`, `E4`, `A4` |
| `ST1-02B` | `T5` | Create valid `O-102B` in `S3`; worker cancellation is enabled | Cancel order | HTTP 200; state `S5`; cancellation is persisted; no completion action afterward | `REQ-ORDER-LIFECYCLE`, `S3,S5`, `T5`, `E4`, `A4` |
| `ST1-03` | `Q2=(T2,T6,T7,T3)` | Create submitted `O-103`; inject one processing failure | Start; inject failure; retry; complete | States `S2 -> S3 -> S6 -> S3 -> S4`; failure reason, retry count, result, and one completion notification are exact | `REQ-ORDER-LIFECYCLE`, `T2,T6,T7,T3` |
| `ST1-04A` | `T-TERM-1A` | Create completed `O-104A` with result and notification already recorded | Send cancel request | HTTP 409 `ORDER_ALREADY_COMPLETED`; state, result, audit, and notification count unchanged | `T-TERM-1A`, `S4`, `E4` |
| `ST1-04B` | `T-TERM-1B` | Create completed `O-104B` with result and notification already recorded | Send completion event | HTTP 409 `ORDER_ALREADY_COMPLETED`; state, result, audit, and notification count unchanged | `T-TERM-1B`, `S4`, `E3` |
| `ST1-05` | `T-INV-1` | Create draft `O-105` with valid draft data | Send completion event | HTTP 409 `ORDER_NOT_IN_PROGRESS`; state remains `S1`; no completion side effect | `T-INV-1`, `S1`, `E3` |

**Coverage check**

- Reachable states: `S1-S6`, `6/6 = 100%`.
- Valid transitions: `T1-T7`, `7/7 = 100%`; `T5` is exercised by `ST1-02B`.
- Selected invalid/terminal attempts: `T-TERM-1A`, `T-TERM-1B`, and `T-INV-1`, `3/3 = 100%`, reported separately.
- Selected guard outcomes: `G1=true`, `G2=true`, `G3=true`, `G4=true`, `G5=true`, and `G6=true`, `6/6 = 100%`.
- `T-IMP-1` is excluded with a rationale and is not an uncovered valid transition.

Outside scope: all SKU and quantity partitions, payment behavior, concurrent cancellation versus completion, message reordering, and non-functional reliability. These are Residual risks for EP, BVA, Decision Tables, concurrency testing, and reliability testing.

### Example 2: Guarded document approval

**Requirement basis — Assumption.** A requester can submit a document they own. An authorized approver can approve or reject it, but the requester cannot approve their own document. The owner can reopen a rejected document. Authorization requirements are teaching assumptions unless supplied by a real requirement.

**Modeled object:** one `Document` object. Users are context for guards, not additional modeled objects.

**States and invariants**

| State ID | Meaning/invariant | Entry and outgoing behavior | Status |
| --- | --- | --- | --- |
| `S21` | `Draft` — document content is editable and owned by the requester | Initial; `T21` exits; `T24` returns here | Assumption |
| `S22` | `PendingApproval` — submitted content is immutable; approval or rejection is available only to an authorized non-owner where applicable | `T21` enters; `T22`, `T23`, `T25`, or `T26` are attempted here | Assumption |
| `S23` | `Approved` — approval is finalized and the document is immutable | `T22` enters; terminal for this model | Assumption |
| `S24` | `Rejected` — rejection reason is persisted and the owner may reopen | `T23` enters; `T24` exits | Assumption |

**Guards**

| Guard ID | Predicate | True/false behavior | Status |
| --- | --- | --- | --- |
| `G21` | `actor == document.owner` | True enables submission or reopen; false means a non-owner operation is forbidden | Assumption |
| `G22` | `actor.hasApprovalAuthority == true` | True enables approval or rejection; false returns HTTP 403 | Assumption |
| `G23` | `actor.id != document.owner.id` | True enables approval; false blocks self-approval with HTTP 403 | Assumption |

**Actions**

| Action ID | Operation/effect | Exact observable result | Status |
| --- | --- | --- | --- |
| `A21` | Persist pending approval | State `S22`; submission audit written | Assumption |
| `A22` | Persist approval | State `S23`; approval audit names actor | Assumption |
| `A23` | Persist rejection reason | State `S24`; exact reason persisted | Assumption |
| `A24` | Reopen document | State `S21`; reopen audit written | Assumption |

**Constraints**

| Constraint ID | Rule | Affected elements | Consequence | Status |
| --- | --- | --- | --- | --- |
| `C21` | Only the owner may submit or reopen | `G21`, `T21`, `T24` | Non-owner attempts are forbidden | Assumption |
| `C22` | Approval requires authority and a non-owner actor | `G22`, `G23`, `T22`, `T25`, `T26` | Failed guard returns HTTP 403 and leaves the document unchanged | Assumption |

**Transition classification:** `T25` and `T26` are invalid attempts because the approval guards fail; `T27` is a forbidden event because a non-owner actor is prohibited from reopening. They remain in the selected-negative denominator together, but their classifications and guard failures are distinct.

**Transition table** — reduced, positive-and-selected-negative table for the declared four-state workflow; it is not exhaustive over every state/event/role combination. Selected invalid cases are included.

| ID | Source | Event | Guards | Action/effect | Destination | Status | Constraint IDs | Exact oracle |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `T21` | `S21` | Submit | `G21=true`, valid content | `A21` | `S22` | Valid | `C21` | HTTP 202; state `PendingApproval`; submission audit written |
| `T22` | `S22` | Approve | `G22=true`, `G23=true` | `A22` | `S23` | Valid | `C22` | HTTP 200; state `Approved`; approval audit names actor |
| `T23` | `S22` | Reject | `G22=true` | `A23` | `S24` | Valid | `C22` | HTTP 200; state `Rejected`; exact reason persisted |
| `T24` | `S24` | Reopen | `G21=true` | `A24` | `S21` | Valid | `C21` | HTTP 200; state `Draft`; reopen audit written |
| `T25` | `S22` | Approve | `G22=false` | None | `S22` | Invalid | `C22` | HTTP 403 `APPROVER_REQUIRED`; no state, persistence, audit, notification, or external-side-effect mutation |
| `T26` | `S22` | Approve | `G22=true`, `G23=false` | None | `S22` | Invalid | `C22` | HTTP 403 `SELF_APPROVAL_FORBIDDEN`; no state, persistence, audit, notification, or external-side-effect mutation |
| `T27` | `S24` | Reopen | `G21=false` | None | `S24` | Forbidden | `C21` | HTTP 403 `OWNER_REQUIRED`; rejection reason and all side effects unchanged |

**Cases and coverage**

| Test ID | Transition/sequence | Setup, role, and data | Steps and oracle | Covered IDs |
| --- | --- | --- | --- | --- |
| `ST2-01` | `T21` | `D-201` in `S21`; owner `u1`; complete document; actor `u1` | Submit; HTTP 202, state `S22`, audit exists | `S21,S22,T21,G21=true,A21` |
| `ST2-02` | `Q21=(T21,T22)` | `D-202` in `S21`; owner `u1`; authorized approver `u2` | Submit, then approve; HTTP 200; state `S23`; approval audit exists | `T21,T22,G21=true,G22=true,G23=true` |
| `ST2-03` | `Q22=(T21,T23,T24)` | `D-203`; owner `u1`; authorized approver `u2`; rejection reason `missing-signature` | Submit, reject, owner reopens; states `S21->S22->S24->S21`; exact reason and audits | `T21,T23,T24` |
| `ST2-04` | `T25` | `D-204` in `S22`; actor `u3` lacks authority | Approve; HTTP 403 `APPROVER_REQUIRED`; state and audit unchanged | `T25,G22=false` |
| `ST2-05` | `T26` | `D-205` in `S22`; owner `u1` is authorized approver | Approve as owner; HTTP 403 `SELF_APPROVAL_FORBIDDEN`; state and audit unchanged | `T26,G22=true,G23=false` |
| `ST2-06` | `T27` | `D-206` in `S24`; owner `u1`; rejection reason `missing-signature`; actor `u3` is not the owner | Attempt reopen as `u3`; HTTP 403 `OWNER_REQUIRED`; state remains `S24`; rejection reason, persistence, audit, notification, and external side effects are unchanged | `T27,G21=false,C21` |

Coverage arithmetic:

- Reachable states: `S21-S24`, `4/4 = 100%`.
- Valid transitions: `T21-T24`, `4/4 = 100%`.
- Selected guard outcomes: `G21=true`, `G21=false`, `G22=true`, `G22=false`, `G23=true`, and `G23=false`, `6/6 = 100%`.
- Selected invalid/forbidden transitions: `T25,T26,T27`, `3/3 = 100%`, separate from valid coverage.

The role, authority, and self-approval rules are Assumptions. EP should partition owner/non-owner and authorized/unauthorized actors; BVA may apply to approval expiration if added; Decision Tables should cover combinations of `G21-G23` and any additional context. Role inheritance, delegated authority, simultaneous approvals, and audit retention are Residual risks unless specified.

### Example 3: Asynchronous payment with timeout and retry

**Requirement basis — Assumption.** One payment is submitted to an external provider. A success callback succeeds the payment. A failure or timeout can retry while the attempt count is below three. At attempt three, timeout expires the payment and failure reaches terminal failure. The exact callback and API behavior below is provisional.

**Model metadata:** model `PAYMENT-ST-1.1`; requirement `REQ-PAYMENT-LIFECYCLE-ASSUMED-1`; manual cases; UTC service clock; second precision; callback ordering and eventual consistency are `Question/TBD`. `T36` remains excluded from valid-transition execution coverage until its provider-cancellation contract is confirmed.

**Event model**

| Event ID | Source | Payload/applicability | Duplicate/order/retry behavior | Status |
| --- | --- | --- | --- | --- |
| `E31` | User | Payment ID, amount, currency, provider reference; applicable in `S31` | One submission for this model; duplicate submission is Residual risk | Assumption |
| `E32` | External callback | Payment ID, attempt ID, success result; ordinary applicability in `S32`; selected late/duplicate applicability is modeled separately in terminal scenarios `DUP31` and `STALE31` | Matching callback may arrive once; duplicate terminal behavior is selected separately in `DUP31`, and late success after expiration in `STALE31` | Assumption |
| `E33` | External callback | Payment ID, attempt ID, failure reason; ordinary applicability in `S32`; selected duplicate applicability is modeled separately in terminal scenario `TERM35` | Failure callback may trigger retry below attempt 3; terminal duplicate behavior is selected separately in `TERM35` | Assumption |
| `E34` | Timer | Payment ID, attempt ID, elapsed time; applicable in `S32` | Timer competes with callbacks at the boundary | Assumption |
| `E35` | Scheduled retry job | Payment ID, next attempt time; applicable in `S33` | Job is due only after retry delay; duplicate job is Residual risk | Assumption |

**Guard model**

| Guard ID | Predicate | True/false behavior | Status |
| --- | --- | --- | --- |
| `G31` | Payment data is complete and valid | True enables `T31`; false is outside this positive model | Assumption |
| `G32` | Callback payment ID and attempt ID match the active attempt | True permits callback processing; false is `Question/TBD` | Assumption |
| `G33` | `attempt < 3` | True enables retry; false selects terminal failure or expiration | Assumption |
| `G34` | `elapsed >= 30s` | True enables timeout; false is not a valid timeout | Assumption |
| `G35` | Retry job is due | True enables `T34`; false is `Question/TBD` | Assumption |

**Action model**

| Action ID | Operation/effect | Exact observable result | Status |
| --- | --- | --- | --- |
| `A31` | Create provider attempt | One provider request; state `Pending`; `attempt=1` | Assumption |
| `A32` | Finalize success | State `Succeeded`; one capture and one success event | Assumption |
| `A33` | Record callback failure and enqueue retry | State `Retrying`; failure persisted; no capture | Assumption |
| `A34` | Increment attempt and submit | Attempt increments exactly once; one provider request; state `Pending` | Assumption |
| `A35` | Record timeout and enqueue retry | State `Retrying`; timeout persisted; no capture | Assumption |
| `A36` | Expire payment | State `Expired`; no capture; cancellation policy is `Question/TBD` | Assumption |
| `A37` | Finalize terminal failure | State `Failed`; no retry and no capture | Assumption |

**Constraints**

| Constraint ID | Rule | Consequence | Status |
| --- | --- | --- | --- |
| `C31` | Payment data must be complete before submission | Invalid submission is outside this positive model | Assumption |
| `C32` | Callback must match payment and active attempt | Mismatched callbacks must not finalize payment; response is `Question/TBD` | Assumption |
| `C33` | Retry is allowed only when `attempt < 3` | No fourth attempt | Assumption |
| `C34` | Retry job is processed only when due | Early-job behavior is `Question/TBD` | Assumption |
| `C35` | Timeout requires elapsed time `>= 30s` | Before-boundary timeout is not valid | Assumption |
| `C36` | Attempt 3 has no retry | Failure enters `S35`; timeout enters `S36` | Assumption |

**States and transitions**

| State ID | Meaning/invariant | Entry and outgoing behavior | Terminal/status |
| --- | --- | --- | --- |
| `S31` | Created; amount, currency, and payment reference exist; provider submission has not started | Initial; `T31` exits | Initial |
| `S32` | Pending; one provider attempt is active; `attempt` is in `1..3` | `T31` or `T34` enters; `T32`, `T33`, `T35`, `T36`, or `T37` exits | Active |
| `S33` | Retrying; a retry job is scheduled and no capture has occurred | `T33` or `T35` enters; `T34` exits; callbacks are `Question/TBD` | Recovery/intermediate |
| `S34` | Succeeded; capture/result is finalized and immutable | `T32` enters; duplicate success is `DUP31` | Terminal |
| `S35` | Failed; third attempt failed and no retry remains | `T37` enters; terminal events are `TERM35` | Terminal |
| `S36` | Expired; third attempt timed out and payment is no longer capturable | `T36` enters; late callbacks are `STALE31` | Terminal |

**Transition table** — positive transitions plus selected robustness cases; it is not exhaustive over every callback, timer, queue, payload, and terminal-state combination. The table is authoritative.

| Transition ID | Source | Event/source | Guard | Action | Destination | Status | Constraint IDs | Exact oracle |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `T31` | `S31` | `E31 submit` / user | `G31=true` | `A31` create provider attempt | `S32` | Valid | `C31` | HTTP 202; persisted `Pending`, `attempt=1`, one provider request |
| `T32` | `S32` | `E32 success-callback` / provider | `G32=true` | `A32` finalize success | `S34` | Valid | `C32` | HTTP 200 acknowledgement; state `Succeeded`; one capture and one success event |
| `T33` | `S32` | `E33 failure-callback` / provider | `G32=true`, `G33=true` | `A33` record failure and enqueue retry | `S33` | Valid | `C32,C33` | HTTP 200 acknowledgement; state `Retrying`; failure recorded; no capture |
| `T34` | `S33` | `E35 retry-job` / scheduler | `G35=true` | `A34` increment attempt and submit | `S32` | Valid | `C34` | Attempt increments exactly once; state `Pending`; one provider request |
| `T35` | `S32` | `E34 timeout` / timer | `G34=true`, `G33=true` | `A35` record timeout and enqueue retry | `S33` | Valid | `C33,C35` | Timeout record persisted; state `Retrying`; no capture |
| `T36` | `S32` | `E34 timeout` / timer | `G34=true`, `G33=false` | `A36` expire payment | `S36` | Question/TBD — blocked pending provider-cancellation contract | `C35,C36` | Local state would become `Expired` with no capture, but provider cancellation action and final side-effect oracle are unspecified |
| `T37` | `S32` | `E33 failure-callback` / provider | `G32=true`, `G33=false` | `A37` finalize failure | `S35` | Valid | `C32,C36` | HTTP 200 acknowledgement; state `Failed`; no retry or capture |

**Terminal and robustness behavior**

- `TERM35`: a duplicate failure callback received in `S35` is an Assumption with HTTP 409 `PAYMENT_ALREADY_FAILED`; state, failure reason, audit, retry count, and capture count remain unchanged.
- `DUP31`: a duplicate success callback received in `S34` is an Assumption with HTTP 200 `already_succeeded`; state, capture count, audit count, and external capture calls remain unchanged.
- `STALE31`: a late success callback received in `S36` is an Assumption with HTTP 409 `PAYMENT_EXPIRED`; state and capture remain unchanged.
- Callbacks in `S33`, mismatched callback IDs, early retry jobs, and pre-boundary timers are `Question/TBD`; do not classify them as invalid until the contract defines their exact oracle. The terminal and late-event IDs above are selected scenario elements with explicit provisional oracles, not members of the ordinary `E32`/`E33` applicability domain.

**Robustness cases**

| Test ID | Sequence | Setup and steps | Exact oracle |
| --- | --- | --- | --- |
| `ST3-01` | `Q31=(T31,T32)` | Create `P-301`; submit; deliver matching success callback | States `S31->S32->S34`; one capture; persisted success; callback HTTP 200 |
| `ST3-02` | `Q32=(T31,T35,T34,T32)` | Create `P-302`; submit; wait exactly 30s; retry; deliver success | At boundary timeout is accepted; states `S31->S32->S33->S32->S34`; attempts `1->2`; one capture |
| `ST3-03` | `Q33=(T31,T35,T34,T35,T34,T36)` | Create `P-303`; timeout attempts 1 and 2; retry to attempt 3; timeout at 30s | States end `S36`; no fourth attempt, no capture, expiration persisted |
| `ST3-04` | `Q34=(T31,T35,T34,T35,T34,T37)` | Create `P-304`; arrange two failures and retry jobs; fail attempt 3 | State `S35`; exact failure reason; no retry and no capture |
| `ST3-05` | `DUP31` | Complete `P-305` to `S34`; deliver the same success callback again | **Assumption:** HTTP 200 `already_succeeded`; state, capture count, and audit count unchanged |
| `ST3-06` | `STALE31` | Create `P-306` in `S31`; submit; at attempt 1 timeout at exactly 30s; run the due retry job; at attempt 2 timeout at exactly 30s; run the due retry job; at attempt 3 timeout at exactly 30s; verify `S36`; deliver a late success callback | **Assumption:** HTTP 409 `PAYMENT_EXPIRED`; state and capture unchanged; provider ordering policy remains Question/TBD |

Coverage arithmetic:

- Reachable states: `S31-S36`, `6/6 = 100%`.
- Valid transitions: `T31-T35` and `T37`, `6/6 = 100%`; `T36` is excluded from the valid executed denominator because its provider-cancellation oracle is Question/TBD.
- Event IDs: `E31-E35`, `5/5 = 100%` for the ordinary model; `DUP31`, `TERM35`, and `STALE31` are separately selected robustness event IDs.
- Selected timeout/retry scenarios: first timeout, retry-to-success, and exhausted timeout, `3/3 = 100%`.
- Selected duplicate/stale scenarios: `DUP31, STALE31`, `2/2 = 100%`.
- Selected sequences: `Q31,Q32,Q33,Q34`, `4/4 = 100%`.

The exact duplicate, stale-callback, provider cancellation, callback ordering, queue delivery, and eventual-consistency behavior are Assumptions or Questions/TBD as marked. Concurrency between callback and timeout, replay protection, clock skew, backoff, provider retries, and network reliability are Residual risks requiring reliability, security, and concurrency testing.

### Example 4: State coverage versus transition and sequence coverage

**Requirement basis — Confirmed within this teaching model.** A document-like object can start editing, save repeatedly, and submit. The model intentionally demonstrates metric differences.

**Model metadata:** model `METRIC-ST-1.0`; requirement `REQ-METRIC-DEMO-1`; manual metric sketches; fixtures may start from a defined state when explicitly stated.

| State ID | Meaning/invariant | Entry and outgoing behavior | Status |
| --- | --- | --- | --- |
| `S41` | `Ready` — object is editable but no edit session is active | Initial; `T41` enters `S42`; `T44` is forbidden | Confirmed |
| `S42` | `Editing` — object is editable and save is available | `T41` enters; `T42` loops; `T43` exits | Confirmed |
| `S43` | `Submitted` — submitted content is immutable | `T43` enters; terminal | Confirmed |

**Transition inventory** — reduced positive table plus one selected invalid case.

| Transition ID | Source | Event/source | Guard | Action/effect | Destination | Status | Exact oracle |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `T41` | `S41` | `E41 start-edit` / user | Editing permission | `A41` open edit session | `S42` | Valid | HTTP 200; state `S42`; one edit audit |
| `T42` | `S42` | `E42 save` / user | Valid draft | `A42` persist draft | `S42` | Valid/self-loop | HTTP 200; state remains `S42`; draft version increments once |
| `T43` | `S42` | `E43 submit` / user | Complete draft | `A43` finalize submission | `S43` | Valid | HTTP 202; state `S43`; one submission audit |
| `T44` | `S41` | `E43 submit` / user | Editing required | None | `S41` | Forbidden (selected negative) | HTTP 409 `EDITING_REQUIRED`; no persistence, audit, notification, or submission invocation |

**Actions/events and constraints:** `E41` starts editing and invokes `A41`; `E42` saves valid draft data and invokes `A42`; `E43` submits complete data and invokes `A43`. `C41` is the explicit constraint `S41 ∧ E43 ⇒ reject because editing is required`; its exact oracle is HTTP `409` `EDITING_REQUIRED`, state unchanged, and no persistence, audit, notification, or submission invocation. The event, action, and constraint IDs are all defined in this inventory and are Confirmed within this teaching model.

Selected pairs and sequence:

- `P41 = (T41,T42)`.
- `P42 = (T42,T43)`.
- `Q41 = (T41,T42,T43)`.

Every sequence case records intermediate states and transition IDs. `C4-B` uses a seeded fixture whose defined starting state is `S42`; it does not claim that `S42` is reachable without `T41`.

| Test ID | Case type | Setup and complete context | Steps | Exact oracle | Covered IDs |
| --- | --- | --- | --- | --- | --- |
| `C4-A` | Separate transitions | Fresh `D-401` in initial `S41`; valid draft and editing permission | Execute `T41`; reset fixture; execute `T43` from seeded `S42` | `T41`: HTTP 200, `S42`, one audit. `T43`: HTTP 202, `S43`, one submission audit | `S41,S42,S43,T41,T43,E41,E43,A41,A43,REQ-METRIC-DEMO-1` |
| `C4-B` | Isolated self-loop | Seed `D-402` in defined starting state `S42`; valid draft | Execute `T42` | HTTP 200; state remains `S42`; draft version increments exactly once; one save audit | `S42,T42,E42,A42,REQ-METRIC-DEMO-1` |
| `C4-C` | Pair closure | Fresh `D-403` in `S41`; valid draft | Execute `T41`, then `T42`; execute `T43` separately | First path ends `S42` after one save; separate submit reaches `S43` | `T41,T42,T43,P41,REQ-METRIC-DEMO-1` |
| `C4-D` | Complete sequence | Fresh `D-404` in `S41`; valid draft and permissions | Execute `T41`, `T42`, `T43` consecutively | States `S41->S42->S42->S43`; one edit audit, one save, one submission; final state immutable | `S41,S42,S43,T41,T42,T43,P41,P42,Q41,REQ-METRIC-DEMO-1` |
| `C4-NEG` | Selected invalid transition | Fresh `D-405` in `S41`; no edit action performed | Attempt `T44` by submitting directly | HTTP 409 `EDITING_REQUIRED`; state remains `S41`; no persistence, audit, notification, or invocation | `S41,T44,E43,C41,REQ-METRIC-DEMO-1` |

| Suite | Included cases and execution mapping | State coverage | Valid-transition coverage | Pair coverage | Sequence coverage |
| --- | --- | --- | --- | --- | --- |
| A | `C4-A`: `T41` and `T43` in separate fixtures | `3/3 = 100%` | `2/3 = 66.7%`; `T42` missing | `0/2 = 0%` | `0/1 = 0%` |
| B | Add isolated `C4-B` from seeded `S42` | `3/3 = 100%` | `3/3 = 100%` | `0/2 = 0%`; no pair is consecutive | `0/1 = 0%` |
| C | Add `C4-C`: execute `T41->T42`; keep `T43` isolated | `3/3 = 100%` | `3/3 = 100%` | `1/2 = 50%`; `P41` only | `0/1 = 0%`; `Q41` incomplete |
| D | Use complete `C4-D`: execute `T41->T42->T43` | `3/3 = 100%` | `3/3 = 100%` | `2/2 = 100%` | `1/1 = 100%` |

The invalid `T44` case is separate: `1/1 = 100%`; it is not part of the three-item valid denominator. None of these metrics proves all values, guards, paths, branches, requirements, or non-functional behavior.

## When to use State-Transition Testing

Use it when:

- an object or process has a meaningful lifecycle;
- behavior depends on current state or event history;
- user, system, timer, external, or asynchronous events cause observable changes;
- retries, timeout, expiration, cancellation, recovery, reopening, or terminal states matter;
- the same event behaves differently by state or context;
- invalid or out-of-order events are high risk;
- the team needs traceability from lifecycle requirements to executable sequences;
- a finite or partitionable model is understandable and maintainable.

Do not force it onto a GUI navigation map with no domain-state change, a single unordered input better served by EP/BVA, many independent parameters better served by Pairwise, a multi-condition rule better served by Decision Tables, or an actor goal better served by use-case/scenario testing.

## Limitations and common mistakes

State-Transition Testing does not by itself cover unmodeled values and formats, numeric or time boundaries, large independent-parameter combinations, complex Boolean guards, every path through loops, implementation branches, concurrency interleavings, or security, performance, reliability, usability, accessibility, and compatibility behavior.

Avoid these mistakes:

1. Modeling GUI screens rather than domain behavior.
2. Mixing several objects without a composition rule.
3. Creating a state for every incidental action.
4. Leaving invariants, entry criteria, or boundaries implicit.
5. Treating every arrow as a ready test case.
6. Omitting system, timer, external, callback, or scheduled triggers.
7. Ignoring invalid, duplicate, stale, out-of-order, and terminal events.
8. Confusing invalid attempts with impossible or unreachable model elements.
9. Treating rejection, no-op, error, and recovery as the same oracle.
10. Leaving role, data, time, timezone, or reference-clock context unspecified.
11. Ignoring self-loops, retries, timeouts, reset, reopen, or recovery.
12. Assuming one path covers every behavior of a state.
13. Counting state coverage as transition coverage.
14. Counting transition coverage as sequence or path coverage.
15. Claiming all paths without selecting a finite scope.
16. Inferring precedence from table order without a requirement.
17. Flattening concurrent dimensions without legal-combination rules.
18. Using vague oracles such as “the system works correctly.”
19. Trusting a visually attractive diagram without table, reachability, and arithmetic review.
20. Removing high-risk transitions merely to reduce diagram density.
21. Making asynchronous cases non-repeatable by omitting clock and ordering settings.
22. Assuming a state model proves non-functional quality.

## Complementary techniques

- **Equivalence Partitioning (EP):** partitions payloads, roles, states, and guard inputs into behaviorally distinct classes.
- **Boundary Value Analysis (BVA):** targets timeout, expiration, retry-count, age, quota, and other state-changing edges.
- **Decision Tables:** enumerate combinations of guards and actions when a transition depends on several conditions.
- **Pairwise Testing:** covers interactions among mostly independent platforms, flags, roles, environments, and event parameters.
- **Condition/cause-effect coverage:** analyzes complex Boolean relationships behind guards.
- **Use-case/scenario testing:** covers actor goals and end-to-end flows across several transitions.
- **Model-based testing:** can generate sequences from a formal model, but automation does not remove the need for requirement validation.
- **Error guessing:** adds duplicate, stale, replay, out-of-order, reset, crash-recovery, and historically defective events.
- **Risk-based testing:** prioritizes financial, security, safety, authorization, data-loss, and high-impact transitions.
- **Exploratory testing:** investigates behavior outside the declared model.
- **Security, performance, reliability, accessibility, usability, and compatibility testing:** address non-functional risks not proved by state coverage.

A practical combination is EP for state and guard classes, BVA for timing and count edges, Decision Tables for guard combinations, State-Transition Testing for lifecycle and sequences, Pairwise for independent context combinations, and risk-based, error-guessing, model-based, exploratory, and non-functional follow-up for residual risks.

## Reusable templates

### State model and invariant template

| State ID | State name/meaning | Entry criteria | State invariant | Allowed events/actions | Forbidden events and exact oracle | Persistence/context | Terminal/dead-end status | Requirement reference | Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `STATE-1` |  |  |  |  |  |  |  |  | Confirmed / Assumption / Question/TBD |

### Event, guard, and action template

| Element ID | Type | Meaning/source or formal predicate | Applicable states/context | True/false or expected influence/oracle | Payload/dependencies/timing | Requirement reference | Status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `EVENT-1` | Event / Guard / Action |  |  |  |  |  | Confirmed / Assumption / Question/TBD |

### Transition inventory template

| Transition ID | Source state | Event/trigger | Guard | Action/effect | Destination state | Validity/status | Exact observable oracle | Retry/timeout/loop/terminal attribute | Requirement reference | Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `TRANS-1` |  |  |  |  |  | Valid / invalid / forbidden / impossible / TBD |  |  |  | Confirmed / Assumption / Question/TBD |

### Transition table template

State the orientation and semantics before using the table. This row-oriented form is execution-friendly.

| Transition ID | Source state | Trigger/event | Guard/preconditions | Action/effect | Destination state | Valid/invalid/impossible status | Constraint IDs | Expected response/side effects | Covered cases |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `TRANS-1` |  |  |  |  |  |  |  |  |  |

### Executable test-case template

| Test case ID | Transition/sequence ID | Title/objective | Requirement reference | Priority | Preconditions/setup | Initial/source state | Complete input and context | Event/steps | Guard/precondition evidence | Exact expected response/action/oracle | Expected destination state | Side effects/persistence/notifications | Covered state/event/guard/action/transition IDs | Technique tags | Assumptions/notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `ST-001` | `TRANS-1` |  |  | High/Medium/Low |  |  |  | 1.  2.  3.  |  |  |  |  |  | State-Transition / EP / BVA / Decision Table / negative |  |

For a sequence, record every intermediate state and transition ID. Every positive case starts in a reachable state with complete valid context. Every negative case identifies the violated rule or guard and exact resulting state and side effects.

### Coverage and gap template

| Coverage ID | Metric | Required denominator | Exercised numerator | Percentage | Uncovered items | Exclusions and rationale | Evidence/notes |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `COV-1` | Reachable-state coverage |  |  |  |  |  |  |

### Residual-risk template

| Risk ID | Uncovered or weakly modeled area | Reason not covered | Impact/priority | Complementary technique or follow-up | Status |
| --- | --- | --- | --- | --- | --- |
| `RISK-1` |  |  |  |  | Residual risk / Question/TBD |

## Verification checklist

Before approving a State-Transition design, verify:

- [ ] Scope, modeled object/lifecycle, requirement basis, setup, and exact oracle are documented.
- [ ] The diagram, model, and testing technique are distinguished.
- [ ] One object or explicitly bounded process is modeled; composition is explicit for multiple dimensions.
- [ ] States have stable IDs, behaviorally distinct meanings, entry criteria, invariants, allowed and forbidden actions.
- [ ] States are mutually exclusive and collectively exhaustive for the boundary, or gaps are visible.
- [ ] Initial, terminal, error, recovery, and dead-end states are identified where relevant.
- [ ] GUI screens and incidental actions are not incorrectly treated as domain states.
- [ ] Equivalent states are merged or their distinction is justified.
- [ ] Events have stable IDs, sources, payload/context, applicable states, and ordering/retry semantics.
- [ ] Guards have stable IDs, predicates, true/false outcomes, context, boundaries, and failed-guard oracles.
- [ ] Transitions have stable IDs, source/destination, event, guard, action, validity, and exact outcomes.
- [ ] User, system, timer, external, callback, scheduled, and data-driven events are included when relevant.
- [ ] Self-loops, retries, timeouts, reset/reopen, recovery, duplicates, stale events, ordering, and terminal events are considered.
- [ ] Valid, invalid, forbidden, impossible, unreachable, and unknown behavior are distinct.
- [ ] Invalid cases identify the violated rule and exact rejection/no-op/error/side-effect oracle.
- [ ] Impossible/unreachable elements have exclusion rationales and are not valid-coverage denominator items.
- [ ] Clock, timezone, precision, reference time, ordering, and eventual-consistency assumptions are explicit.
- [ ] Concurrent dimensions use explicit composition or orthogonal modeling.
- [ ] Diagram notation, markers, labels, legend, and table authority are clear.
- [ ] Dense models are split without deleting important risk transitions.
- [ ] Every selected transition maps to a complete executable case with traceability.
- [ ] Sequence cases record every intermediate state and transition ID.
- [ ] State, valid-transition, event, guard, invalid, terminal, invariant, pair, sequence/path, and timing/retry metrics are separate where selected.
- [ ] Coverage uses deduplicated IDs and explicit denominators.
- [ ] State coverage is not presented as transition or path coverage.
- [ ] Transition coverage is not presented as all guards, values, sequences, branches, requirements, concurrency, or non-functional coverage.
- [ ] Uncovered items, exclusions, assumptions, Questions/TBD, and residual risks are visible.
- [ ] EP, BVA, Decision Tables, Pairwise, scenario, condition/cause-effect, model-based, error-guessing, risk-based, and exploratory follow-ups are identified where useful.
- [ ] Oracles are exact rather than “works correctly.”
- [ ] Tables render with matching columns and the document contains no blank scaffolds, duplicate headings, placeholders, or non-English prose.
