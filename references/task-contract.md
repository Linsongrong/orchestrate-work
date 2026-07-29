# Task Contract

Use this structure when assigning a worker. Omit fields that truly do not apply, but never omit scope or deliverable.

```text
Objective:
Relevant context and settled decisions:
Owned scope:
Out of scope:
Deliverable:
Acceptance criteria:
Required evidence or checks:
Time budget or completion gate:
Escalate when:
```

Require this compact return shape:

```text
Status: complete | partial | blocked
Result or artifact locations:
Evidence and checks:
Material decisions or findings:
Unresolved risks:
Controller action needed:
```

## Worker Rules

- Solve only the assigned objective.
- Read only the context needed for the owned scope.
- Treat the task contract as authoritative; do not reconstruct or request the full parent conversation unless a specific missing fact blocks progress.
- Preserve user changes and avoid unrelated edits.
- Do not create additional agents unless the contract authorizes further decomposition.
- Verify the result before returning it.
- Stop when the contract's completion gate is met; do not broaden investigation to improve completeness cosmetically.
- At the time limit, return the best evidence collected and mark remaining gaps instead of continuing silently.
- Report uncertainty and failed checks directly; do not hide them behind a completion claim.
- Return compact evidence and artifact locations instead of raw logs or a chronological transcript.

## Verifier Contract

```text
Objective: Determine whether the delivered artifacts satisfy the acceptance criteria.
Artifacts and raw evidence:
Acceptance criteria:
Checks to run:
Out of scope: Reimplementing the solution unless explicitly authorized.
Return: Findings ordered by severity, passed checks, residual risk, and a clear accept/reject verdict.
```
