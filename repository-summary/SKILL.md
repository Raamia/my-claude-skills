---
name: repository-summary
description: Produce a compact, reusable repository map covering architecture, entry points, commands, package ownership, and tests. Use when entering an unfamiliar repository or when the existing summary is stale.
allowed-tools: Read, Glob, Grep, Bash, Write
---

# Repository Summary

## Goal

Replace repeated whole-repository discovery with one stable summary.

## Procedure

1. Read root instructions and manifest files.
2. Inspect only two directory levels initially.
3. Identify applications, packages, services, entry points, and test locations.
4. Extract package-manager scripts without printing full manifests.
5. Record generated, vendor, cache, and build directories that should be excluded.
6. Write or update `docs/agent/repository-summary.md` only when requested.

## Output sections

- Architecture
- Entry points
- Package map
- Important commands
- Test map
- Generated or excluded paths
- High-risk boundaries
- Last verified date

## Limits

- Maximum target length: 120 lines.
- Do not include file contents that can be referenced by path.
- Do not infer architecture when the repository does not establish it.
