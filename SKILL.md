---
name: orchestrate-work
description: Coordinate non-trivial or long-running work through a high-reasoning GPT-5.6 Sol controller, bounded GPT-5.6 Sol workers, and proportional verification while protecting the controller's context. Use when the user explicitly asks for delegation, subagents, parallel work, or orchestration; when a task contains a substantial independently verifiable branch that would otherwise consume significant search, tool-output, implementation, or test context; when independent workstreams can run concurrently; or when an important result benefits from an independent check. Do not use for straightforward localized work, tightly coupled steps, trivial tool use, or work that depends on unresolved user choices.
---

# Orchestrate Work

Coordinate complex work without making the user repeat delegation instructions. Keep the controller responsible for intent, decisions, continuity, integration, and final delivery. Delegate only cohesive execution that earns its coordination cost or protects substantial controller context.

## Decide Whether To Delegate

Default to direct work. Delegation must pass the gates below before any spawn.

1. Inspect enough of the task to identify dependencies, ownership, and acceptance criteria.
2. Keep work with the controller when any of these is true:
   - the complete task can be performed and verified in no more than three short tool calls without producing substantial context;
   - the change is localized, tightly coupled to the controller's current reasoning, or faster to do than to specify and review;
   - a worker would need most of the full conversation to avoid guessing;
   - the task depends on an unresolved user choice or consequential controller decision.
3. Delegate a cohesive unit only when all of these are true:
   - its boundary and relevant context can be stated independently;
   - its result has objective acceptance criteria or evidence;
   - it can proceed without repeated controller decisions;
   - delegating isolates meaningful execution time or context rather than merely moving a short action elsewhere.
4. Require at least one concrete benefit: several search, inspection, implementation, or test cycles; potentially voluminous tool output; meaningful parallel critical-path savings; or valuable independent verification.
5. Start with one worker by default. Add a second only for a genuinely independent workstream with disjoint ownership and meaningful wall-clock savings. Use more than two execution workers only when the dependency graph clearly justifies it.
6. Keep sequentially coupled steps under one owner. Do not create separate workers for individual files, commands, report headings, or phases of one small change.
7. Do not delegate merely to demonstrate orchestration or keep all available slots busy.

Read [routing-policy.md](references/routing-policy.md) before the first spawn to choose effort, parallelism, or escalation.

## Operate As Controller

1. Restate the operative objective and acceptance criteria internally. Ask the user only when an unresolved choice would materially change the result.
2. Build a small dependency graph. Separate cohesive independent work from sequential gates.
3. Reserve for the controller:
   - interpreting user intent;
   - architecture and consequential tradeoffs;
   - resolving conflicting evidence;
   - integrating shared state or overlapping edits;
   - accepting the final result and communicating it to the user.
4. For long-running work, maintain a compact continuity checkpoint containing the objective, acceptance criteria, settled decisions, completed work, current work, next steps, and unresolved risks. Update it after each worker wave or material decision. Prefer plan or task-state tooling; do not create a repository file solely for this checkpoint unless the task needs one.
5. Give each worker one bounded task using the contract in [task-contract.md](references/task-contract.md). Include a time budget or explicit completion gate and only the task-local context needed.
6. Keep ownership disjoint. Never assign concurrent agents to edit the same files or mutate the same external state.
7. Continue useful controller work while workers run, but do not duplicate their assignments or repeat their raw exploration.
8. Integrate worker results from artifacts and evidence, not from confidence claims.
9. At a worker's time limit, request one concise progress return. If it still does not return promptly, interrupt it and continue from available evidence, recording the gap as residual risk.

## Dispatch Sol Workers

For every `spawn_agent` call, explicitly set the worker configuration when the interface supports it:

- Set `model` to `gpt-5.6-sol`.
- Set `reasoning_effort` to `low` for deterministic lookup, extraction, formatting, and routine checks.
- Set `reasoning_effort` to `medium` for bounded implementation, diagnosis, research, synthesis, review, and ordinary independent verification.
- Set `fork_turns` to `"none"` so the worker receives the task contract rather than the long controller conversation.

Use `gpt-5.6-sol` with high or xhigh reasoning for the controller's decomposition, architecture, conflict resolution, and final acceptance. This skill cannot change the already-running controller, so treat the user's current controller selection as authoritative. Do not use `fork_turns="all"` by default. If the runtime does not expose model or reasoning overrides, use its available fallback, do not claim that explicit routing occurred, and mention the limitation only when it affects confidence or the user's request.

## Verify Proportionally

- Require workers to run the checks appropriate to their changes.
- Add an independent verifier for high-impact, cross-boundary, or difficult-to-reverse results with non-obvious failure modes.
- Reserve a concurrency slot for required verification, or run verification as a sequential gate after workers finish.
- Do not use a separate verifier for trivial or mechanically verifiable work.
- Give the verifier the objective, artifacts, acceptance criteria, and raw evidence. Do not leak the controller's expected verdict.
- Treat missing evidence, inconsistent findings, repeated failure, or scope expansion as an escalation to the controller.

## Control Context And Cost

- Protect controller context by offloading bounded work that would generate substantial search results, logs, tool output, or implementation iteration.
- Do not offload a short action merely to save a few controller tokens.
- Prefer one cohesive worker over several narrow workers whose outputs must be reassembled.
- Prefer sufficient evidence delivered on time over exhaustive exploration. Stop workers once additional investigation is unlikely to change the decision or artifact.
- Use short default collection windows: about 1-2 minutes for deterministic inspection or extraction, 2-5 minutes for a bounded review or research branch, and 5-15 minutes for bounded implementation plus checks. Exceed these only when tool runtime or task scope clearly justifies it.
- Return only decisions, material evidence, artifact locations, unresolved risks, and verification results to the main conversation. Do not paste full worker transcripts.

## Finish

Before responding to the user:

1. Confirm that every acceptance criterion is satisfied or explicitly marked unresolved.
2. Confirm that required verification completed and no necessary worker remains running.
3. Refresh the continuity checkpoint for any work that will continue after this response.
4. Reconcile conflicting worker conclusions yourself.
5. Report one integrated outcome. Mention delegation details only when they help explain evidence, risk, or remaining work.
