---
name: failure-work-queue
description: Convert compiler errors, failing tests, crashes, or unfinished files into a deterministic, resumable work queue with non-overlapping batches and review gates. Use for large migrations or repair efforts where failures themselves can drive progress.
allowed-tools: Read, Glob, Grep, Bash, Agent, Write, Edit
---

# Failure Work Queue

Use machine-observable failures as the source of truth for what remains. Rebuild the queue from repository state on every cycle so work can resume after interruption.

## Safety

By default, plan the queue and print proposed commands. Execute repairs and validation only when the user's request or current workflow explicitly authorizes execution. Do not install dependencies or deploy.

## Select the source of truth

Choose the cheapest deterministic signal that represents the current phase:

- missing target files for translation
- compiler or type-checker diagnostics for build repair
- captured stack traces for startup and smoke-test repair
- failing test identifiers for behavioral parity
- CI platform and shard failures for final convergence

Save normalized results to a queue artifact when the run is too large or expensive to reproduce for every item. Preserve the command, revision, platform, and timestamp that produced it.

## Build safe batches

1. Normalize duplicate diagnostics without losing distinct root causes.
2. Group by package, crate, subsystem, file ownership, or likely root cause.
3. Identify shared files and dependency boundaries before parallelizing.
4. Assign one writer to a file or coupled unit at a time.
5. Give each worker a bounded input set and an explicit validation target.

Use separate worktrees or branches only when their disk and merge costs are justified. Do not use `git stash`, destructive resets, or broad cleanup commands as coordination mechanisms.

## Run the loop

For each batch:

1. An implementer fixes the assigned failures without expanding scope.
2. Independent reviewers search for regressions and invalid shortcuts.
3. A fixer applies supported feedback.
4. Run the narrowest meaningful check.
5. Periodically rerun the authoritative global check and rebuild the queue from its output.

Run a fast compiler or checker inside each batch when it completes in seconds. If the global check takes minutes or consumes shared resources, run it once per wave and distribute its saved diagnostics.

## Reject false progress

Do not reduce the queue by:

- stubbing required behavior
- disabling warnings or assertions without a demonstrated reason
- deleting, skipping, or weakening tests
- swallowing errors
- adding placeholders that merely compile
- making unrelated broad refactors

If a workaround needs a long justification, treat that as a signal to re-examine the design rather than proof that the workaround is safe.

## Stop conditions

Complete only when the authoritative queue is empty and the final gate passes on required platforms. Stop earlier when the declared retry or cost limit is reached, failures stop decreasing across two full waves, or progress requires a user decision.

Return queue counts by phase, fixes made, systemic rules updated, validation evidence, and unresolved failures.
