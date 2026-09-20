---
name: adversarial-review-loop
description: Run independent implementer, adversarial reviewer, and fixer roles over a risky or high-volume change. Use when generated code, migrations, or large mechanical patches need fresh-context review and an objective acceptance gate.
allowed-tools: Read, Glob, Grep, Bash, Agent, Write, Edit
---

# Adversarial Review Loop

Separate authorship from review so the reviewer searches for failure rather than defending the implementation.

## Safety

By default, propose the loop, roles, and checks. Edit code or execute validation only when the user's request or current workflow already authorizes implementation. Do not install dependencies or deploy.

## Define the loop

Before changing code, record:

- the bounded work item
- requirements and invariants
- files or ownership boundaries
- validation command or other objective acceptance gate
- retry, time, or cost limit

If no meaningful acceptance gate exists, stop and establish one before scaling the loop.

## Keep roles independent

Use separate context windows or agents when available:

1. **Implementer** — makes the smallest change that satisfies the work item.
2. **Reviewer A** — assumes the change is wrong and searches for correctness, safety, lifecycle, compatibility, and concurrency failures.
3. **Reviewer B** — independently performs the same review without seeing Reviewer A's conclusions.
4. **Fixer** — evaluates both reviews, applies justified corrections, and rejects unsupported suggestions.

Give reviewers the diff, requirements, relevant contracts, and validation evidence. Do not give them the implementer's private reasoning or a request to confirm that the code is correct.

When the reviewers materially disagree, use an independent judge or escalate the disputed invariant to the user. Reviewers should not silently edit the implementation they are judging.

## Review targets

Prioritize defects that can survive compilation and superficial tests:

- lifecycle, ownership, cleanup, and asynchronous teardown
- semantic differences between languages or frameworks
- error and exceptional paths
- boundary values, coercion, integer width, and time handling
- concurrency, reentrancy, and shared mutable state
- platform-specific behavior and FFI contracts
- weakened assertions, skipped tests, stubs, or placeholders

Ignore style preferences unless they hide a concrete failure mode.

## Improve the process

If the same defect appears in multiple work items, update the shared rulebook, generator, prompt, or checker and rerun the affected batch. Do not keep hand-patching a systemic error.

Finish only when the acceptance gate passes after fixes and the review findings are resolved or explicitly documented. Report remaining uncertainty; do not convert it into confidence language.
