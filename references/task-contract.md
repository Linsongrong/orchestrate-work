# Task Contract

Give each direct child one bounded, task-local work order. The controller records one unified dispatch record in its ledger; it is not history. Use one concise clause per field, reference authoritative artifacts, and include only facts that change a decision, scope, or authority. It replaces separate preflight and contract templates. Omit conditional fields that do not apply; write `none` for absent authority or budget.

```text
Controller-only routing record (not sent to worker):
Current and recommended model/effort: current | none for new agent; recommended
Concrete lower-route insufficiency: lower model and lower effort when above baseline | n/a
Reuse, mandatory-downgrade, and cache decision:

Worker-sent work order:
Execution mode: worker
Remaining delegation depth: 0
Codex agent spawning or Codex task/thread creation: prohibited
orchestrate-work invocation: prohibited

Role, task shape, phase, and assurance profile:
Assigned model/effort: requested | actual when known, including fallback
Objective:
Relevant context:
Owned scope:
Out of scope:
Deliverable:
Acceptance criteria:
Required evidence and check ownership:
Actual side-effect authority and budget:
Completion gate:
Escalate when:

Conditional: prototype hypothesis/minimum experiment/decision threshold/backlog/Gate-repair budget; dependencies or inputs; execution environment/worktree; runtime fallback; shared-worktree map; functional base and excluded historical artifacts; checks already satisfied; final verification assignment; soft timeout/progress signal; mutable remote-state receipt, freshness bound, and reread trigger.
```

The preamble is exact and always copied into the dispatch message; do not replace it with inherited context or a ledger link. All controller-only fields and all applicable worker work-order fields block dispatch when absent; conditional fields are required only when applicable. The worker receives only work-order fields. The recommended route is selected under [routing-policy.md](routing-policy.md); when above baseline, record concrete lower-model and lower-effort gaps. For a new agent, record actual route after creation when known. A delta followup is allowed only under the exact-route reuse rule in that policy; repeat the preamble and state objective delta, findings, completion gate, assigned route, and applicable functional base plus current or superseded identity/digest; retain the reuse decision in the controller record. Otherwise use a full record or fresh agent.

For a prototype executor, keep the tightly coupled experiment and author checks in one candidate. Contract one decision verifier only after it is complete. Do not create verifier contracts for intermediate artifacts.

## Return

```text
Status: complete | partial | blocked
Result or artifact locations:
Artifact handoff receipt (conditional): absolute path, type, size, readability, stable identity/digest
Evidence and checks: targeted checks; candidate, command, environment, result, and owner when reusable or independently verified
Requested and actual model/effort, including fallback if used:
Material decisions or findings:
Unresolved risks:
Controller action needed:
```

Use `complete` only when the completion gate is met. Use `partial` only for a controller-requested checkpoint or a reusable subset blocked by external dependency/state, authority/budget, or genuine decomposition/user decision; state completed work, remaining work, ownership, and next action. Use `blocked` only when no safe in-scope progress remains without external change or user authority. Routine recoverable command, tool-path, JSON/reference, restart/socket/readiness, and healthy observation-timeout faults remain worker responsibility and do not justify partial. Keep bulky logs outside the return unless needed to resolve failure or conflict.

## Role Boundaries

- **All workers:** solve only the work order; do not delegate, spawn agents, create/fork Codex tasks or threads, invoke this skill, silently inherit a route, mutate unowned state, or expand scope. Domain or business Task objects explicitly required by the objective and assigned side-effect authority are allowed. Preserve user changes. Stop at the completion gate and report failures, uncertainty, scope pressure, and blockers plainly.
- **Scout:** gather only needed decision evidence; return sources, confidence, and unresolved questions, without implementation.
- **Executor:** implement within ownership, run assigned author checks, repair returned findings, and freeze its candidate during verification. Prefer repository implementation, then standard/native facilities, then existing dependencies before minimum new code. Do not add speculative abstractions, files, dependencies, configuration, compatibility layers, or tests without a present acceptance or risk need. Prefer one shared root-cause repair over symptom patches when the flow supports it. It cannot accept its own artifact.
- **Integrator:** combine accepted inputs only within assigned paths and run assigned cross-component checks. Its output is separately verified.
- **Verifier:** independently inspect a complete candidate against authoritative criteria and raw evidence; identify the authoritative source of rules and evidence and challenge self-authenticating or co-mutable candidate/proof loops when relevant. Do not edit or repair. Check explicit requirements and applicable trust, authority, side-effect, conclusion, replayability, accessibility, safety, security, and data-loss concerns. Report severity-ordered findings, passed checks, residual risk, and immutable accept/reject verdict. Status-only/non-normative documentation may be excluded only when the contract says so.
