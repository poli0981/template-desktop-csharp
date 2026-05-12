# Releasing — runbook

The end-to-end process for cutting a new release of a project built from this
template. The same flow applies whether you're shipping the template itself
(unusual) or an app spun off from it (common).

## Versioning

[Semantic Versioning 2.0.0](https://semver.org/spec/v2.0.0.html):

- **MAJOR** — incompatible API or binary change. Increment when users have to
  do work to upgrade.
- **MINOR** — backwards-compatible feature add.
- **PATCH** — backwards-compatible bug or security fix.

Tags use the prefix `v`: `v1.4.0`, `v1.4.1`, `v2.0.0-rc.1`.

## Pre-release checklist

- [ ] `main` is green (`build`, `codeql` checks passing).
- [ ] No open Dependabot security PRs untriaged.
- [ ] [`CHANGELOG.md`](../CHANGELOG.md) `[Unreleased]` section reflects what's
      actually in `main`.
- [ ] Version in `Directory.Packages.props` / `Directory.Build.props` matches
      the tag you're about to push.
- [ ] All `TODO(release)` and `FIXME` markers triaged.

## Cut the release

```powershell
# 1. Roll [Unreleased] forward in CHANGELOG.md
#    Move the section under a dated header: ## [1.4.0] - YYYY-MM-DD
#    Append a new empty [Unreleased] block above it.
#    Update the compare links at the bottom.

# 2. Commit the changelog bump
git add CHANGELOG.md Directory.Packages.props Directory.Build.props
git commit -S -m "chore(release): v1.4.0"
git push

# 3. Tag (signed) and push the tag
git tag -s v1.4.0 -m "release: v1.4.0"
git push origin v1.4.0
```

Pushing the tag fires [`.github/workflows/release.yml`](../.github/workflows/release.yml):

1. Builds + tests on `windows-latest`.
2. Publishes `src/App` as self-contained `win-x64`.
3. Installs `vpk` and packs Velopack artifacts into `release/`.
4. Creates the GitHub Release with auto-generated notes and a linked
   "Announcements" Discussion thread.
5. Attaches `*-full.nupkg`, `*-delta.nupkg`, `Setup.exe`, and `RELEASES`.

Optional follow-ups from the org-wide workflows (only if configured):

- `announce-release.yml` posts to Discord.
- `notify-release-pipeline.yml` keeps the team channel in sync as the pipeline
  progresses.

## Post-release verification

- [ ] Download the `Setup.exe` from the Release page on a clean Windows VM,
      install, and run the app once.
- [ ] Bump a tiny patch in `main` (e.g. comment fix), push a tag `v1.4.1-rc.0`,
      run `vpk` locally, point an installed copy at the dev feed, and confirm
      auto-update applies the delta.
- [ ] Check the Discussion thread was opened in the correct category.

## Rolling back

GitHub Releases are immutable in spirit but editable in practice. To unpublish
a broken release:

```powershell
# 1. Mark the release as a pre-release (hides it from the "latest" tag)
gh release edit v1.4.0 --prerelease

# 2. Communicate: edit the Discussion thread to point users at the previous
#    stable version. Pin the thread.

# 3. Cut v1.4.1 with the fix.
```

Avoid deleting tags or releases entirely — auto-update clients use the
`RELEASES` file to walk the version history and will get confused.

## Yanking a compromised release

If a release contains a security vulnerability or a malicious dependency:

1. **Open a private Security Advisory** before touching the public release.
2. Mark the release as a pre-release **and** edit its description to point at
   the advisory.
3. Cut a patch release with the fix.
4. **Don't delete the bad release** — clients that already auto-updated to it
   need the `RELEASES` file to roll forward.

## Hotfix releases off an older minor

When `main` has diverged but you need to patch an older line (e.g. enterprise
users still on `v1.3.x` while `main` is at `v2.x`):

```powershell
git checkout -b release/1.3 v1.3.4
# cherry-pick the fix
git cherry-pick <sha>
# bump CHANGELOG, packages
git commit -S -m "chore(release): v1.3.5"
git tag -s v1.3.5 -m "release: v1.3.5"
git push origin release/1.3 v1.3.5
```

`release.yml` runs against the tag regardless of which branch produced it.

## Manual re-run

If you need to re-trigger the release workflow for an existing tag:

- **Re-announce only** —
  Actions → *Announce Release to Discord* → Run workflow → pass `tag`.
- **Re-build + re-attach assets** —
  Actions → *Release* → Run workflow → pass `tag`. The `gh release upload`
  step uses `--clobber` so it overwrites existing assets.
