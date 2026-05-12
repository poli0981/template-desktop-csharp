# Contributing

Thanks for your interest! This file applies to **the template itself**. Each
project spawned from this template should fork this file and adjust it to match
its own scope before its first public release.

## Before you start

- Read the [Code of Conduct](CODE_OF_CONDUCT.md).
- For non-trivial changes, open a Discussion or draft Issue first so we can
  agree on direction. Small fixes can skip straight to a PR.
- This template targets the **latest LTS / current** language and dependency
  versions (see [README.md](README.md#why-this-template)). Suggestions that
  drag the floor down to an out-of-support version will not be merged.

## Dev setup

See [`docs/dev_env.md`](docs/dev_env.md). Minimum: .NET 10 SDK + the
`windowsappsdk` workload.

```powershell
dotnet --info
dotnet workload install windowsappsdk
dotnet restore
dotnet build  -c Release
dotnet test   -c Release --no-build
```

## Branch & PR flow

1. **Fork** (external) or **branch** (collaborator) from `main`.
   Naming: `feat/<short-slug>`, `fix/<short-slug>`, `chore/<short-slug>`.
2. **Commit** with GPG signing on. Conventional Commits encouraged
   (`feat:`, `fix:`, `docs:`, `chore:`, `ci:`).
3. **Open a PR** against `main`. Keep PRs small and self-contained — split
   refactors from feature work.
4. CI must pass (`build`, `codeql-csharp`, formatting). If a hook fails locally,
   fix it — do **not** use `--no-verify` or `--no-gpg-sign`.
5. Maintainer **squash-merges** to keep history linear. The PR title becomes the
   commit message; make it good.

## Coding standards

- C# style: `dotnet format` enforced. EditorConfig in repo root.
- Nullable reference types: **enabled**. No `#nullable disable` without an
  inline comment explaining why.
- AOT / trimming friendly where reasonable (most WinUI apps publish trimmed).
- Public APIs need XML doc comments **only if** they are exposed to consumers
  outside the project. Internal helpers do not.

## Tests

- Unit tests live in `tests/<Project>.Tests/`. Use xUnit + FluentAssertions by
  convention; pick another framework only if you have a reason.
- UI smoke tests for WinUI go in `tests/<Project>.UITests/` using
  [WinAppDriver / Appium](https://github.com/microsoft/WinAppDriver) or
  [Playwright for .NET](https://playwright.dev/dotnet/).
- New behavior needs at least one test; a bug fix needs a regression test.

## Releasing (maintainers)

1. Bump the version in `Directory.Packages.props` / project files.
2. Update [`CHANGELOG.md`](CHANGELOG.md) — move `[Unreleased]` into a dated
   release header.
3. Tag: `git tag -s vX.Y.Z -m "release: vX.Y.Z"` then `git push --tags`.
4. `release.yml` builds, signs, packs Velopack, and creates the GitHub Release.
5. `announce-release.yml` posts to Discord automatically.

## Security

Do **not** report security issues through public Issues. See
[`SECURITY.md`](SECURITY.md).

## License

Code contributed to a repo derived from this template is licensed under
whatever `LICENSE` file that repo ships. **This template's `LICENSE` is
intentionally empty** — each downstream project picks one explicitly on first
commit.
