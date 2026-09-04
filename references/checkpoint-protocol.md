# Checkpoint Protocol

Use a checkpoint only when work is expected to span multiple waves, compaction or resume, or long-lived remote coordination. Single-wave orchestration skips it. A worker never owns, writes, or restores one.

Keep one compact controller-owned checkpoint in task or plan state when available. Update it at phase start, material dispatch or decision, blocker, and phase completion. Keep only goal and completion criteria; phase and next action; active workstreams with owner/status; blockers; important decisions; latest verified evidence; and, only when chronology matters, a short append-only milestone timeline.

Do not copy transcripts, raw worker returns, logs, or the dispatch ledger. Link authoritative artifacts and evidence instead. The checkpoint is not evidence and never duplicates routing, ownership, authority, budget, or evidence-reuse decisions.

After actual compaction or resume, reconcile checkpoint claims with current user direction and authoritative Git (or equivalent), tests, and produced artifacts before dispatching, accepting, or reusing evidence. Discard or correct stale claims and results that no longer match the objective or candidate identity, then update the checkpoint and ledger. When no checkpoint exists, reconstruct only the minimum control state from user direction and authoritative artifacts, tests, and ledger; never infer verification or completion from a narrative.
