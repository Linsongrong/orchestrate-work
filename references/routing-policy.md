# Routing Policy

Use this policy in orchestration mode for role, model, parallelism, ownership, recovery, and escalation decisions.

## Set Assurance And Side-Effect Controls

Set one assurance profile for every phase and record it in the ledger before dispatching work.

| Profile | Required evidence | Avoid |
| --- | --- | --- |
| `prototype` | One independent Gate for each major implementation, one focused end-to-end check, concise raw evidence for the path under test, and a minimal REJECT record for a failed Gate | Re-gating status-only or explicitly non-normative documentation changes, full-history revalidation, low-value sidecars, manifests, and large indexes before final ACCEPT unless policy or risk requires them |
| `formal` | Every `prototype` safeguard plus proportional independent verification of important artifacts and final integrated verification | Reducing critical safety, authority, or external-effect checks for speed |
| `release` | Every `formal` requirement plus reproducible environment evidence, external dependency review, and release-readiness checks | Treating an unverified environment or external dependency as release-ready |

Default to `formal`. Use `prototype` only when the user explicitly requests a rapid prototype. Use `release` only when the user explicitly requests release-level assurance. Keep a profile fixed within a phase. You may raise it mid-phase; lower it only with explicit user authorization, and apply the lower profile only to work that has not started.

For a prototype Gate rejection, record only the candidate identity, finding, reproduction evidence, verdict, and next action until a final ACCEPT. Defer manifests, sidecars, history revalidation, and large indexes unless repository policy or a concrete risk requires them. Status-only and explicitly non-normative documentation changes may avoid a functional re-Gate. Changes to rules, prompts, schemas, policies, or other behavior-defining text are functional candidates and require appropriate Gate coverage. Run one phase-level full regression by default; add more only when the acceptance criteria, policy, or risk requires them.

Treat paid model calls, real-world or production runs, remote writes, irreversible actions, and secret persistence as budgeted side effects. Before dispatch, record the resource, limit, consumed count, renewal authority, and safe handling rule. Default their budget to zero when the user has not authorized a limit. Read-only, low-cost inspection does not need a numeric budget. Workers must declare consumption before performing a budgeted action and return control when the limit is exhausted; they may not silently retry. Keep secrets in approved ephemeral environment channels unless the user explicitly authorizes another mechanism.

## Route Roles And Models

| Work shape | Role | Default model and effort |
| --- | --- | --- |
| Deterministic lookup, extraction, formatting, or routine checks | Scout or verifier | `gpt-5.6-luna` at baseline `medium` |
| Routine bounded implementation, tests, docs, ordinary diagnosis, or integration | Executor or integrator | `gpt-5.6-terra` at baseline `medium` |
| Complex or high-risk implementation, research, diagnosis, conflict analysis, or important independent verification | Any matching specialist role | `gpt-5.6-sol` at baseline `medium` |
| Intent, decomposition, consequential decisions, conflict resolution, and final acceptance | Controller | Current controller; prefer high-reasoning capability |

Every child dispatch must explicitly name a model, reasoning effort, and routing rationale. Selecting the controller's model or effort is permitted only when stated as an intentional route; accidental inheritance is prohibited. Keep model choice tied to work shape, not effort alone. Start all three model routes at `medium`. Raise `gpt-5.6-luna` to `high` only for bounded ambiguity, multi-source reconciliation, or routine checks that need substantial judgment. Raise `gpt-5.6-terra` to `high` only for cross-boundary implementation, ambiguous diagnosis, or meaningful integration risk. Raise `gpt-5.6-sol` to `high` only for high-stakes or ambiguous trust, architecture, conflict, or adversarial verification; raise it to `xhigh` only after `high` is demonstrably insufficient. Use `max` only as an exceptional escalation from the Sol route after `xhigh` is demonstrably insufficient and the quality or risk justifies it; record the evidence and rationale. When overrides are available, set `fork_turns` to `"none"` and pass task-local context. If an override cannot be applied, record the requested route, actual runtime model and effort, fallback reason, and material confidence impact in the ledger and evidence return. Prefer the stronger route only when the added judgment is justified. After a failure, first diagnose whether the contract or approach is wrong; escalate model or effort only when the capability gap remains evidenced.

- **Scout:** Produce bounded discovery or evidence. A scout may later execute only under a new executor contract.
- **Executor:** Produce or repair artifacts and run relevant local checks. An executor may repair its own rejected work.
- **Integrator:** Own substantive merging, cross-component adjustments, and integration checks. Treat integration as execution, not controller glue.
- **Verifier:** Independently judge artifacts and report findings. Never modify or fix the artifact being verified.

An executor or integrator must never verify its own artifact. Use a distinct verifier for important artifacts. Avoid a separate verifier only for low-risk work that is completely established by necessary mechanical checks; final integrated verification remains mandatory.

## Assign Verification And Evidence Ownership

Assign each required check to one primary owner before dispatch:

- **Executor:** targeted author checks for the changed artifact and its direct behavior.
- **Verifier:** orthogonal acceptance and risk checks, including independent reproduction only when the contract requires it.
- **Integrator:** cross-component, merge, and interface checks introduced by integration.
- **Specifically contracted final verifier:** one final integrated check appropriate to the delivered candidate.
- **Controller:** judge evidence, resolve conflicts, and accept or reject; do not rerun an owned check merely to observe it.

Do not duplicate an identical check across roles merely for ceremony. A verifier must use an orthogonal method, an independent environment, or a risk-based check when repetition is required. On the verifier's first pass, identify the authoritative source of rules and evidence. Challenge self-authenticating or co-mutable candidate/proof loops when relevant; do not accept a candidate solely because it supplies mutable proof of its own compliance.

Record evidence as a receipt keyed by candidate identity or digest, command, environment, result, and owner. Reuse it only while all five remain applicable and the source is trusted. Rerun only after a candidate or environment change, missing or untrusted evidence, or when independent reproduction is an explicit acceptance need. Contracts must state checks owned, checks already satisfied, and checks that must not be rerun.

## Dispatch In Dependency Waves

1. Identify ready workstreams from the ledger.
2. Dispatch all ready work that is independent, disjoint, and useful within capacity. Commonly run two executors and preserve room for a verifier.
3. Do not serialize independent work by default, force parallelism onto dependent work, or fill slots with speculative research.
4. End the wave at its completion gates, update the ledger, resolve rejected or conflicting results, then dispatch the next ready wave.

Default to one layer of direct child agents. When model or effort overrides are available, set `fork_turns` to `"none"` and provide only task-local context. A perceived need for most of the conversation signals poor decomposition; refine the contract instead. Child agents may not spawn other agents.

A delegated child is in worker mode. It may use ordinary specialist skills required by its contract, but must not invoke `orchestrate-work`, create agents or tasks, or take over controller responsibilities. Return a concrete escalation when the contract needs further decomposition.

## Own State Explicitly

Assign exclusive ownership of every file, directory, artifact, and external mutation. If ownership overlaps, serialize the work or appoint one owner. When concurrent writers share a worktree, maintain a ledger ownership map with each path or resource, its writer, allowed mutation, freeze state, and dependent verifier or integrator.

Use a shared worktree only when changes are truly disjoint and relevant checks cannot observe a partially modified repository. Otherwise use isolated worktrees and assign an integrator to combine accepted results. Apply the same rule to external systems: concurrent agents must not mutate overlapping state.

Agents edit or produce their owned artifacts directly and run checks appropriate to them. Freeze a functional candidate's paths when it enters independent verification; verifiers remain read-only, and integrators may modify only their assigned integration paths. Keep raw logs and bulky exploration in the worker context; return compact evidence and artifact locations unless failure or conflict requires controller inspection. Record a stable candidate identity or digest before reusing or independently verifying its evidence.

Treat functional candidates, verification verdicts, and ledger integration as distinct logical objects. The executor owns the candidate; the verifier produces an immutable verdict without changing it; the controller records status without changing the verdict. Make separate commits only when repository policy or isolation needs them. For repair, state the functional base and excluded historical verification artifacts in the contract.

## Monitor By Progress

Give each task a measurable completion gate and a soft timeout appropriate to expected work and tool latency. Do not use fixed universal minute limits.

At a soft timeout, request one compact progress report. Continue a worker while it shows healthy, in-scope progress, produces new evidence, and approaches its acceptance gate. Interrupt or reroute only when it is stalled, repeats a failed approach, leaves scope, consumes the main line with noncritical hardening, or blocks the critical path without useful progress.

## Recover And Stop

- First verification failure: return findings to the original executor for repair and record the candidate, finding, reproduction evidence, verdict, and next action.
- Continue acceptance-critical repair while each iteration has new evidence and a credible path toward acceptance; do not impose a universal one-repair limit.
- Repeated same failure, no new evidence, noncritical hardening consuming the main line, or two non-converging rounds: reassess the contract, assumptions, and approach, then backlog, change route, or escalate model or effort only for an evidenced capability gap.
- Continued failure after reassessment: stop that route. Try another in-scope route when available; otherwise report a concrete blocker or request the needed user decision.
- Worker loss or tool failure: preserve accepted artifacts and ledger state, then redispatch or use another in-scope route.
- Conflicting evidence: keep disputed output unaccepted until the controller resolves it or commissions targeted evidence.
- Scope discovery: admit only acceptance-critical, blocking, or assumption-disproving work. Backlog optional work and ask the user about material scope change.

Do not let the controller casually repair failed delegated work. An emergency takeover is allowed only when it is recorded in the ledger and independently verified.

## Freeze And Advance Phases

After a phase is accepted, update the ledger with its completed deliverables, explicit non-goals, latest Gate verdict, next objective, assurance profile, and remaining or newly granted authority. Start the next phase automatically only when it was already in the authorized plan. Otherwise ask the user for the new objective or authority. An accepted Gate that has not yet reached the ledger is `pending integration`, not phase completion.

## Escalate To User-Owned Tasks

Use direct child agents by default. Suggest a new user-owned Codex task only for work that is long-lived and independently steerable or needs strong worktree isolation. Creating or handing off such a task requires explicit user authorization; do not substitute it for ordinary child-agent orchestration.

When the user changes the objective, perform impact analysis, interrupt only affected branches, revise the ledger and contracts, and do not integrate results produced against stale requirements.
