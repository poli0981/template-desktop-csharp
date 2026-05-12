# GitHub Workflows — reference

Every YAML under `.github/workflows/` in this template, what it does, and what
secrets / repo settings it needs.

Two categories:

- **Local** — the workflow's body lives in this repo.
- **Reusable caller** — a thin shim that delegates to `poli0981/.github/.github/workflows/<name>.yml@main`.
  Edit the caller for inputs; edit the org-wide repo for the actual logic.

---

## CI & quality

| Workflow | Type | Trigger | Purpose |
|---|---|---|---|
| [`build.yml`](../.github/workflows/build.yml) | Local | push / PR / manual | Restore → build → test on `windows-latest` with .NET 10. Uploads test results. |
| [`codeql.yml`](../.github/workflows/codeql.yml) | Caller → `codeql-csharp.yml` | push / PR / weekly Mon 04:00 UTC | Static analysis, `security-extended,security-and-quality` query suites. |

## Release pipeline

| Workflow | Type | Trigger | Purpose |
|---|---|---|---|
| [`release-drafter.yml`](../.github/workflows/release-drafter.yml) | Local | push to `main`, PR events | Maintains a rolling draft release built from merged PR titles. |
| [`release.yml`](../.github/workflows/release.yml) | Local | tag `v*.*.*` / manual | Build, test, publish + Velopack `vpk pack`, attach to GitHub Release, open Discussion in "Announcements". |
| [`announce-release.yml`](../.github/workflows/announce-release.yml) | Caller → `announce-release.yml` | release published | Discord announcement. |
| [`notify-release-pipeline.yml`](../.github/workflows/notify-release-pipeline.yml) | Caller → `notify-release-pipeline.yml` | any workflow completion | Discord status updates for the release pipeline. |

## Notifications & ops

| Workflow | Type | Trigger | Purpose |
|---|---|---|---|
| [`notify-ci-failure.yml`](../.github/workflows/notify-ci-failure.yml) | Caller → `notify-ci-failure.yml` | any workflow completion | Discord alert when a workflow on `main` fails. |

## Community automation

| Workflow | Type | Trigger | Purpose |
|---|---|---|---|
| [`stale.yml`](../.github/workflows/stale.yml) | Local | daily 02:00 UTC | Marks/closes inactive issues + PRs. Exempts `pinned`, `security`, `keep-open`. |
| [`lock-threads.yml`](../.github/workflows/lock-threads.yml) | Local | daily 03:00 UTC | Locks closed issues/PRs after 30 days, Discussions after 60. |
| [`labeler.yml`](../.github/workflows/labeler.yml) | Local | PR opened/sync | Auto-labels PRs based on changed paths (config: [`.github/labeler.yml`](../.github/labeler.yml)). |
| [`security-advisory-discussion.yml`](../.github/workflows/security-advisory-discussion.yml) | Local | `repository_dispatch: security-advisory-published` / manual | Opens a public Discussion when an Advisory ships. |

## What needs configuring per-repo

### Secrets (Settings → Secrets and variables → Actions)

| Secret | Used by | Required? |
|---|---|---|
| `GITHUB_TOKEN` | most workflows | provided automatically |
| `OPS_GH_TOKEN` | org-wide `dependabot-summary`, `weekly-digest` (in `poli0981/.github`) | only if you opt in |
| `DISCORD_*_WEBHOOK` | notify / announce workflows in `poli0981/.github` | only if you opt in |
| `CODE_SIGN_PFX_BASE64` + `CODE_SIGN_PASSWORD` | `release.yml` (commented stub) | optional |
| `VELOPACK_FEED_TOKEN` | `release.yml` (commented stub) | optional |

### Repo settings

- **Discussions** → enable. Create categories: `Announcements`, `Security`, `Q&A`, `Ideas`, `Show & tell`.
- **Branch protection on `main`** → require `build` + `codeql` checks, require signed commits, require linear history, require 1 review (or 0 if solo).
- **Code scanning default setup** → can co-exist with `codeql.yml`, but prefer the workflow-driven path.
- **Dependabot alerts** → enable; the included `dependabot.yml` adds version updates on top.

### Org-wide reusable workflows live in [`poli0981/.github`](https://github.com/poli0981/.github)

If you fork this template into a different org, either:

1. Change the `uses:` lines in the caller workflows to point at your org's `.github` repo, **or**
2. Inline the reusable workflow's body into the caller (copy from
   `https://github.com/poli0981/.github/blob/main/.github/workflows/<name>.yml`).
