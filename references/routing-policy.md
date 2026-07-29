# Routing Policy

## Sol Routing

| Task shape | Owner | Model and effort | Delegation |
| --- | --- | --- | --- |
| Localized action or tightly coupled reasoning | Controller | `gpt-5.6-sol`, current controller effort | None |
| Deterministic lookup, extraction, formatting, or routine test | Worker | `gpt-5.6-sol`, low | Only when it isolates meaningful context or joins a larger delegated scope |
| Bounded implementation, diagnosis, research, or synthesis | Worker | `gpt-5.6-sol`, medium | One worker per cohesive independent scope |
| Ordinary independent verification | Worker | `gpt-5.6-sol`, medium | Only after a material artifact exists |
| Ambiguous objective, architecture, cross-scope tradeoff, or final acceptance | Controller | `gpt-5.6-sol`, high or xhigh | Delegate evidence gathering only |

Treat the user's current controller selection as authoritative because a skill cannot change the already-running controller. For every worker override, set `fork_turns="none"` and include the necessary task-local context in the assignment. Do not inherit the full controller conversation merely for convenience. If the current `spawn_agent` interface lacks `model` or `reasoning_effort`, use the available runtime default and report the limitation when it is material.

## Delegation Gate

Delegate only when all four checks pass:

1. **Boundary:** the assignment is one cohesive unit with disjoint file or external-state ownership.
2. **Verifiability:** the result can be accepted from artifacts, checks, or concrete evidence.
3. **Autonomy:** the worker does not need recurring architectural or product decisions.
4. **Payoff:** the worker isolates substantial execution/context, reduces the critical path, or provides meaningful independent verification.

Keep work local when it fits in three short tool calls without substantial output, when the task contract would be almost as long as the work, or when reviewing the result would require repeating the worker's investigation.

## Parallelism Gate

Start with one worker. Consider parallelism only after the delegation gate passes for each proposed workstream.

Run workstreams concurrently only when all of these are true:

1. Neither needs the other's intermediate result.
2. Their file and external-state ownership does not overlap.
3. Each result can be checked independently.
4. Parallel execution saves meaningful wall-clock time.

Otherwise, order the work as sequential gates.

Add a second execution worker only when parallel execution saves meaningful time. More than two execution workers is exceptional and requires a clear dependency graph. Reserve capacity for required verification or schedule it after execution. Do not consume every slot with open-ended research.

## Escalation Rules

Return control to the controller when any of these occurs:

- a worker discovers that an assumption changes the objective;
- evidence conflicts across workers;
- a task needs access or authority outside its contract;
- the same approach fails twice;
- expected scope expands materially;
- verification cannot establish correctness;
- an action is destructive, externally consequential, or difficult to reverse.

The controller may revise the plan, take the task back at high or xhigh effort, or ask the user for the specific missing decision. Do not make a worker silently increase its own scope or reasoning role.

## Budget Rules

- Count specification, coordination, and review as part of delegation cost.
- Do not spawn a worker for work the controller can complete and verify in one short tool sequence.
- Count avoided controller context as a real benefit, but require enough benefit to outweigh writing and reviewing the task contract.
- Prefer fewer well-bounded workers over many narrow workers with high coordination overhead.
- Do not use available concurrency as a target.
- Stop expanding the agent tree once additional work no longer changes the decision or deliverable.
- Give every worker a time budget or measurable completion gate. Scale it to the task rather than using one universal duration.
- Default to 1-2 minutes for deterministic inspection or extraction, 2-5 minutes for bounded review or research, and 5-15 minutes for bounded implementation and checks. Use a longer window only when a known tool runtime or task property requires it.
- At expiry, ask once for a compact evidence return. Interrupt a worker that continues beyond the collection window, then finish with the evidence available and state the gap.
