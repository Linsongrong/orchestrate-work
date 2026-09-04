---
name: orchestrate-work
description: Coordinate non-trivial or long-running work through a controller-first orchestration loop with bounded specialist agents, durable task state, adaptive model routing, and independent verification. Use when the user explicitly asks for delegation, subagents, parallel work, or orchestration; when independent workstreams can run concurrently; when substantial discovery, implementation, testing, or debugging must be isolated; or when an important result needs independent verification.
---

# Orchestrate Work

If operating under another controller's contract, enter **worker mode**: do not invoke this skill, spawn agents, or create/fork Codex tasks or threads, and do not replan the objective. Domain or business Task objects explicitly required by the objective and assigned side-effect authority remain allowed. Complete the assigned specialist role or return a concrete escalation.

Choose one mode:

- **Direct:** only a genuinely trivial, local task with no meaningful decomposition or repeated search, implementation, or debugging. Explicit invocation strongly presumes orchestration.
- **Orchestration:** the controller manages and judges; it delegates substantive discovery, implementation, integration, and verification and does not own a main execution branch. It may complete a direct unit only when mechanical, local, low-risk, and requiring no independent judgment (for example, existence, digest, non-normative status, or harmless local state updates); behavior, candidate identity, side-effect authority/budget or externally consequential effects, prompt, schema, and policy changes are never direct units.

In orchestration mode, read [routing-policy.md](references/routing-policy.md) and [task-contract.md](references/task-contract.md) before the first plan or dispatch; reread them only after context loss or a policy change. Read [checkpoint-protocol.md](references/checkpoint-protocol.md) only for multiple waves, compaction/resume, or long-lived remote coordination.

## Prototype Fast Path

For `prototype`, state one hypothesis, run one minimum experiment against one decision threshold, give one executor ownership of the main candidate and targeted author checks, then dispatch one focused independent decision verifier. Do not split that experiment into design, mock, component, or preflight Gates, or add an integrator unless integration is substantive.

Start from the irreducible outcome, constraints, and sufficient evidence. Prefer fewer assumptions, components, agents, state transitions, and checks. Uncertainty alone adds no work; expand only for an acceptance, trust, authority, evidence, or cross-owner need.

## Controller Loop

1. Do enough orientation to bound the work, select the assurance profile, and record each dispatch through the unified record.
2. Split only independently acceptable work. Keep one tightly coupled prototype main line executor-owned; add a scout or parallel executor only for a blocking unknown or truly disjoint input.
3. Dispatch ready disjoint work within capacity. Give each agent exclusive ownership, a completion gate, minimum necessary context, and named check ownership.
4. While work runs, maintain control state, resolve blockers, prepare later dispatches, and avoid duplicating delegated substantive work.
5. Accept only complete candidates with the applicable identity and author evidence. A verifier issues an immutable verdict; the controller judges it but cannot implement and accept the same substantive artifact.
6. Integrate through an integrator only when integration is substantive. In `formal` and `release`, assign a focused final independent verifier. Every check must name its unique concrete failure mode; do not default to adversarial, edge, or broad-regression ceremony.
7. On rejection, use the recovery policy. Freeze an accepted phase before starting another; record deliverables, non-goals, next objective, profile, and authority.

Controller work is orientation, decomposition, dispatch, ledger/control state, conflict resolution, acceptance, communication, and tiny glue. Before a second substantive controller search, test, or fix cycle, delegate or record an exceptional emergency takeover and require independent verification.

## Stop And Recover

Continue until acceptance criteria pass or the recovery policy requires stopping for a genuine blocker, authority/budget boundary, no-new-evidence/nonconvergence, or required user decision. Before delivery, confirm necessary agents have settled, stale results were not integrated, conflicting evidence is resolved or remains unaccepted, and unresolved risk is recorded accurately. Ordinary recoverable faults remain worker-owned and do not justify partial. On actual compaction, reconcile current user direction with authoritative Git, tests, and artifacts before acting; the checkpoint is recovery aid, never evidence. Send synthesized, adaptive progress updates for long-running work and on material decisions, verification, blockers requiring the user, and completion.

Use [routing-policy.md](references/routing-policy.md) for the authoritative rules on profiles, routes, budgets, ownership, evidence, recovery, and advancement. Use [task-contract.md](references/task-contract.md) for the mandatory worker preamble, dispatch record, role boundaries, and return. Use [checkpoint-protocol.md](references/checkpoint-protocol.md) for conditional checkpointing.
