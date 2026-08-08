# Task Contract

Give every child agent one bounded contract. Omit fields only when genuinely irrelevant.

```text
Execution mode: worker
Delegation depth: 1
Role: scout | executor | integrator | verifier
Phase:
Assurance profile: prototype | formal | release
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
Material decisions or findings:
Unresolved risks:
Controller action needed:
```

Keep raw transcripts, logs, and large evidence collections outside the return unless the controller needs them to resolve a failure or conflict.

## Common Rules

- Treat the contract as authoritative and solve only its objective.
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

- Implement within exclusive ownership and run necessary local checks.
- Repair own work when the controller returns independent verification findings.
- Treat a submitted candidate as frozen while independent verification runs. For a repair, use the stated functional base and do not absorb historical verifier artifacts into the candidate.
- Do not issue the acceptance verdict for own artifacts.

### Integrator

- Combine accepted inputs, own overlap and integration adjustments, and run integration checks.
- Treat generated integration artifacts as own work requiring a separate verifier.
- Modify only assigned integration paths; do not alter a frozen candidate or a verifier verdict.

### Verifier

- Inspect the artifact independently against the supplied criteria and raw evidence.
- Report findings ordered by severity, passed checks, residual risk, and an explicit accept or reject verdict.
- Do not edit, repair, or reimplement the artifact. Send failures to the controller.
- Treat the verdict as an immutable verification artifact. Exclude status-only artifacts unless the contract explicitly includes them in scope.

## Verification Contract

For a verifier, make the general contract concrete with:

```text
Objective: Determine whether the supplied artifacts satisfy every acceptance criterion.
Artifacts and evidence:
Acceptance criteria:
Checks to run:
Out of scope: Modifying, repairing, or reimplementing the artifacts.
Completion gate: Clear accept/reject verdict with findings, passed checks, and residual risk.
```
