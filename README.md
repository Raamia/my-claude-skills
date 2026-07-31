# my-claude-skills

A collection of [Claude Code](https://claude.com/claude-code) skills for everyday engineering workflows: diff review, context management, repository onboarding, session handoffs, targeted validation, and README generation.

## Install

Each skill is a self-contained directory with a `SKILL.md`. To install one, copy its directory into your `.claude/skills/` folder (global, at `~/.claude/skills/`, or project-local at `<repo>/.claude/skills/`):

```bash
cp -r <skill-name> ~/.claude/skills/
```

Claude Code picks up skills automatically from that location — no restart or registration step needed.

## Skills

| Skill | Use it when |
|---|---|
| [`concise-diff-review`](concise-diff-review) | Reviewing the current Git diff for blocking correctness, security, compatibility, performance, and test issues before commit or handoff. |
| [`context-budget`](context-budget) | Long Claude Code sessions are accumulating stale context — after major phases, repeated tool output, or before compaction/a fresh session. |
| [`context-router`](context-router) | Starting broad, ambiguous, unfamiliar, or cross-cutting work and you want the smallest sufficient set of files, skills, and checks before implementing. |
| [`create-readme`](create-readme) | Generating a README.md for a project. |
| [`repository-summary`](repository-summary) | Entering an unfamiliar repository, or an existing repo summary has gone stale. |
| [`session-handoff`](session-handoff) | Wrapping up a work phase and handing continuation off to a fresh session or another agent. |
| [`targeted-validation`](targeted-validation) | Picking the narrowest reliable validation commands after implementation, before reaching for an expensive full suite. |
| [`gpt-image-2`](gpt-image-2) | Generating images with GPT Image 2 through an existing ChatGPT Plus/Pro subscription via the local Codex CLI. Third-party skill, MIT-licensed — see its `SKILL.md` for original source and attribution. |

## Notes

- `context-router` composes `context-budget`, `targeted-validation`, `session-handoff`, and `concise-diff-review` — install all of them together if you use it.
- All skills here avoid destructive or irreversible actions by default (no auto-execution of validation/deploy commands, no dependency installs) — see each `SKILL.md` for exact limits.
- `gpt-image-2` is included as-is from its upstream source (see its `SKILL.md` header for homepage/license/GitHub links); the rest were authored for this repo.

## License

MIT — see [LICENSE](LICENSE). The `gpt-image-2` skill carries its own MIT license per its `SKILL.md` frontmatter.
