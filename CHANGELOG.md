# Changelog

All notable changes to this template are recorded here. Format follows
[Keep a Changelog 1.1.0](https://keepachangelog.com/en/1.1.0/) and the project
adheres to [Semantic Versioning 2.0.0](https://semver.org/spec/v2.0.0.html).

Repos spun off from this template should **replace** this file with their own
on first commit and start at `0.1.0`.

## [Unreleased]

## [0.1.0] - 2026-05-12

### Added

- **Community health** — README, CHANGELOG, CODE_OF_CONDUCT, CONTRIBUTING,
  SECURITY, SUPPORT, ACKNOWLEDGEMENTS, MAINTAINERS, AGENTS (+ CLAUDE and
  Copilot pointers), empty LICENSE (chosen per derived repo).
- **Build baseline** — `global.json` pins .NET 10 LTS, `Directory.Build.props`
  enforces C# 14 + nullable + warnings-as-errors + deterministic + SourceLink,
  `Directory.Packages.props` adopts Central Package Management with
  WindowsAppSDK 1.8.x, CommunityToolkit.Mvvm 8.4.2, Velopack 0.0.1298,
  Microsoft.Extensions.* 10.0, xUnit, FluentAssertions, NSubstitute.
- **Tooling** — `NuGet.config`, `.editorconfig`, `.gitattributes`, `.gitignore`,
  `.markdownlint.json`, `.vscode/{extensions,settings}.json`,
  `.config/dotnet-tools.json` (`vpk`, `dotnet-format`, `dotnet-outdated`).
- **Docs** — `docs/{pc_spec,dev_env,WORKFLOWS,ABOUT_THE_AUTHOR,TEMPLATE_SETUP,
  RELEASING,ROADMAP,ARCHITECTURE}.md` with Vietnamese mirrors for the
  user-facing ones in `docs/i18n/vi/`.
- **GitHub Actions** — `build.yml` (restore→build→test on windows-latest),
  `codeql.yml` (guarded caller of `poli0981/.github/codeql-csharp`),
  `release.yml` (Velopack pack + GitHub Release + Discussion in Announcements),
  `release-drafter.yml` + native `.github/release.yml`,
  `security-advisory-discussion.yml`, `stale.yml`, `lock-threads.yml`,
  `labeler.yml`, `lint.yml` (markdownlint + actionlint).
- **Repo config** — `dependabot.yml` with grouped weekly NuGet + Actions PRs,
  `FUNDING.yml` (GitHub, Ko-fi, Buy Me a Coffee, Patreon, PayPal), four
  issue/PR templates, `CODEOWNERS`, `.github/labeler.yml`,
  `.github/release-drafter.yml`.

### Notes

- Versions verified 2026-05-12 against active LTS / current support windows
  through at least 2028. Fresh repos created from this template should start
  with zero Dependabot or CodeQL alerts.

## [0.0.0] - 2026-05-12

- Empty starting point.

[Unreleased]: https://github.com/poli0981/template-desktop-csharp/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/poli0981/template-desktop-csharp/releases/tag/v0.1.0
[0.0.0]: https://github.com/poli0981/template-desktop-csharp/releases/tag/v0.0.0
