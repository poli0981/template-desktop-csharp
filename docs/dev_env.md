# Development Environment

> Toolchain, IDE setup, and day-to-day workflow for C# desktop apps spun off
> from this template. Hardware lives in [`pc_spec.md`](pc_spec.md). Vietnamese
> mirror: [`i18n/vi/dev_env.md`](i18n/vi/dev_env.md).

## IDEs

| IDE | Use for |
|---|---|
| **JetBrains Rider 2026.x** | day-to-day C# 14 / WinUI 3 / refactoring / debugging |
| **Visual Studio 2026** | XAML hot-reload, MSIX packaging, designer-heavy work |
| **VS Code** | quick edits, markdown, workflow YAML, Tauri sidecars |

WinUI 3 designer support is best in Visual Studio 2026 with the **Windows App SDK
C# Templates** component installed.

## .NET toolchain

```powershell
# Verify
dotnet --info        # expect 10.0.x SDK
dotnet --list-sdks
dotnet workload list # should include 'windowsappsdk'

# Install / repair workload if missing
dotnet workload install windowsappsdk
```

Pin the SDK per repo with `global.json`:

```json
{
  "sdk": { "version": "10.0.100", "rollForward": "latestFeature" }
}
```

## Central package management

Use `Directory.Packages.props` so every `.csproj` in the solution shares pinned
versions. Bump in one place; Dependabot opens a single grouped PR.

## Git

- `commit.gpgsign = true` — all commits signed.
- Conventional commits encouraged (`feat:`, `fix:`, `chore:`, …) but not enforced.
- Default branch: `main`. Release branches: `release/x.y`.
- See [`CONTRIBUTING.md`](../CONTRIBUTING.md) for branch & PR flow.

## Local CI parity

Mirror what `.github/workflows/build.yml` runs:

```powershell
dotnet restore
dotnet build  -c Release --no-restore
dotnet test   -c Release --no-build --verbosity normal
```

## Auto-update / packaging — Velopack

```powershell
dotnet tool update -g vpk
dotnet publish src/App -c Release -r win-x64 --self-contained -o publish/
vpk pack --packId <YourApp> --packVersion <X.Y.Z> --packDir publish/
```

Release artifacts (`*-full.nupkg`, `*-delta.nupkg`, `Setup.exe`) get attached to
the GitHub Release by `.github/workflows/release.yml`.

## Optional layers

| Need | Add |
|---|---|
| Web companion | Node.js 24 LTS — pin in repo's `.nvmrc` and `package.json#engines` |
| Native sidecar | Rust stable + Tauri 2 — drop into `webapp/` per the template's convention |
| Browser extension | a separate sibling repo — link from README |
