# GitHub Copilot — Repository Instructions

GitHub Copilot Workspace and Copilot Chat in the IDE read this file. The full
agent ruleset is in [`../AGENTS.md`](../AGENTS.md); this file only highlights
the bits Copilot is most likely to violate.

- Target **C# 14 / .NET 10 LTS**. Use modern language features:
  primary constructors, file-scoped namespaces, collection expressions,
  pattern matching, `Span<T>` (implicit conversions in C# 14), records when
  they fit.
- **Nullable reference types are on.** Don't paper over warnings with `!`;
  fix the annotation.
- **No WPF / WinForms / MAUI / Avalonia**. WinUI 3 via Windows App SDK 1.8.x only.
- **Central Package Management is on.** When suggesting a new NuGet, add the
  `PackageVersion` to [`../Directory.Packages.props`](../Directory.Packages.props),
  then a versionless `PackageReference` to the `.csproj`.
- **Conventional Commits.** PR titles must start with `feat:` / `fix:` / `docs:`
  / `chore:` / `ci:` / `refactor:` / `test:`.
- **GPG signed commits required.** Don't suggest `--no-gpg-sign` or `--no-verify`.

See [`../AGENTS.md`](../AGENTS.md) for the full rule set.
