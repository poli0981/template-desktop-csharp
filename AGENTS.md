# AGENTS.md

Guidance for AI coding agents (Claude Code, Cursor, GitHub Copilot Workspace,
Aider, Cline, etc.) working in this repository.

This file is the **canonical** agent doc. `CLAUDE.md` and
[`.github/copilot-instructions.md`](.github/copilot-instructions.md) are thin
pointers to this file so every tool reads the same rules.

---

## TL;DR

| Rule | Value |
|---|---|
| Default branch | `main` |
| Branch naming | `feat/<slug>`, `fix/<slug>`, `chore/<slug>`, `docs/<slug>`, `ci/<slug>` |
| Commit style | Conventional Commits (`feat:`, `fix:`, `docs:`, `chore:`, `ci:`, `refactor:`, `test:`) |
| Signing | `commit.gpgsign=true` — **never** bypass with `--no-gpg-sign` |
| Hooks | **never** bypass with `--no-verify` — fix the underlying issue |
| Merge style | squash-merge, PR title becomes the commit message |
| Language | C# 14 / .NET 10 LTS. Treat warnings as errors. Nullable on. |
| UI | WinUI 3 via Windows App SDK 1.8.x. Don't introduce WPF / WinForms / MAUI. |
| Tests | xUnit + FluentAssertions in `tests/<Project>.Tests/`. New behavior needs a test. |
| Docs | Repos derived from this template keep `docs/pc_spec.md`, `docs/dev_env.md`, VI mirrors. |

If you're about to do something not covered below, **ask first** in the PR
description rather than guessing.

---

## What this repo is

A **GitHub template repo** seeding offline-first C# 14 / .NET 10 LTS desktop
apps with WinUI 3 + Fluent 2 and Velopack auto-update. The template ships:

- Community health files (`README`, `CHANGELOG`, `CODE_OF_CONDUCT`, `CONTRIBUTING`,
  `SECURITY`, `SUPPORT`, `ACKNOWLEDGEMENTS`, `MAINTAINERS`)
- `Directory.Build.props` + `Directory.Packages.props` + `global.json` baseline
- GitHub Actions workflows (build, CodeQL, release, release-drafter, stale,
  lock-threads, labeler, security-advisory-discussion)
- Dependabot + CodeQL configuration
- Issue / PR templates

There is **no application code yet** — the first real app spun off from this
template will define the `src/` baseline.

## Don't do these things

- **Don't pull a dependency back to a non-LTS / out-of-support version.** The
  whole point of this template is that fresh repos start with zero Dependabot
  / CodeQL alerts. Bumping NuGet packages forward is fine; rolling them back
  to "make a test green" is not.
- **Don't add a `LICENSE`.** It is intentionally empty. Each repo derived from
  this template picks one after `Use this template`.
- **Don't add WPF, WinForms, MAUI, Avalonia, or Electron.** WinUI 3 only.
- **Don't add a Tauri 2 sidecar to this template.** Add it in the *derived*
  repo where it's actually needed.
- **Don't write multi-paragraph XML doc comments.** One-line summaries are
  enough for internal APIs; drop them entirely for `internal` types.
- **Don't introduce `dynamic`, runtime reflection-heavy code, or
  `[UnconditionalSuppressMessage]`** without a comment explaining why.

## Do these things

- **Pin GitHub Actions to a tagged major** (`@v6`, `@v5`). Pinning to a
  full SHA is preferred for any workflow that handles secrets — note this in
  the PR description if you bump one.
- **Update `Directory.Packages.props` and the README version table together**
  when a dependency bump lands. The README is the source of truth.
- **Add the change to `CHANGELOG.md` under `[Unreleased]`** before opening the
  PR. Group entries by `Added` / `Changed` / `Fixed` / `Removed` / `Security`.
- **Mirror Vietnamese docs** when adding new public-facing docs:
  `docs/<name>.md` (EN) + `docs/i18n/vi/<name>.md` (VI).
- **Use `dotnet format`** before committing if you touched `.cs` files.
  CI will fail on style drift.

## Tooling map

| Want to… | Use |
|---|---|
| Run the same checks as CI | `dotnet restore && dotnet build -c Release -warnaserror && dotnet test -c Release --no-build` |
| Pack a release | `vpk pack --packId <repo> --packVersion <X.Y.Z> --packDir publish/` |
| Lint markdown | `npx markdownlint-cli2 "**/*.md" "#node_modules"` |
| Lint workflow YAML | `actionlint` (`brew install actionlint` / `winget install rhysd.actionlint`) |
| See planned versions | [`docs/pc_spec.md`](docs/pc_spec.md) + [`docs/dev_env.md`](docs/dev_env.md) |
| See active workflows | [`docs/WORKFLOWS.md`](docs/WORKFLOWS.md) |

## Reusable workflows live elsewhere

Several callers in `.github/workflows/` delegate to
[`poli0981/.github`](https://github.com/poli0981/.github). If a behavior change
affects every repo (Discord notification format, CodeQL queries, etc.), the fix
goes there, **not** here. Update only the caller's inputs in this repo.

## Verbosity & memory hygiene

- Don't write planning docs to disk while working. Use this conversation /
  PR description.
- If you create new top-level files, add a one-line description to
  `README.md`'s "Repository layout" tree and to `docs/WORKFLOWS.md` if a
  workflow.
- **Never** commit anything under `.claude/`, `.idea/`, `.vs/`, `node_modules/`,
  `bin/`, `obj/`, `publish/`, `release/`. Verified against `.gitignore`.
