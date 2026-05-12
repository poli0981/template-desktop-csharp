# Roadmap

The template itself has a small scope on purpose — it should stay opinionated,
not grow into a kitchen sink. This file lists the things I want to ship into
the template versus the things I deliberately won't.

Status legend: ✅ done · 🚧 in progress · 📅 planned · ❓ considering · 🚫 out of scope

## v0.1 — baseline (now)

- ✅ Community health files (README, CHANGELOG, CoC, CONTRIBUTING, SECURITY,
      SUPPORT, ACKNOWLEDGEMENTS, MAINTAINERS).
- ✅ `Directory.Build.props` + `Directory.Packages.props` + `global.json` for
      .NET 10 LTS / C# 14.
- ✅ Build, CodeQL, Release, Release-Drafter, Stale, Lock-threads, Labeler,
      Security-advisory-discussion workflows.
- ✅ Dependabot config (grouped NuGet + Actions, weekly).
- ✅ Issue/PR templates.
- ✅ AGENTS.md + Copilot/Claude pointers.
- ✅ Empty LICENSE — picked per-repo.

## v0.2 — scaffolding (next)

- 📅 A minimal `src/App/` WinUI 3 sample with MVVM Toolkit wired up.
- 📅 A minimal `tests/App.Tests/` xUnit project.
- 📅 `tools/` directory with a `New-VelopackRelease.ps1` helper.
- 📅 `webapp/` Tauri 2 sidecar option (commented-out scaffold, opt-in).
- 📅 Internationalization scaffolding (`Strings/en-US/Resources.resw` +
      `Strings/vi/Resources.resw`).

## v0.3 — supply-chain hardening

- 📅 SBOM generation in `release.yml` (CycloneDX or SPDX).
- 📅 `cosign` keyless signing for Velopack `.nupkg` artifacts (in addition
      to or instead of PFX).
- 📅 GitHub Actions pinned to full SHA + Dependabot for SHA-based pins.
- 📅 OIDC-based publishing to NuGet.org / private feed.

## v0.4 — diagnostics

- ❓ Built-in crash reporter (Sentry vs. Microsoft AppCenter sunset vs. roll
      our own).
- ❓ OpenTelemetry / structured logging out of the box.
- ❓ Velopack telemetry: install/uninstall/update counters.

## v0.5 — distribution

- ❓ MSIX packaging path alongside Velopack.
- ❓ Microsoft Store submission workflow.
- ❓ Auto-generated download landing page (GitHub Pages from `docs/site/`).

## Out of scope

- 🚫 WPF / WinForms / MAUI / Avalonia support. WinUI 3 only.
- 🚫 Cross-platform Linux/macOS desktop. Use Tauri 2 (or a different template)
      for that.
- 🚫 Server / backend / API templates. This template is desktop-only.
- 🚫 Mobile (Android, iOS).
- 🚫 Electron, Tauri 1.x, Tauri 2 as the *primary* shell (it's only ever a
      sidecar here).
- 🚫 Long-term support for non-LTS .NET. The template moves to the next LTS
      within a quarter of GA.

## How to influence the roadmap

- Open a **Discussion** in the `Ideas` category.
- Don't open an Issue for a roadmap item that isn't planned yet — Issues are
  for things concrete enough to act on within a couple of releases.

This file gets reviewed at every minor-version release and pruned aggressively.
Anything that sits in 📅 for two minors without traction moves to ❓ or 🚫.
