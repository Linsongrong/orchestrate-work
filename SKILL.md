---
name: orchestrate-work
description: Coordinate non-trivial work through a controller, bounded worker agents, and independent verification. Use when the user explicitly asks for delegation, subagents, parallel work, or orchestration; when a task has two or more independent workstreams; or when an important result benefits from an independent check. Do not use for a straightforward localized task, tightly coupled steps that cannot run independently, or work that depends on unresolved user choices.
---

# Orchestrate Work

Coordinate complex work without making the user repeat delegation instructions. Keep the controller responsible for intent, decisions, integration, and final delivery; delegate only bounded execution.

## Decide Whether To Delegate

1. Apply the direct-work gate before any spawn: if the controller can fully inspect and verify the entire scope in three or fewer short tool calls, do not create execution workers. Multiple requested dimensions or report headings do not by themselves constitute independent workstreams.
2. Inspect the task before spawning agents.
3. Work directly when the task is small, localized, tightly coupled, or faster to complete than to specify and review.
4. Delegate when at least two workstreams are genuinely independent, a specialist can inspect a bounded area that exceeds the direct-work gate, or an independent verification pass materially reduces risk.
5. Do not delegate merely to demonstrate orchestration.
6. Respect the available concurrency limit. Spawn only agents that can make useful progress immediately.
7. Estimate coordination cost before spawning. If specifying, waiting for, and reviewing a worker is likely to cost more than roughly one quarter of doing that scope directly, keep it with the controller.

Read [routing-policy.md](references/routing-policy.md) when choosing worker roles, model class, reasoning effort, parallelism, or escalation.

## Operate As Controller

1. Restate the operative objective and acceptance criteria internally. Ask the user only when an unresolved choice would materially change the result.
2. Build a small dependency graph. Separate independent work from sequential gates.
3. Reserve for the controller:
   - interpreting user intent;
   - architecture and consequential tradeoffs;
   - resolving conflicting evidence;
   - integrating shared state or overlapping edits;
   - accepting the final result and communicating it to the user.
4. Give each worker one bounded task using the contract in [task-contract.md](references/task-contract.md). Include a time budget or explicit completion gate and pass the minimum task-local context needed.
5. Keep ownership disjoint. Never assign concurrent agents to edit the same files or mutate the same external state.
6. Continue useful controller work while workers run. Do not duplicate their assignments.
7. Integrate worker results from artifacts and evidence, not from confidence claims.
8. At a worker's time limit, request one concise progress return. If it still does not return promptly, interrupt it and continue from available evidence, recording the gap as residual risk.

## Verify Proportionally

- Require workers to run the checks appropriate to their changes.
- Add an independent verifier for high-impact, cross-boundary, ambiguous, or difficult-to-reverse results.
- Reserve a concurrency slot for required verification, or run verification as a sequential gate after workers finish.
- Do not use a separate verifier for trivial or mechanically verifiable work.
- Give the verifier the objective, artifacts, acceptance criteria, and raw evidence. Do not leak the controller's expected verdict.
- Treat missing evidence, inconsistent findings, repeated failure, or scope expansion as an escalation to the controller.

## Control Context And Cost

- Prefer the cheapest worker model and reasoning effort that can reliably complete the bounded task.
- Use low reasoning for deterministic lookup, extraction, formatting, and routine checks.
- Use medium reasoning for bounded implementation, diagnosis, synthesis, and review.
- Keep high or xhigh reasoning for ambiguous decomposition, architecture, conflict resolution, and final acceptance.
- If a requested model or effort is unavailable, choose the closest available capability and continue.
- Prefer sufficient evidence delivered on time over exhaustive exploration. Stop workers once additional investigation is unlikely to change the decision or artifact.
- Use short default collection windows: about 1-2 minutes for deterministic inspection or extraction, 2-5 minutes for a bounded review or research branch, and 5-15 minutes for bounded implementation plus checks. Exceed these only when tool runtime or task scope clearly justifies it.
- Return only decisions, material evidence, artifact locations, unresolved risks, and verification results to the main conversation. Do not paste full worker transcripts.

## Finish

Before responding to the user:

1. Confirm that every acceptance criterion is satisfied or explicitly marked unresolved.
2. Confirm that required verification completed and no necessary worker remains running.
3. Reconcile conflicting worker conclusions yourself.
4. Report one integrated outcome. Mention delegation details only when they help explain evidence, risk, or remaining work.
