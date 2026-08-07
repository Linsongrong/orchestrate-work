# Routing Policy

Use this policy in orchestration mode for role, model, parallelism, ownership, recovery, and escalation decisions.

## Route Roles And Models

| Work shape | Role | Preferred model and effort |
| --- | --- | --- |
| Deterministic lookup, extraction, formatting, or routine checks | Scout or verifier | `gpt-5.6-luna`, task-appropriate effort |
| Routine bounded implementation, tests, docs, or ordinary diagnosis | Executor or integrator | `gpt-5.6-terra`, task-appropriate effort |
| Complex implementation, research, diagnosis, conflict analysis, or important independent verification | Any matching specialist role | `gpt-5.6-sol`, task-appropriate effort |
| Intent, decomposition, consequential decisions, conflict resolution, and final acceptance | Controller | Current controller; prefer high-reasoning capability |

Prefer the stronger model when routing is uncertain. If model or reasoning overrides are unavailable, use the runtime fallback and record any material confidence impact.

- **Scout:** Produce bounded discovery or evidence. A scout may later execute only under a new executor contract.
- **Executor:** Produce or repair artifacts and run relevant local checks. An executor may repair its own rejected work.
- **Integrator:** Own substantive merging, cross-component adjustments, and integration checks. Treat integration as execution, not controller glue.
- **Verifier:** Independently judge artifacts and report findings. Never modify or fix the artifact being verified.

An executor or integrator must never verify its own artifact. Use a distinct verifier for important artifacts. Avoid a separate verifier only for low-risk work that is completely established by necessary mechanical checks; final integrated verification remains mandatory.

## Dispatch In Dependency Waves

1. Identify ready workstreams from the ledger.
2. Dispatch all ready work that is independent, disjoint, and useful within capacity. Commonly run two executors and preserve room for a verifier.
3. Do not serialize independent work by default, force parallelism onto dependent work, or fill slots with speculative research.
4. End the wave at its completion gates, update the ledger, resolve rejected or conflicting results, then dispatch the next ready wave.

Default to one layer of direct child agents. Set `fork_turns` to `"none"` and provide only task-local context. A perceived need for most of the conversation signals poor decomposition; refine the contract instead. Child agents may not spawn other agents.

## Own State Explicitly

Assign exclusive ownership of every file, directory, artifact, and external mutation. If ownership overlaps, serialize the work or appoint one owner.

Use a shared worktree only when changes are truly disjoint and relevant checks cannot observe a partially modified repository. Otherwise use isolated worktrees and assign an integrator to combine accepted results. Apply the same rule to external systems: concurrent agents must not mutate overlapping state.

Agents edit or produce their owned artifacts directly and run checks appropriate to them. Keep raw logs and bulky exploration in the worker context; return compact evidence and artifact locations unless failure or conflict requires controller inspection.

## Monitor By Progress

Give each task a measurable completion gate and a soft timeout appropriate to expected work and tool latency. Do not use fixed universal minute limits.

At a soft timeout, request one compact progress report. Continue a worker that shows healthy, in-scope progress. Interrupt only when it is stalled, repeats a failed approach, leaves scope, or blocks the critical path without useful progress.

## Recover And Stop

- First verification failure: return findings to the original executor for repair.
- Second failure: reassess assumptions, contract, model, agent, or approach before redispatch.
- Continued failure: stop that route. Try another in-scope route when available; otherwise report a concrete blocker or request the needed user decision.
- Worker loss or tool failure: preserve accepted artifacts and ledger state, then redispatch or use another in-scope route.
- Conflicting evidence: keep disputed output unaccepted until the controller resolves it or commissions targeted evidence.
- Scope discovery: admit only acceptance-critical, blocking, or assumption-disproving work. Backlog optional work and ask the user about material scope change.

Do not let the controller casually repair failed delegated work. An emergency takeover is allowed only when it is recorded in the ledger and independently verified.

## Escalate To User-Owned Tasks

Use direct child agents by default. Suggest a new user-owned Codex task only for work that is long-lived and independently steerable or needs strong worktree isolation. Creating or handing off such a task requires explicit user authorization; do not substitute it for ordinary child-agent orchestration.

When the user changes the objective, perform impact analysis, interrupt only affected branches, revise the ledger and contracts, and do not integrate results produced against stale requirements.
