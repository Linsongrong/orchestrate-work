---
name: orchestrate-work
description: Coordinate work through continuous main-line ownership, bounded specialist agents, adaptive model routing, durable state, and independent verification. Use when the user requests orchestration, independent workstreams benefit from parallelism or context isolation, important results need independent verification, or long-lived work needs coordination. Task length or file count alone does not require delegation.
---

# Orchestrate Work

If operating under another controller's contract, enter **worker mode**: do not invoke this skill, spawn agents, or create/fork Codex tasks or threads, and do not replan the objective. Domain or business Task objects explicitly required by the objective and assigned side-effect authority remain allowed. Complete the assigned specialist role or return a concrete escalation.

Respect the user's execution preferences. Explicit invocation requests these safeguards, not a minimum agent count. Choose execution ownership separately from assurance:

- **Direct:** the primary agent completes work with proportional author checks when no concrete delegation benefit or independent-verification requirement applies. Repeated investigation, implementation, or debugging does not itself require a handoff.
- **Orchestration:** keep one accountable owner for each coupled main line, which may be the primary agent or a delegated executor. Prefer primary ownership when investigation and implementation depend on continuous context; delegate bounded independent branches, substantial routine batches, or work needing context isolation. Keep a non-executing controller when consequential coordination, conflicting evidence, or oversight would compete with main-line execution. Never write into another active owner's scope.

Important results and explicit assurance profiles retain their independent-verification requirements regardless of who implements. If the user disallows delegation, continue authorized work and proportional self-checks, disclose the missing independent assurance, and do not claim an independent Gate passed or silently lower the criteria.

For orchestration or substantive model routing, read [routing-policy.md](references/routing-policy.md); read [task-contract.md](references/task-contract.md) before dispatch. Reread only after context loss or a policy change. Read [checkpoint-protocol.md](references/checkpoint-protocol.md) only for multiple waves, compaction/resume, or long-lived remote coordination, including a primary-owned main line.

## Prototype Fast Path

For `prototype`, state one hypothesis, run one minimum experiment against one decision threshold, keep the main candidate and targeted author checks with one owner, then dispatch one focused independent decision verifier. The owner may be the primary agent; its self-checks never replace that verifier. Do not split the experiment into design, mock, component, or preflight Gates, or add an integrator unless integration is substantive.

Start from the irreducible outcome, constraints, and sufficient evidence. Prefer fewer assumptions, components, agents, state transitions, and checks. Uncertainty alone adds no work; expand only for an acceptance, trust, authority, evidence, or cross-owner need.

## Controller Loop

1. Bound the work, select its assurance profile and main-line owner, and record ownership, route, authority, and completion boundary. Use the unified record for each dispatch.
2. Split only independently acceptable work. Keep a tightly coupled main line with its owner; add a scout or parallel executor only for a blocking unknown or truly disjoint input.
3. Dispatch ready disjoint work within capacity. Give each agent exclusive ownership, a completion gate, minimum necessary context, and named check ownership.
4. While work runs, advance owned work, maintain control state, and resolve blockers without duplicating delegated work. Reassign a bounded branch if execution prevents timely oversight.
5. Accept only complete candidates with applicable identity and evidence. The author cannot issue the independent verdict. A controller that authored the candidate may record acceptance only from a matching independent accept verdict; it cannot override a rejection or relax criteria to accept its work.
6. Assign integration ownership only when integration is substantive; the primary agent may integrate within its scope. In `formal` and `release`, require focused final independent verification. One explicitly assigned Gate can cover a single complete candidate and final verification when no later integration changes it. Each check needs a unique concrete failure mode; do not default to adversarial, edge, or broad-regression ceremony.
7. On rejection, use the recovery policy. Freeze an accepted phase before starting another; record deliverables, non-goals, next objective, profile, and authority.

The primary agent retains responsibility for intent, scope, control state, communication, and delivery even when it implements. As an author it also follows the executor's minimum-work, evidence, and budget rules. Mechanical checks and harmless status updates do not require separate agents or Gates. Ownership changes require an explicit transfer, not concurrent repair by controller and worker.

## Stop And Recover

Continue until acceptance criteria pass or the recovery policy requires stopping for a genuine blocker, authority/budget boundary, no-new-evidence/nonconvergence, or required user decision. Before delivery, confirm necessary agents have settled, stale results were not integrated, conflicting evidence is resolved or remains unaccepted, and unresolved risk is recorded accurately. Ordinary recoverable faults remain worker-owned and do not justify partial. On actual compaction, reconcile current user direction with authoritative Git, tests, and artifacts before acting; the checkpoint is recovery aid, never evidence. Send synthesized, adaptive progress updates for long-running work and on material decisions, verification, blockers requiring the user, and completion.

Use [routing-policy.md](references/routing-policy.md) for the authoritative rules on profiles, routes, budgets, ownership, evidence, recovery, and advancement. Use [task-contract.md](references/task-contract.md) for the mandatory worker preamble, dispatch record, role boundaries, and return. Use [checkpoint-protocol.md](references/checkpoint-protocol.md) for conditional checkpointing.
