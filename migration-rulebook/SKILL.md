---
name: migration-rulebook
description: Design and stress-test the rulebook, semantic gap inventory, dependency map, and parity judge for a large language or framework migration before translating the full codebase. Use when planning a structure-preserving port or major automated migration.
allowed-tools: Read, Glob, Grep, Bash, Write, Edit
---

# Migration Rulebook

Prepare a migration so later work can be mechanical, resumable, and judged against observable behavior.

## Establish the judge first

1. Identify behavior that both the source and target implementations can expose.
2. Separate portable tests from tests coupled to source-language internals.
3. Define parity checks for public behavior, output, performance-sensitive contracts, and supported platforms.
4. Run the judge against the source implementation.
5. Deliberately introduce or simulate a known defect and confirm the judge fails.

Do not weaken, skip, or delete tests merely to make the target pass.

## Choose the migration shape

- For a structure-preserving port, write a rulebook that maps source types, idioms, ownership, errors, concurrency, FFI, and build conventions to target equivalents.
- For a redesign, write an architecture document with explicit compatibility boundaries and use disposable end-to-end prototypes instead of line-by-line comparison.

Record unresolved decisions rather than silently choosing inconsistent translations.

## Build the planning artifacts

Create or update the locations requested by the user. Otherwise propose, but do not write without authorization:

- `migration-rulebook.md`: deterministic translation rules and examples
- `migration-gaps.md`: semantic differences the normal rules cannot safely cover
- `migration-dependencies.md` or a machine-readable equivalent: translation order and coupled units
- `migration-judge.md`: commands, fixtures, thresholds, and exit conditions

Prefer generating dependency data with repository tools or a deterministic script. Mark inferred edges separately from declared dependencies.

## Stress-test before fan-out

Select a small, representative sample containing both ordinary and difficult code.

1. Translate the sample strictly with the rulebook.
2. Independently translate the same sample using target-language expertise.
3. Compare the results for behavioral, ownership, error-handling, and architecture differences.
4. Convert recurring differences into better rules or explicit gap entries.
5. Adversarially review the rulebook and gap inventory together for contradictions.
6. Discard the pilot translations unless the user explicitly asks to retain them.

The output of the pilot is a stronger process, not production migration progress.

## Return

- migration shape and rationale
- parity judge and evidence it can fail
- artifacts created or proposed
- pilot sample and lessons encoded into the rulebook
- remaining blockers before full translation
- estimated safe batch boundaries and validation cadence
