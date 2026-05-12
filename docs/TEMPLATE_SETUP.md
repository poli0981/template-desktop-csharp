# Post-creation setup checklist

You just clicked **Use this template → Create a new repository**. Walk through
this list before your first commit. Most steps take seconds; skipping them
will trip up the included workflows.

## 1. Repo identity

- [ ] **Description** — set it to something concrete. Suggested format:
      *"<one-line tagline> — built from `poli0981/template-desktop-csharp`."*
- [ ] **Topics** — add at least: `csharp`, `dotnet-10`, `winui3`, `windows`,
      plus 1–2 specific to your app.
- [ ] **Homepage** (if you have a marketing site).
- [ ] **Update `README.md`** — replace the template's title, "What it's for",
      "Quick start", and `[Unreleased]` link with your project's reality.
- [ ] **Delete or keep this file** — once you finish the checklist.

## 2. Licensing

- [ ] **Pick a license.** Common choices for a desktop app:
  - **MIT** — permissive, no copyleft
  - **Apache-2.0** — permissive + patent grant
  - **GPL-3.0** — copyleft (forks must also be GPL)
  - **Proprietary** — strip `LICENSE`, replace with a `EULA.md`
- [ ] Replace the empty `LICENSE` file with the chosen license text.
- [ ] Update the `License` section in `README.md`.
- [ ] Update `Directory.Build.props` `<PackageLicenseExpression>` if you ship
      NuGet packages.

## 3. People & identity

- [ ] Replace **`poli0981`** references with your own handle (or keep them if
      you are the maintainer). Files that mention it:
  - `README.md` · `CONTRIBUTING.md` · `SECURITY.md` · `SUPPORT.md`
  - `.github/FUNDING.yml` · `.github/CODEOWNERS`
  - `.github/ISSUE_TEMPLATE/config.yml`
  - `docs/ABOUT_THE_AUTHOR.md`
- [ ] Update `.github/FUNDING.yml` with **your** donation handles, or delete it.
- [ ] Replace `docs/ABOUT_THE_AUTHOR.md` with your own bio, or delete it and
      remove the link from `README.md`.

## 4. Org-wide reusable workflows

The template's caller workflows point at `poli0981/.github`. Three options:

- **You are `poli0981`** — nothing to do. ✓
- **You forked `poli0981/.github` into your own account/org** — find/replace
  `poli0981` in `.github/workflows/*.yml`.
- **You don't have an org-wide `.github`** — either:
  - delete the callers (`announce-release.yml`, `notify-ci-failure.yml`,
    `notify-release-pipeline.yml`), or
  - inline the body from
    `https://github.com/poli0981/.github/blob/main/.github/workflows/<name>.yml`.

## 5. Discussions

- [ ] **Settings → General → Features → Discussions** — turn it on.
- [ ] **Categories** — create at least:
  - `Announcements` (Announcement format) — used by `release.yml`
  - `Security` (Open-ended) — used by `security-advisory-discussion.yml`
  - `Q&A`, `Ideas`, `Show and tell`
- [ ] Verify category **slugs** match what the workflows expect: the GraphQL
      API takes a slug, not the display name. The defaults in this template
      assume slugs `announcements` and `security`.

## 6. Branch protection

`main`:
- [ ] Require status checks: **`build`** and **`Analyze (csharp)`**.
- [ ] Require signed commits.
- [ ] Require linear history (encourages squash-merge).
- [ ] Require 1 review (or 0 if solo — but require yourself via CODEOWNERS).
- [ ] Restrict who can push to `main` to maintainers.

## 7. Secrets

Settings → Secrets and variables → Actions. None are strictly required until
you ship a signed binary. When you do:

- [ ] `CODE_SIGN_PFX_BASE64` — base64-encoded code-signing certificate
- [ ] `CODE_SIGN_PASSWORD` — PFX password
- [ ] `VELOPACK_FEED_TOKEN` — only if publishing to a private feed

If you adopt the org-wide notification workflows from `poli0981/.github`,
those add `DISCORD_*_WEBHOOK` and `OPS_GH_TOKEN`.

## 8. Dependabot & CodeQL

- [ ] **Settings → Code security → Dependabot alerts** — on.
- [ ] **Dependabot security updates** — on.
- [ ] **CodeQL default setup** — choose between this and the workflow-driven
      [`codeql.yml`](../.github/workflows/codeql.yml). The workflow gives you
      finer control of query suites; the default setup is one click. Don't run
      both for the same language.

## 9. Project skeleton

Once the housekeeping is done, scaffold `src/`:

```powershell
mkdir src tests
dotnet new sln -n YourApp
dotnet new winui3-app -o src/App
dotnet new xunit  -o tests/App.Tests
dotnet sln add src/App tests/App.Tests
```

Then update:

- [ ] `Directory.Packages.props` — add / version the packages you actually
      reference.
- [ ] `README.md` — repository layout section.
- [ ] `CHANGELOG.md` — add an `Added` entry under `[Unreleased]`.

## 10. First release

When you're ready to ship v0.1.0:

```powershell
git tag -s v0.1.0 -m "release: v0.1.0"
git push origin v0.1.0
```

`release.yml` takes it from there — see [`RELEASING.md`](RELEASING.md) for the
full runbook.
