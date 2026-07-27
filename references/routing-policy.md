# Routing Policy

## Task Classes

| Task shape | Owner | Preferred effort | Delegation |
| --- | --- | --- | --- |
| One localized, directly verifiable action | Controller | Low or medium | None |
| Deterministic lookup, extraction, formatting, or routine test | Worker | Low | Optional when it frees the controller |
| Bounded implementation, diagnosis, research, or synthesis | Worker | Medium | One worker per independent scope |
| Ambiguous objective, architecture, or cross-scope tradeoff | Controller | High or xhigh | Delegate evidence gathering only |
| Important result with non-obvious failure modes | Independent verifier | Medium or high | Verify after artifacts exist |

## Parallelism Gate

Before considering parallelism, apply a hard direct-work gate: when the controller can read and verify the complete scope in no more than three short tool calls, keep execution local. A list of review dimensions is not evidence of independent work.

Run workstreams concurrently only when all of these are true:

1. Neither needs the other's intermediate result.
2. Their file and external-state ownership does not overlap.
3. Each result can be checked independently.
4. Parallel execution saves meaningful wall-clock time.

Otherwise, order the work as sequential gates.

When independent verification is required, reserve capacity for it or schedule it after the execution wave. Do not consume every slot with open-ended research.

## Escalation Rules

Return control to the controller when any of these occurs:

- a worker discovers that an assumption changes the objective;
- evidence conflicts across workers;
- a task needs access or authority outside its contract;
- the same approach fails twice;
- expected scope expands materially;
- verification cannot establish correctness;
- an action is destructive, externally consequential, or difficult to reverse.

The controller may revise the plan, raise reasoning effort, assign a different specialist, or ask the user for the specific missing decision.

## Budget Rules

- Count specification, coordination, and review as part of delegation cost.
- Do not spawn a worker for work the controller can complete and verify in one short tool sequence.
- Keep work local when expected coordination cost exceeds roughly 25% of the controller's direct-work estimate.
- Prefer fewer well-bounded workers over many narrow workers with high coordination overhead.
- Use all available concurrency only when there are enough independent critical-path tasks.
- Stop expanding the agent tree once additional work no longer changes the decision or deliverable.
- Give every worker a time budget or measurable completion gate. Scale it to the task rather than using one universal duration.
- Default to 1-2 minutes for deterministic inspection or extraction, 2-5 minutes for bounded review or research, and 5-15 minutes for bounded implementation and checks. Use a longer window only when a known tool runtime or task property requires it.
- At expiry, ask once for a compact evidence return. Interrupt a worker that continues beyond the collection window, then finish with the evidence available and state the gap.
