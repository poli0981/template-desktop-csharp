# template-desktop-csharp

> A production-ready GitHub template for **C# 14 / .NET 10 LTS desktop apps** —
> offline-first, WinUI 3 + Fluent 2, Velopack auto-update, supply-chain hardened
> out of the box.

Click **Use this template → Create a new repository** to scaffold a new project
from this baseline.

---

## What it's for

- Native **Windows desktop apps** — productivity tools, browser-extension companion apps,
  capture / clipboard / automation utilities, small games, anything that runs locally.
- **Offline-first** by default: the app must work with no network. Optional network features
  (auto-update, telemetry, sync) are opt-in and degrade gracefully when offline.
- Useful as the seed for **shell extensions, tray apps, single-window tools** — anywhere a
  packaged `.exe` + auto-update story beats Electron / Tauri.

## Why this template

Every dependency, language version, and GitHub Action is pinned to a release that is
**still in active LTS / current support after 2028-01-01**, so a freshly-spawned repo
should start with **zero open Dependabot or CodeQL Code-scanning alerts**.

| Layer | Pick | Supported until | Why |
|------|-----|------|-----|
| Runtime | **.NET 10 LTS** | 2028-11-10 | LTS, ships C# 14, AOT-friendly, faster JIT |
| Language | **C# 14** | (= .NET 10) | extension members, field-backed props, implicit `Span<T>` |
| UI | **WinUI 3** via Windows App SDK 1.8.x | rolling LTS | native Fluent 2, dark/light, Mica, no WebView baggage |
| Design | **Fluent 2** | rolling | Microsoft's current design system, Figma-aligned |
| MVVM | **CommunityToolkit.Mvvm 8.4.x** | active | source-gen `ObservableProperty`, AOT-safe |
| Installer / updater | **Velopack** (latest) | active | one-command installer + delta updates, replaces Squirrel/ClickOnce |
| IDE | **Visual Studio 2026** or **JetBrains Rider 2026.x** | rolling | C# 14 + Win App SDK templates |
| (optional) JS sidecar | **Node.js 24 LTS "Krypton"** | 2028-04-30 | only if you ship a web companion / installer GUI |
| CI | `actions/checkout@v6`, `actions/setup-dotnet@v5`, `github/codeql-action@v3` | rolling | latest GA pins |

When a dependency upgrade lands, **bump versions in this file + `Directory.Packages.props`
together** so the table stays the source of truth.

## Quick start

```powershell
# 1. Use this template on GitHub, clone your new repo, then:
dotnet --version          # expect 10.0.x
dotnet workload install windowsappsdk    # WinUI 3 workload
dotnet restore
dotnet build -c Release

# 2. Run the app
dotnet run --project src/App
```

Pack a release with Velopack (filled in once the project is scaffolded):

```powershell
dotnet tool install -g vpk
vpk pack --packId YourApp --packVersion 0.1.0 --packDir publish/
```

## Repository layout (planned)

```text
.
├── .github/                    # workflows, issue/PR templates, FUNDING, dependabot
├── docs/
│   ├── pc_spec.md              # dev machine spec (EN)
│   ├── dev_env.md              # toolchain & workflow (EN)
│   ├── ABOUT_THE_AUTHOR.md     # social / contact
│   └── i18n/vi/…               # Vietnamese mirrors
├── src/                        # application code (added by you)
├── tests/                      # unit / integration / UI tests
├── CHANGELOG.md
├── CODE_OF_CONDUCT.md
├── CONTRIBUTING.md
├── SECURITY.md
├── SUPPORT.md
├── ACKNOWLEDGEMENTS.md
├── LICENSE                     # intentionally empty — pick after `Use this template`
└── README.md
```

## CI / automation included

See [.github/workflows/](.github/workflows/) — every workflow either runs in this repo
directly or calls a reusable workflow from [`poli0981/.github`](https://github.com/poli0981/.github).

| Workflow | Trigger | Purpose |
|---|---|---|
| `build.yml` | push / PR | restore → build → test (.NET 10, Release) |
| `codeql-csharp.yml` | push / PR / weekly | CodeQL static analysis for C# |
| `release.yml` | tag `v*` | build, sign, publish GitHub Release + Velopack feed |
| `announce-release.yml` | release published | Discord announcement (reusable) |
| `notify-ci-failure.yml` | any workflow fails on `main` | Discord alert (reusable) |
| `notify-release-pipeline.yml` | release pipeline events | Discord status (reusable) |
| `auto-create-discussion.yml` | release / critical security advisory | open a Discussion thread |
| `stale.yml` | daily cron | close stale issues / PRs |
| `labeler.yml` | PR opened | auto-label PR by path |

Optional / configure-per-repo:

- `dependabot.yml` — weekly NuGet + GitHub Actions updates, grouped to one PR per area.
- `lock-threads.yml` — lock issues 30 days after they close.

## Status

`v0.0.0` — template only. No application code yet. First real app spun off from this
template will define the `src/` baseline.

## Author & contact

See [`docs/ABOUT_THE_AUTHOR.md`](docs/ABOUT_THE_AUTHOR.md).

## Funding

If something built from this template helps you, you can chip in via the
**Sponsor ❤** button at the top of any repo created from it (configured in
[`.github/FUNDING.yml`](.github/FUNDING.yml)).

## License

Not set yet — `LICENSE` is intentionally empty. Pick one (MIT / Apache-2.0 / GPL-3.0 /
proprietary) **per new repo** after running *Use this template*.
