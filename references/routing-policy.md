# Routing Policy

Use this policy in orchestration mode for role, model, parallelism, ownership, recovery, and escalation decisions.

## Set Assurance And Side-Effect Controls

Set one assurance profile for every phase and record it in the ledger before dispatching work.

| Profile | Required evidence | Avoid |
| --- | --- | --- |
| `prototype` | One independent decision Gate per hypothesis experiment, centered on one focused end-to-end check and concise raw evidence for the path under test; a minimal REJECT record for a failed Gate | Separate Gates for subchanges serving the same experiment; re-gating status-only or explicitly non-normative documentation changes; broad regression, full-history revalidation, low-value sidecars, manifests, and large indexes before the decision unless policy or concrete risk requires them |
| `formal` | The prototype risk floor plus proportional independent verification of important artifacts, one phase-level regression by default, and final integrated verification | Reducing critical safety, authority, or external-effect checks for speed |
| `release` | Every `formal` requirement plus reproducible environment evidence, external dependency review, and release-readiness checks | Treating an unverified environment or external dependency as release-ready |

Default to `formal`. Use `prototype` only when the user explicitly requests a rapid prototype. Use `release` only when the user explicitly requests release-level assurance. Keep a profile fixed within a phase. You may raise it mid-phase; lower it only with explicit user authorization, and apply the lower profile only to work that has not started.

For each prototype experiment, record the `Hypothesis`, `Minimum experiment`, `Decision threshold`, `Nonblocking backlog`, and default Gate/repair budget before dispatch. Keep all subchanges needed to run that minimum experiment in one executor-owned candidate unless ownership or side-effect risk requires separation. Before adding work, an agent, an integration step, or an optional Gate, start with the irreducible outcome, constraints, and sufficient evidence; among sufficient choices prefer fewer assumptions, components, agents, state transitions, and checks. Uncertainty alone does not justify an addition. Add it only when it changes a concrete acceptance decision, protects a trust boundary or authority/side effect, enables valid evidence reuse, or resolves cross-owner coordination.

Default a tightly coupled prototype experiment to one executor-owned candidate. Design notes, wrapper behavior, schemas, mocks, and component checks that exist only to build that candidate are executor author work, not separately accepted workstreams or independent Gates. Use scouts only for independently blocking unknowns and parallel executors only for truly disjoint inputs whose concurrency saves critical-path time without adding integration or verification stages. Do not dispatch the independent verifier until the complete minimum experiment candidate and its author evidence are ready.

Never use the value test to remove checks needed to prevent a wrong experiment conclusion, an authority or side-effect violation, an irreversible unsafe action, an unreviewable result, or a responsibility-boundary failure. These form the prototype risk floor. Other engineering completeness risks remain visible in the nonblocking backlog.

Treat only the recorded decision threshold and prototype risk floor as blocking prototype acceptance criteria. Do not promote a backlog item into the current experiment's acceptance criteria without recording why it changes the experiment decision or obtaining authority for a material scope change.

For a prototype Gate rejection, record only the candidate identity, finding, reproduction evidence, verdict, and next action until the experiment decision. Default to one narrow repair and re-Gate for the same experiment. After another rejection, stop the automatic repair chain and reassess the hypothesis, abstraction, and experiment design. Continue repair only when new evidence, acceptance criticality, and a credible convergence path are recorded; otherwise backlog, reroute, redesign the experiment, or obtain authority for a harder assurance phase. Defer broad regression, manifests, sidecars, history revalidation, and large indexes unless repository policy or a concrete risk requires them to trust the experiment decision. Status-only and explicitly non-normative documentation changes may avoid a functional re-Gate. Changes to rules, prompts, schemas, policies, or other behavior-defining text are functional candidates and require coverage in the experiment Gate.

In `formal` and `release`, a specifically contracted verifier must run the final integrated verification. In `prototype`, the focused independent Gate may also serve as the final check only for one candidate with no substantive integration and only when the verifier contract names both purposes. Multiple candidates, interface composition, or integration changes require an independent integrated Gate.

Treat paid model calls, real-world or production runs, remote writes, irreversible actions, and secret persistence as budgeted side effects. Before dispatch, record the resource, limit, consumed count, renewal authority, and safe handling rule. Default their budget to zero when the user has not authorized a limit. Read-only, low-cost inspection does not need a numeric budget. A validation or command failure before an external action begins consumes no side-effect count. An attempted external action with unknown outcome consumes one count conservatively until reconciled. Exhausting a resource blocks only its side effect and retries: workers may complete otherwise-ready zero-consumption work, but must return control immediately before consuming the exhausted resource. They may not silently retry. Keep secrets in approved ephemeral environment channels unless the user explicitly authorizes another mechanism.

## Keep Implementation Minimum Sufficient

After understanding the real caller and execution flow, implement only what the current acceptance criteria and profile risk floor require. Prefer, in order: reuse the repository's implementation, the standard library, the native platform, an already installed dependency, then the minimum new code. Do not add abstractions, files, dependencies, configuration, compatibility layers, or tests without a present need. Put nonblocking simplifications in the backlog unless they materially affect scope, cost, or acceptance.

At a phase start, material assumption break, user direction change, or nonconvergence, reduce the decision to its irreducible outcome, constraints, and sufficient evidence; among sufficient choices prefer fewer assumptions, components, agents, state transitions, and checks. This is a decision-point rule, not a persistent mode or routine-substep ritual. Uncertainty by itself is not a present need: additions require a current acceptance criterion, observed failure, profile risk floor or trust boundary, or conclusion/replayability need.

For a bug fix, trace the relevant callers and flow before editing. Prefer one shared root-cause repair over symptom patches, unless the flow shows the symptoms have distinct causes.

Minimum sufficient work never removes explicit requirements, trust-boundary validation, security, data-loss prevention, accessibility basics, physical calibration where relevant, conclusion validity, replayability, authority, or side-effect safety. Keep those obligations in the profile risk floor.

Every added check must name a concrete failure mode and contribute unique decision evidence. Do not duplicate checks or Gates for ceremony; include this review in the existing applicable Gate. Preserve the profile boundary: `prototype` uses the smallest decision-relevant runnable check plus its risk floor, `formal` uses proportional regression, and `release` retains release-readiness checks.

## Route Roles And Models

| Work shape | Role | Default model and effort |
| --- | --- | --- |
| Deterministic lookup, extraction, formatting, or routine checks | Scout or verifier | `gpt-5.6-luna` at baseline `medium` |
| Routine bounded implementation, tests, docs, ordinary diagnosis, or integration | Executor or integrator | `gpt-5.6-terra` at baseline `medium` |
| Architecture, consequential conflict resolution, adversarial verification, or a task with a recorded Terra capability gap | Any matching specialist role | `gpt-5.6-sol` at baseline `medium` |
| Intent, decomposition, consequential decisions, conflict resolution, and final acceptance | Controller | Current controller; prefer high-reasoning capability |

Select a **route** as the pair `model/effort`, bottom-up for every new task shape: choose the lowest sufficient model, then the lowest sufficient effort for that model. Start at `luna/medium` for deterministic checks and `terra/medium` for routine bounded work. Routine implementation or repair after architecture, plus configuration, field mapping, directory preparation, and fixed tests, default to `terra/medium`; deterministic checks default to `luna/medium`. Generic labels such as complex, important, or high-risk do not justify Sol: record the concrete capability Terra lacks for this task. Likewise, every effort increase needs independent evidence that `medium` is insufficient; `high` is not implied by the selected model.

Use `luna/high` only for bounded ambiguity, multi-source reconciliation, or a routine check needing substantial judgment; use `terra/high` only for cross-boundary implementation, ambiguous diagnosis, or meaningful integration risk. Use `sol/high` only for high-stakes or ambiguous trust, architecture, conflict, or adversarial verification after recording both the Terra gap and why Sol `medium` is insufficient. Escalate to `sol/xhigh`, then `max`, only with new evidence that the immediately lower effort is insufficient. After failure, first correct a deficient contract or approach; escalate only for a remaining evidenced capability gap.

Model and effort are sticky after creation, but task shape is not. Reevaluate the recommended route before every `followup_task`; never inherit a route automatically. Record the current task shape, recommended route, and reuse or mandatory-downgrade decision. Reuse only for genuinely related work whose required route exactly matches the current one. A route mismatch requires a fresh lower or otherwise suitable agent and compact task-local handoff, while adjacent low-tier tasks should be batched under one suitably routed agent rather than fragmented into one agent per minor operation. Same-executor continuity never overrides a mandatory downgrade: an architecture-to-routine transition normally requires a Terra executor, not a retained Sol agent. A role transition into independent verification always requires a distinct verifier regardless of route or cache considerations.

Make the total-cost decision cache-aware without treating cache hits as guaranteed. Preserve a genuinely same-task, same-route agent when its useful context outweighs transfer cost; otherwise send a compact handoff containing the objective, candidate/digest, relevant paths, settled facts, acceptance criteria, and scope boundary. Keep stable reference material and task prompts ordered consistently because prompt reuse depends on a matching rendered prefix. Do not assume cache sharing across agents or models. Savings from a downgrade or reuse cannot override independent-verifier separation.

In `prototype`, when Sol is selected, record a bounded high-tier work budget and reassess at its boundary. This is a justification checkpoint, not an automatic agent replacement: continuing the same agent is allowed only while the same task shape and concrete Terra gap remain; renewal otherwise requires a fresh bottom-up decision. This prevents Sol drift without forcing wasteful handoffs mid-task.

Every child dispatch must begin with the exact non-delegation preamble in [task-contract.md](task-contract.md), even when the runtime or parent context already identifies the child as a worker. It must explicitly name the route and its rationale. Selecting the controller's route is permitted only when intentional; silent inheritance is prohibited. For fresh creation, use the name `<role>_<work>_<model>_<effort>` with valid lowercase underscore tokens, such as `verifier_release_sol_high`; it is a requested-route hint, not actual-route evidence. If the recommended route is unavailable, select and record an explicit fallback on a fresh agent. When overrides are available, set `fork_turns` to `"none"` and pass task-local context. Do not claim an actual route before creation confirms it. After dispatch, record requested and actual routes, fallback, reuse/new-agent rationale, and any material confidence impact in the ledger and evidence return.

- **Scout:** Produce bounded discovery or evidence. A scout may later execute only under a new executor contract.
- **Executor:** Produce or repair artifacts and run relevant local checks. An executor may repair its own rejected work.
- **Integrator:** Own substantive merging, cross-component adjustments, and integration checks. Treat integration as execution, not controller glue.
- **Verifier:** Independently judge artifacts and report findings. Never modify or fix the artifact being verified.

An executor or integrator must never verify its own artifact. Use a distinct verifier for important artifacts. Avoid a separate verifier only for low-risk work that is completely established by necessary mechanical checks; apply the profile-specific final verification rule above.

## Assign Verification And Evidence Ownership

Assign each required check to one primary owner before dispatch:

- **Executor:** targeted author checks for the changed artifact and its direct behavior.
- **Verifier:** orthogonal acceptance and risk checks, including independent reproduction only when the contract requires it.
- **Integrator:** cross-component, merge, and interface checks introduced by integration.
- **Specifically contracted final verifier:** the final check required by the assurance profile; in a qualifying single-candidate prototype, the same verifier contract may own the focused decision Gate and final check.
- **Controller:** judge evidence, resolve conflicts, and accept or reject; do not rerun an owned check merely to observe it.

Do not duplicate an identical check across roles merely for ceremony. Each check must name a concrete failure mode and add unique decision evidence. A verifier must use an orthogonal method, an independent environment, or a risk-based check when repetition is required. On the verifier's first pass, identify the authoritative source of rules and evidence. Challenge self-authenticating or co-mutable candidate/proof loops when relevant; do not accept a candidate solely because it supplies mutable proof of its own compliance.

Record an evidence receipt keyed by stable candidate identity or digest, command, environment, result, and owner before evidence reuse or independent verification, and whenever cross-owner, concurrent, or mutable-state risk applies. Reuse it only while all five remain applicable and the source is trusted; evidence from a superseded identity never transfers. Rerun after a candidate or environment change, missing or untrusted evidence, or when independent reproduction is an explicit acceptance need. Ordinary local/static low-risk steps need targeted author evidence, not a candidate digest or expanded receipt. Contracts must state checks owned, checks already satisfied, and checks that must not be rerun only when reuse applies. For mutable remote state only, also record observed state/time, identity or revision when available, freshness bound, and reread trigger; reread before a dependent mutation or acceptance when the bound has expired or material change is indicated.

## Dispatch In Dependency Waves

Before `spawn_agent` or `followup_task`, require the compact routing preflight in [task-contract.md](task-contract.md). The ledger must be current for role and task shape; current and recommended route; concrete lower-route insufficiency when above baseline; reuse, mandatory-downgrade, and cache decision; ownership and completion gate; and actual side-effect authority and budget. Add candidate identity, latest Gate, blocker, high-tier renewal, or fuller state only when a named acceptance, trust-boundary, authority, evidence-reuse, or cross-owner coordination need applies; otherwise stop dispatch only for a missing routing-core field. Reassess role, task shape, and route at implementation-to-verification, verifier-REJECT-to-repair, architecture-to-routine execution, implementation-to-installation or mechanical checking, after two repairs by one agent, context restore or compaction, and material candidate identity or digest change. Candidate change alone does not force a route change.

1. Identify ready workstreams from the ledger.
2. Dispatch all ready work that is independent, disjoint, and useful within capacity. Commonly run two executors and preserve room for a verifier.
3. Do not serialize independent work by default, force parallelism onto dependent work, or fill slots with speculative research.
4. End the wave at its completion gates, update the ledger, resolve rejected or conflicting results, then dispatch the next ready wave.

Default to one layer of direct child agents. When model or effort overrides are available, set `fork_turns` to `"none"` and provide only task-local context. A perceived need for most of the conversation signals poor decomposition; refine the contract instead. Child agents may not spawn other agents.

A delegated child is in worker mode with zero remaining delegation depth. It may use ordinary specialist skills required by its contract, but must not invoke `orchestrate-work`, spawn agents, create tasks, or take over controller responsibilities. Return a concrete escalation when the contract needs further decomposition. These restrictions must appear in the dispatch message itself; a ledger reference is not sufficient.

## Own State Explicitly

Assign exclusive ownership of every file, directory, artifact, and external mutation. If ownership overlaps, serialize the work or appoint one owner. When concurrent writers share a worktree, maintain a ledger ownership map with each path or resource, its writer, allowed mutation, freeze state, and dependent verifier or integrator.

Use a shared worktree only when changes are truly disjoint and relevant checks cannot observe a partially modified repository. Otherwise use isolated worktrees and assign an integrator to combine accepted results. Apply the same rule to external systems: concurrent agents must not mutate overlapping state.

Agents edit or produce their owned artifacts directly and run checks appropriate to them. Ordinary local low-risk work returns artifact locations and targeted author evidence; it does not need a digest or expanded receipt. Before evidence reuse or independent verification, and whenever cross-owner, concurrent, or mutable-state risk applies, a writer reports an artifact handoff receipt containing each absolute path, type, size, readability, and stable identity or digest; the controller confirms it in the declared environment. A verifier is eligible only after a complete candidate has applicable verified handoff/identity, author evidence covering the current decision threshold and risk floor, and no unresolved ordinary author-debugging issue; a partial candidate is never Gate-ready. A missing or mismatched applicable receipt is an incomplete handoff, not a quality Gate. Freeze a functional candidate's paths when it enters independent verification; verifiers remain read-only, and integrators may modify only their assigned integration paths. Keep raw logs and bulky exploration in the worker context; return compact evidence and artifact locations unless failure or conflict requires controller inspection. Evidence from a superseded candidate identity must not transfer.

Treat functional candidates, verification verdicts, and ledger integration as distinct logical objects. The executor owns the candidate; the verifier produces an immutable verdict without changing it; the controller records status without changing the verdict. Make separate commits only when repository policy or isolation needs them. For repair, state the functional base and excluded historical verification artifacts in the contract.

## Monitor By Progress

Give each task a measurable completion gate and a soft timeout appropriate to expected work and tool latency. Do not use fixed universal minute limits.

At a soft timeout, request one compact progress report. Continue a worker while it shows healthy, in-scope progress, produces new evidence, and approaches its acceptance gate. Interrupt or reroute only when it is stalled, repeats a failed approach, leaves scope, consumes the main line with noncritical hardening, or blocks the critical path without useful progress.

## Recover And Stop

- First verification failure: return findings to the original executor only when preflight confirms its current route matches the recommended route; otherwise create a fresh executor and explicitly transfer repair ownership and the functional base. Record the candidate, finding, reproduction evidence, verdict, and next action.
- In `prototype`, default to one narrow repair and re-Gate. A further rejection requires route reassessment; continue only with recorded new evidence, acceptance criticality, and a credible convergence path. Otherwise backlog, reroute, redesign the experiment, or seek authority for a harder assurance phase.
- In `formal` and `release`, continue acceptance-critical repair while each iteration has new evidence and a credible path toward acceptance; do not impose a universal one-repair limit.
- Repeated same failure, no new evidence, noncritical hardening consuming the main line, or two non-converging rounds: reassess the contract, assumptions, and approach, then backlog, change route, or escalate model or effort only for an evidenced capability gap.
- Continued failure after reassessment: stop that route. Try another in-scope route when available; otherwise report a concrete blocker or request the needed user decision.
- Worker loss or tool failure: preserve accepted artifacts and ledger state, then recover routine command, tool-path, JSON/reference, restart/socket/readiness, and healthy-observation-timeout faults within the side-effect, retry, and convergence limits before redispatching or using another in-scope route. These faults do not make a result partial by themselves.
- Conflicting evidence: keep disputed output unaccepted until the controller resolves it or commissions targeted evidence.
- Scope discovery: admit only acceptance-critical, blocking, or assumption-disproving work. Backlog optional work and ask the user about material scope change.

Do not let the controller casually repair failed delegated work. Before a second substantive controller search, test, or fix cycle, delegate or record an emergency takeover; orientation, handoff checks, and direct units do not count. An emergency takeover is allowed only when it is recorded in the ledger and independently verified.

## Freeze And Advance Phases

After a phase is accepted, update the ledger with its completed deliverables, explicit non-goals, latest Gate verdict, next objective, assurance profile, and remaining or newly granted authority. Start the next phase automatically only when it was already in the authorized plan. Otherwise ask the user for the new objective or authority. An accepted Gate that has not yet reached the ledger is `pending integration`, not phase completion.

## Escalate To User-Owned Tasks

Use direct child agents by default. Suggest a new user-owned Codex task only for work that is long-lived and independently steerable or needs strong worktree isolation. Creating or handing off such a task requires explicit user authorization; do not substitute it for ordinary child-agent orchestration.

When the user changes the objective, perform impact analysis, interrupt only affected branches, revise the ledger and contracts, and do not integrate results produced against stale requirements.
