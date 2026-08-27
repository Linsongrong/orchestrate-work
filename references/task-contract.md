# Task Contract

Give every child agent one bounded contract. Keep it task-local and omit conditional fields that do not apply. Do not turn the template into a checklist that expands the worker's scope. Prefer one short clause per field and reference ledger keys or artifact paths instead of restating their contents.

Start every child dispatch message with this exact preamble. Copy it into the message itself; never replace it with a ledger reference or assume the child inherits it:

```text
Execution mode: worker
Remaining delegation depth: 0
Agent spawning or task creation: prohibited
orchestrate-work invocation: prohibited
```

## Core Contract

The four-line preamble, fixed dispatch preflight, role/profile, explicit model route, objective, ownership, deliverable, acceptance, check ownership, authority, completion, and escalation fields are always required. Treat both `spawn_agent` and `followup_task` as dispatches; missing preflight fields block dispatch. Write `none` instead of adding explanatory boilerplate when no authority or budget applies.

Before the dispatch message, record this fixed preflight in the controller ledger:

```text
Role:
Task shape:
Current model/effort: model/effort | none for new agent
Recommended model/effort:
Why the next lower model is insufficient:
Why the next lower effort is insufficient:
Task-shape change since current route: none | describe
High-tier budget and renewal decision: n/a | describe
Mandatory downgrade decision: n/a | describe
Context-transfer/cache-aware reuse decision:
Ownership:
Acceptance gate:
Side-effect budget:
Reuse/new-agent decision and rationale:
```

`Recommended model/effort` is the lowest sufficient complete route for the current task shape. Write `n/a` for `Why the next lower model is insufficient` when the selected model is Luna; otherwise state the concrete lower-model insufficiency. Write `n/a` for `Why the next lower effort is insufficient` when the selected effort is `medium`; otherwise state why `medium` is insufficient. Never use only "complex", "important", or "high-risk" as either reason. For fresh creation, use the name `<role>_<work>_<model>_<effort>` with valid lowercase underscore tokens, such as `executor_api_terra_medium`. This is only an observability hint for the requested creation route, not actual-route evidence; followup reuse keeps the existing name. Model and effort are sticky after creation; `followup_task` cannot reroute a reused agent. Reevaluate the task shape before followup and explicitly record the current task shape, recommended route, and reuse or mandatory-downgrade decision. Reuse only when the current route exactly matches the recommendation and related work is worth batching; a mismatch requires a fresh lower or otherwise suitable agent. Same-executor continuity never overrides a mandatory downgrade, including retaining Sol for Terra/Luna-shaped work. The cache decision may preserve a genuinely same-task, same-route agent, but must not assume cross-agent or cross-model cache hits or bypass a distinct verifier. If the recommended route is unavailable, select and record an explicit fallback on that fresh agent. For a new agent, record only the requested route until creation confirms an actual route; after either dispatch, record requested and actual route where known and any fallback.

```text
Execution mode: worker
Remaining delegation depth: 0
Agent spawning or task creation: prohibited
orchestrate-work invocation: prohibited
Role: scout | executor | integrator | verifier
Phase and assurance profile: prototype | formal | release
Model, reasoning effort, and brief routing rationale: lowest sufficient route and concrete lower-route insufficiency when applicable
Objective:
Relevant context or ledger reference:
Owned scope:
Out of scope:
Deliverable:
Acceptance criteria:
Required evidence and check ownership:
Authority and side-effect budget: write `none` when no budgeted action is allowed or needed
Completion gate:
Escalate when:
```

## Conditional Fields

Add only when applicable:

```text
Prototype experiment: hypothesis, minimum experiment, decision threshold, nonblocking backlog, Gate/repair budget
Dependencies and inputs: when they are not obvious from relevant context
Execution environment or worktree: when location affects execution or evidence
Runtime fallback: when the requested route is unavailable at dispatch
Shared-worktree ownership map or reference: for concurrent writers
Functional base and excluded historical artifacts: for repair or re-verification
Checks already satisfied and must not be rerun: when reusing evidence
Final verification assignment: for a final verifier or a prototype verifier owning both the focused Gate and final check
Soft timeout and progress signal: for work requiring active monitoring
Mutable remote-state receipt: observed state/time, identity or revision when available, freshness bound, and reread trigger; only for state that may change before dependent mutation or acceptance
```

For a prototype executor, put all tightly coupled design, adapters, mocks, implementation, and targeted author checks needed by the minimum experiment into one contract. Reference the ledger experiment instead of copying its full hypothesis and backlog into the contract. Do not create verifier contracts for intermediate artifacts. Contract one decision verifier after that candidate is complete.

Use a compact delta followup only when the same role, functional workstream/candidate lineage, ownership, authority, budget, and exact recommended route remain. It must repeat the non-delegation preamble and explicitly state the objective delta, new findings, completion gate, current task shape, recommended route, reuse decision, functional base, and current or superseded candidate identity/digest; unchanged fields may reference the prior contract. Record the resulting identity/digest before reusing evidence or dispatching verification, so evidence for the superseded digest cannot transfer. Any material change requires a full contract, and a route mismatch requires a fresh suitable agent.

Require this compact return exactly:

```text
Status: complete | partial | blocked
Result or artifact locations:
Artifact handoff receipt: absolute path, type, size, readability, digest or n/a; observed immediately before return; write `none` when no artifact was assigned
Evidence and checks: include candidate, command, environment, result, and owner when evidence may be reused
Requested and actual model/effort, including fallback if used:
Material decisions or findings:
Unresolved risks:
Controller action needed:
```

Keep raw transcripts, logs, and large evidence collections outside the return unless the controller needs them to resolve a failure or conflict.

Use `complete` only when the assigned completion gate is met. Use `partial` only for a controller-requested checkpoint, or a reusable subset that cannot progress because of external dependency/state, authority or budget, or a genuine decomposition or user decision; state the completed reusable result, remaining work, reason, whether ownership is retained or released, and next owner/action. Use `blocked` only when no safe in-scope progress remains without external change or user authority. Routine recoverable command, tool-path, JSON/reference, transient restart/socket/readiness, and healthy observation-timeout faults remain executor responsibility within side-effect, retry, and convergence limits; they do not justify `partial`. Stop retrying on exhausted budget, no new evidence, or nonconvergence under the existing recovery rules.

## Common Rules

- Treat the contract as authoritative and solve only its objective.
- Do not spawn an agent, create or fork a task, ask another agent to continue the work, or invoke `orchestrate-work`. If the objective needs decomposition, stop and return the specific decomposition need to the controller.
- Use the explicitly contracted model and reasoning effort. Do not silently inherit a route; report any runtime fallback with its reason and confidence impact.
- Use only task-local context; request a specific missing fact instead of the parent conversation.
- Do not expand scope or mutate unowned state. You may use ordinary non-orchestration specialist skills needed to complete the contract.
- Implement only what the current acceptance criteria and profile risk floor require; backlog speculative completeness unless it materially affects scope, cost, or acceptance. Uncertainty alone is not a present need: additions require a current acceptance criterion, observed failure, profile risk floor or trust boundary, or conclusion/replayability need. Do not add unrequested abstractions, files, dependencies, configuration, compatibility layers, or tests without one.
- Preserve existing user changes and avoid unrelated edits.
- Produce artifacts directly when assigned ownership.
- Immediately before returning, confirm each declared artifact exists and is readable and report its absolute path, type, size, and digest when applicable. Do not claim a missing or mismatched artifact is complete.
- Before a budgeted side effect, report the intended consumption and verify remaining budget. Exhaustion blocks only that named side effect and its retries; continue otherwise-ready work that consumes none of it, but stop and escalate immediately before consuming it without renewed authorization.
- Stop at the completion gate, not at an arbitrary elapsed-time cutoff.
- At a soft timeout, return compact progress when asked and continue if progress is healthy.
- Report failed checks, uncertainty, scope pressure, and blockers without hiding them behind `complete`.

## Role Rules

### Scout

- Gather only evidence needed for the stated decision or execution branch.
- Return findings, sources, confidence, and unresolved questions; do not implement unless given a new executor contract.

### Executor

- Implement within exclusive ownership and run the targeted author checks assigned in the contract.
- Understand the relevant caller and execution flow first. Reuse repository code, then the standard library, native platform, or installed dependencies before writing minimum new code. For a bug, prefer one shared root-cause repair over symptom patches when the flow supports it.
- Repair own work when the controller returns independent verification findings.
- Treat a submitted candidate as frozen while independent verification runs. For a repair, use the stated functional base and do not absorb historical verifier artifacts into the candidate.
- Do not issue the acceptance verdict for own artifacts.

### Integrator

- Combine accepted inputs, own overlap and integration adjustments, and run assigned cross-component checks.
- Treat generated integration artifacts as own work requiring a separate verifier.
- Modify only assigned integration paths; do not alter a frozen candidate or a verifier verdict.

### Verifier

- Inspect the artifact independently against the supplied criteria and raw evidence. First identify the authoritative source of rules and evidence; challenge self-authenticating or co-mutable candidate/proof loops when relevant.
- Begin only with a complete candidate, verified handoff/identity, author evidence for the current decision threshold and risk floor, and no unresolved ordinary author-debugging issue. A partial candidate is never Gate-ready.
- Confirm that the candidate did not minimize away explicit requirements, trust-boundary validation, security, data-loss prevention, accessibility basics, physical calibration where relevant, conclusion validity, replayability, authority, or side-effect safety. Treat each required check as evidence for a concrete failure mode; do not add a duplicate Gate for ceremony.
- Report findings ordered by severity, passed checks, residual risk, and an explicit accept or reject verdict.
- Do not edit, repair, or reimplement the artifact. Send failures to the controller.
- Treat the verdict as an immutable verification artifact. Status-only and explicitly non-normative documentation artifacts may be out of scope when the contract says so; rules, prompts, schemas, policies, and other behavior-defining text require appropriate verification.

## Verification Contract

For a verifier, make the general contract concrete with:

```text
Objective: Determine whether the supplied artifacts satisfy every acceptance criterion.
Artifacts and evidence:
Acceptance criteria:
Checks to run:
Checks already satisfied and must not be rerun: omit when none
Independent reproduction required: yes | no; reason
Final integrated verification: yes | no; identify the integrated candidate when yes
Out of scope: Modifying, repairing, or reimplementing the artifacts.
Completion gate: Clear accept/reject verdict with findings, passed checks, and residual risk.
```
