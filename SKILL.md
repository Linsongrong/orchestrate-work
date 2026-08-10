---
name: orchestrate-work
description: Coordinate non-trivial or long-running work through a controller-first orchestration loop with bounded specialist agents, durable task state, adaptive model routing, and independent verification. Use when the user explicitly asks for delegation, subagents, parallel work, or orchestration; when independent workstreams can run concurrently; when substantial discovery, implementation, testing, or tool output should be isolated from the main context; or when an important result needs an independent check. Explicit invocation strongly presumes orchestration, except for genuinely trivial and local tasks.
---

# Orchestrate Work

If you are a delegated child agent operating under another controller's task contract, enter **worker mode**: do not invoke this skill in either mode, do not create agents or tasks, and do not replan the objective. Use ordinary specialist skills needed for the assigned role, or return a concrete escalation to the controller.

Choose one mode:

- **Direct mode:** Complete only a genuinely trivial, local task whose implementation and verification require no meaningful decomposition or repeated search, implementation, or debugging cycles. Explicit invocation strongly presumes this exception does not apply.
- **Orchestration mode:** Act as manager and judge. Delegate substantive discovery, implementation, integration, and verification. Do not own a main execution branch.

In orchestration mode, read [routing-policy.md](references/routing-policy.md) before planning the first wave or choosing models, parallelism, ownership, worktrees, recovery, or thread escalation. Read [task-contract.md](references/task-contract.md) before assigning any agent role.

## Establish Control State

1. Perform only enough reconnaissance to frame the work.
2. Create a durable task ledger containing:
   - objective, acceptance criteria, and non-goals;
   - current phase, assurance profile, authority, and side-effect budget;
   - settled decisions and key assumptions;
   - dependencies and workstream status;
   - current work and next ready work;
   - risks and blockers;
   - candidate identities or digests, evidence receipts, verification ownership, and reuse decisions;
   - dispatch routes, including requested and actual model and reasoning effort, rationale, and runtime fallbacks.
3. Use task or plan state when available. Do not create a repository file solely for the ledger unless the task requires one.
4. Set the phase assurance profile before dispatch: default to `formal`; use `prototype` only when the user explicitly asks for a rapid prototype; use `release` only when explicitly requested. Do not downgrade an active phase without explicit user authorization.
5. Update the ledger after every wave and every material decision. After context compaction, restore the ledger before continuing.
6. Admit a new side branch only when it is required by an acceptance criterion, blocks current work, or disproves a key assumption. Ask the user before a material scope change; otherwise backlog it. Reassess, reroute, or backlog a side branch when it repeats the same failure, produces no new evidence, consumes the main line with noncritical hardening, or does not converge after two rounds.

## Operate The Controller Loop

1. Map dependencies and split the objective into bounded, independently acceptable workstreams.
2. Assign direct child agents as scouts, executors, integrators, or verifiers. Every dispatch must explicitly select and contract a model and reasoning effort, including an intentional same-as-controller choice; silent inheritance is prohibited. Do not permit child agents to spawn.
3. Dispatch all ready, independent, disjoint workstreams in a wave within available capacity. Commonly use two execution slots while retaining capacity for verification; preserve serial order when dependencies require it.
4. Before a costly, mutable, irreversible, or sensitive external action, allocate a ledger budget and include it in the contract. Do not let a worker retry after the budget is exhausted.
5. Give every agent exclusive ownership and task-local context. Assign check ownership before dispatch: executor targeted author checks, verifier orthogonal acceptance and risk checks, integrator cross-component checks, and one final phase-level integrated check run by a specifically contracted verifier. Require direct artifact production and a compact evidence return.
6. While agents run, maintain the ledger, make decisions, prepare later contracts, and resolve blockers. Do not duplicate delegated work or undertake another substantive execution branch.
7. Collect results at completion gates. Use soft timeouts and stall detection: ask once for compact progress, continue healthy work, and interrupt only stalled, repeatedly failing, out-of-scope, or critical-path-blocking work.
8. Integrate accepted results through an agent assigned the integrator role when integration is substantive.
9. Reuse trusted evidence only when its candidate identity or digest, command, environment, result, and owner still match. Rerun after a candidate or environment change, when evidence is missing or untrusted, or when independent reproduction is itself required.
10. Independently verify important artifacts and always have a specifically contracted verifier run one final integrated verification appropriate to the deliverable. The controller judges returned evidence and verdicts; it does not duplicate delegated checks unless independent reproduction is required.
11. On rejection, return the first failure to the original executor. Continue an acceptance-critical route while each iteration produces new evidence and approaches acceptance. Diagnose a repeated failure as a contract or approach problem before escalating model or effort; reassess and reroute when two rounds do not converge, rather than imposing a universal one-repair limit.
12. Freeze an accepted phase before starting another: record completed deliverables, unfinished non-goals, the next objective, its profile, and its authority. Do not treat "continue" as permission to expand an unbounded prior phase.
13. Continue until every acceptance criterion passes. Context integrity and reliable evidence outrank minimum total token cost.

## Enforce Controller Boundaries

Reserve controller work for minimal orientation, decomposition, decisions, ledger maintenance, dispatch, conflict resolution, acceptance, user communication, and tiny glue. Do not take a main branch that needs multiple search, implementation, or debugging cycles. Record an emergency takeover in the ledger, including why delegation could not continue and how the result will be independently verified.

When the user changes direction, analyze impact, stop only affected branches, update the ledger and contracts, and reject stale results from integration.

Send user updates only when creating the ledger and dispatching the first wave, completing a verification wave, changing the plan, scope, or a material decision, encountering a blocker that requires the user, or completing the task. The controller synthesizes these updates; do not forward raw child progress. Default to: current objective, phase result or progress, convergence, stop condition, and user action needed. Expand evidence or worker detail only on request.

## Finish

Before delivery:

1. Confirm every acceptance criterion and required integrated check passed.
2. Confirm the latest accepted or rejected Gate is recorded in the ledger, or explicitly marked `pending integration`; do not declare the phase complete otherwise.
3. Confirm no necessary agent remains running and no stale result was integrated.
4. Reconcile conflicts and record unresolved risk accurately.
5. Deliver one coherent outcome grounded in artifacts and evidence.
