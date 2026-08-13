---
name: orchestrate-work
description: Coordinate non-trivial or long-running work through a controller-first orchestration loop with bounded specialist agents, durable task state, adaptive model routing, and independent verification. Use when the user explicitly asks for delegation, subagents, parallel work, or orchestration; when independent workstreams can run concurrently; when substantial discovery, implementation, testing, or tool output should be isolated from the main context; or when an important result needs an independent check. Explicit invocation strongly presumes orchestration, except for genuinely trivial and local tasks.
---

# Orchestrate Work

If you are a delegated child agent operating under another controller's task contract, enter **worker mode**: do not invoke this skill in either mode, do not create agents or tasks, and do not replan the objective. Use ordinary specialist skills needed for the assigned role, or return a concrete escalation to the controller.

Choose one mode:

- **Direct mode:** Complete only a genuinely trivial, local task whose implementation and verification require no meaningful decomposition or repeated search, implementation, or debugging cycles. Explicit invocation strongly presumes this exception does not apply.
- **Orchestration mode:** Act as manager and judge. Delegate substantive discovery, implementation, integration, and verification. Do not own a main execution branch.

Within orchestration mode, the controller may complete a **direct unit** when it is mechanical, local, low-risk, and needs no independent judgment or repeated search, implementation, or debugging. Examples include checking artifact existence, reconciling a digest, or updating non-normative status. Record the unit and its direct check in the ledger; do not create a separate wave or Gate for it. A change to behavior, candidate identity, authority, side effects, prompts, schemas, or policies is not a direct unit merely because the diff is small; keep it in the applicable candidate and Gate.

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
   - dispatch routes, including requested and actual model and reasoning effort, rationale, and runtime fallbacks;
   - for each `prototype` experiment: hypothesis, minimum experiment, decision threshold, nonblocking backlog, and Gate/repair budget.
3. Use task or plan state when available. Do not create a repository file solely for the ledger unless the task requires one.
4. Set the phase assurance profile before dispatch: default to `formal`; use `prototype` only when the user explicitly asks for a rapid prototype; use `release` only when explicitly requested. Do not downgrade an active phase without explicit user authorization.
5. Update the ledger after every wave and every material decision. After context compaction, restore the ledger before continuing.
6. Admit a new side branch only when it is required by an acceptance criterion, blocks current work, or disproves a key assumption. Ask the user before a material scope change; otherwise backlog it. In `prototype`, protect the minimum experiment from noncritical hardening. Reassess, reroute, or backlog a side branch when it repeats the same failure, produces no new evidence, consumes the main line with noncritical hardening, or does not converge after two rounds.

## Operate The Controller Loop

1. Map dependencies and split the objective into bounded, independently acceptable workstreams. In `prototype`, organize the main line around the hypothesis experiment, not individual files, fields, wrappers, design stages, mocks, or defects. Prefer one executor for a tightly coupled minimum experiment and keep its design, implementation, adapters, and author checks in one candidate. Add a scout or parallel executor only for an independently blocking unknown or a truly disjoint input that reduces critical-path time without creating extra integration or Gate work.
2. Assign direct child agents as scouts, executors, integrators, or verifiers. Start every child dispatch message with the exact non-delegation preamble from [task-contract.md](references/task-contract.md); do not rely on inherited context, ledger links, or the child reading this skill. Every dispatch must explicitly select and contract a model and reasoning effort, including an intentional same-as-controller choice; silent inheritance is prohibited. If the assigned work needs further decomposition, the child must return a concrete escalation instead of spawning an agent or creating a task.
3. Dispatch all ready, independent, disjoint workstreams in a wave within available capacity. Commonly use two execution slots while retaining capacity for verification; preserve serial order when dependencies require it.
4. Before a costly, mutable, irreversible, or sensitive external action, allocate a ledger budget and include it in the contract. Exhaustion blocks only that named side effect and its retries; dispatch or continue otherwise-ready work whose contract consumes none of it.
5. Give every agent exclusive ownership and task-local context. Assign check ownership before dispatch: executor targeted author checks, verifier orthogonal acceptance and risk checks, integrator cross-component checks, and the phase-level final check required by the assurance profile. Require direct artifact production, an immediate artifact handoff receipt, and a compact evidence return.
6. While agents run, maintain the ledger, make decisions, prepare later contracts, and resolve blockers. Do not duplicate delegated work or undertake another substantive execution branch.
7. Collect results at completion gates. Before accepting a worker return or dispatching a verifier, use a direct unit to confirm each declared artifact exists and is readable in the declared environment and that its type, size, and digest when applicable match the handoff receipt. Treat a mismatch as an incomplete handoff, not a new quality Gate. Use soft timeouts and stall detection: ask once for compact progress, continue healthy work, and interrupt only stalled, repeatedly failing, out-of-scope, or critical-path-blocking work.
8. Integrate accepted results through an agent assigned the integrator role when integration is substantive.
9. Reuse trusted evidence only when its candidate identity or digest, command, environment, result, and owner still match. Rerun after a candidate or environment change, when evidence is missing or untrusted, or when independent reproduction is itself required.
10. Independently verify important artifacts. In `formal` and `release`, have a specifically contracted verifier run one final integrated verification appropriate to the deliverable. In `prototype`, do not create separate independent design, mock, component, field, or preflight Gates for one experiment: the executor owns those targeted author checks. Dispatch the independent decision verifier only after the minimum experiment candidate and its author evidence are ready. That focused Gate may also be the final check when the deliverable is one candidate with no substantive integration and the verifier is explicitly contracted for both; otherwise use a separate independent integrated Gate. The controller judges returned evidence and verdicts; it does not duplicate delegated checks unless independent reproduction is required.
11. On rejection, return the first failure to the original executor. For `prototype`, default to one narrow repair and re-Gate for the same hypothesis experiment. Another rejection forces route reassessment; further repair is allowed only when the controller records new evidence, acceptance criticality, and a credible convergence path. Otherwise backlog the issue, redesign the experiment, change route, or request authority to move into a harder assurance phase. For `formal` and `release`, continue an acceptance-critical route while each iteration produces new evidence and approaches acceptance. In every profile, diagnose a repeated failure as a contract or approach problem before escalating model or effort, and reassess when two rounds do not converge rather than imposing a universal hard repair limit.
12. Freeze an accepted phase before starting another: record completed deliverables, unfinished non-goals, the next objective, its profile, and its authority. Do not treat "continue" as permission to expand an unbounded prior phase.
13. Continue until every acceptance criterion passes. In `prototype`, blocking acceptance criteria are limited to the recorded decision threshold and prototype risk floor; do not promote backlog items into acceptance criteria without a material scope decision. Context integrity and reliable evidence outrank minimum total token cost.

## Enforce Controller Boundaries

Reserve controller work for minimal orientation, decomposition, decisions, ledger maintenance, dispatch, conflict resolution, acceptance, user communication, and tiny glue. Do not take a main branch that needs multiple search, implementation, or debugging cycles. Record an emergency takeover in the ledger, including why delegation could not continue and how the result will be independently verified.

When the user changes direction, analyze impact, stop only affected branches, update the ledger and contracts, and reject stale results from integration.

Send user updates when creating the ledger and dispatching the first wave, completing a verification wave, changing the plan, scope, or a material decision, encountering a blocker that requires the user, or completing the task. For a long-running phase, also set an adaptive time or decision heartbeat in the ledger and send a synthesized update before the user is left without a meaningful view of the main line. Include the current objective or prototype hypothesis, latest decision-relevant evidence, convergence and side-effect budget, next decision or stop condition, and user action needed. Do not forward raw child progress. Expand evidence or worker detail only on request.

## Finish

Before delivery:

1. Confirm every acceptance criterion and required integrated check passed.
2. Confirm the latest accepted or rejected Gate is recorded in the ledger, or explicitly marked `pending integration`; do not declare the phase complete otherwise.
3. Confirm no necessary agent remains running and no stale result was integrated.
4. Reconcile conflicts and record unresolved risk accurately.
5. Deliver one coherent outcome grounded in artifacts and evidence.
