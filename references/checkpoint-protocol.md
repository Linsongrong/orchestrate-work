# Checkpoint Protocol

Use this protocol in orchestration mode for every assurance profile. It preserves the controller's recovery position across compaction without replacing the task ledger. Direct mode does not create a checkpoint by default, and a worker never owns, writes, or restores one.

## Controller Checkpoint

Keep one compact controller-owned checkpoint in task or plan state when available. Update it at phase start, before or after a material dispatch or decision, on a blocker, and at phase completion. Keep only:

- goal and completion criteria;
- current phase and next action;
- active workstreams with owner and status;
- blockers;
- important decisions;
- latest verified evidence; and
- a short append-only timeline of material milestones when chronology is needed.

Do not copy the transcript, raw worker returns, logs, or the routing ledger into the checkpoint. Link or name authoritative artifacts and evidence instead. Keep detailed routing, ownership, authority, budget, and evidence-reuse decisions in the ledger under their existing rules.

## Restore After Compaction

Before resuming controller action, read the checkpoint and reconcile it with the latest user direction and authoritative state: Git or equivalent source control, current test results, and produced artifacts. Treat the checkpoint as a recovery aid, not evidence. Discard or correct stale claims, reject results that no longer match the objective or candidate identity, then update the checkpoint and ledger before dispatching, accepting, or reusing evidence.

When no checkpoint is available, reconstruct only the minimum necessary control state from user direction and authoritative artifacts, tests, and ledger state; do not infer missing verification or completion from a prior narrative.
