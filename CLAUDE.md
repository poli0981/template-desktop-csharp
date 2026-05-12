# CLAUDE.md

Claude Code reads this file. The repo-wide agent rules live in
[`AGENTS.md`](AGENTS.md) — every editor / agent tool uses the same set of
constraints, so do not duplicate them here.

## Claude-specific notes

- The maintainer communicates in **Vietnamese**. Respond in Vietnamese when
  they write in Vietnamese, English otherwise. Code, commits, docs, and PR
  titles stay in English.
- The maintainer's tone is casual / indie. Drop the corporate hedging.
- This repo has a shared org-level `.github` repo at
  [`poli0981/.github`](https://github.com/poli0981/.github) with reusable
  workflows. **Don't inline what's reusable** — call it.
- Memory: prefer updating `memory/` files in
  `~/.claude/projects/E--template-desktop-csharp/memory/` over re-deriving
  context from `git log` for each session.

Everything else: see [`AGENTS.md`](AGENTS.md).
