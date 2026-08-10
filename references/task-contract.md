# Task Contract

Give every child agent one bounded contract. Model and reasoning fields are mandatory; other fields may be omitted only when genuinely irrelevant.

```text
Execution mode: worker
Delegation depth: 1
Role: scout | executor | integrator | verifier
Phase:
Assurance profile: prototype | formal | release
Selected model: required; name the intended runtime model, including an intentional same-as-controller choice
Reasoning effort: required; low | medium | high | xhigh | max
Routing rationale: required; why this model and effort fit the work and risk
Runtime fallback: required; write `none` when unused, otherwise requested route, actual route, fallback reason, and confidence impact
Objective:
Relevant context and settled decisions:
Dependencies and inputs:
Owned files, directories, artifacts, or external state:
Execution environment or worktree:
Shared-worktree ownership map or reference: required for concurrent writers
Functional base and excluded historical artifacts: required for repairs or re-verification
Side-effect budget and handling rule: resource, limit, consumed, renewal authority
Out of scope:
Deliverable:
Acceptance criteria:
Required evidence and checks:
Checks owned by this role:
Checks already satisfied and must not be rerun:
Final integrated verification: for a final verifier, identify the integrated candidate and assigned check; otherwise `not assigned`
Completion gate:
Soft timeout and progress signal:
Escalate when:
Agent spawning: prohibited
Orchestration skill: prohibited
```

Require this compact return exactly:

```text
Status: complete | partial | blocked
Result or artifact locations:
Evidence and checks:
Candidate identity or digest and evidence receipts: candidate, command, environment, result, owner
Requested and actual model/effort, including fallback if used:
Material decisions or findings:
Unresolved risks:
Controller action needed:
```

Keep raw transcripts, logs, and large evidence collections outside the return unless the controller needs them to resolve a failure or conflict.

## Common Rules

- Treat the contract as authoritative and solve only its objective.
- Use the explicitly contracted model and reasoning effort. Do not silently inherit a route; report any runtime fallback with its reason and confidence impact.
- Use only task-local context; request a specific missing fact instead of the parent conversation.
- Do not invoke `orchestrate-work`, spawn agents, create tasks, expand scope, or mutate unowned state. You may use ordinary specialist skills needed to complete the contract.
- Preserve existing user changes and avoid unrelated edits.
- Produce artifacts directly when assigned ownership.
- Before a budgeted side effect, report the intended consumption and verify remaining budget. Stop and escalate when it is exhausted; do not retry without renewed authorization.
- Stop at the completion gate, not at an arbitrary elapsed-time cutoff.
- At a soft timeout, return compact progress when asked and continue if progress is healthy.
- Report failed checks, uncertainty, scope pressure, and blockers without hiding them behind `complete`.

## Role Rules

### Scout

- Gather only evidence needed for the stated decision or execution branch.
- Return findings, sources, confidence, and unresolved questions; do not implement unless given a new executor contract.

### Executor

- Implement within exclusive ownership and run the targeted author checks assigned in the contract.
- Repair own work when the controller returns independent verification findings.
- Treat a submitted candidate as frozen while independent verification runs. For a repair, use the stated functional base and do not absorb historical verifier artifacts into the candidate.
- Do not issue the acceptance verdict for own artifacts.

### Integrator

- Combine accepted inputs, own overlap and integration adjustments, and run assigned cross-component checks.
- Treat generated integration artifacts as own work requiring a separate verifier.
- Modify only assigned integration paths; do not alter a frozen candidate or a verifier verdict.

### Verifier

- Inspect the artifact independently against the supplied criteria and raw evidence. First identify the authoritative source of rules and evidence; challenge self-authenticating or co-mutable candidate/proof loops when relevant.
- Report findings ordered by severity, passed checks, residual risk, and an explicit accept or reject verdict.
- Do not edit, repair, or reimplement the artifact. Send failures to the controller.
- Treat the verdict as an immutable verification artifact. Status-only and explicitly non-normative documentation artifacts may be out of scope when the contract says so; rules, prompts, schemas, policies, and other behavior-defining text require appropriate verification.

## Verification Contract

For a verifier, make the general contract concrete with:

```text
Objective: Determine whether the supplied artifacts satisfy every acceptance criterion.
Artifacts and evidence:
Acceptance criteria:
Checks to run:
Checks already satisfied and must not be rerun:
Independent reproduction required: yes | no; reason
Final integrated verification: yes | no; identify the integrated candidate when yes
Out of scope: Modifying, repairing, or reimplementing the artifacts.
Completion gate: Clear accept/reject verdict with findings, passed checks, and residual risk.
```
