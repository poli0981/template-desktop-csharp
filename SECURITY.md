# Security Policy

## Supported versions

This is a template, so only the **latest commit on `main`** is supported. Repos
derived from this template should keep their own `SECURITY.md` updated with
their actual support window.

## Reporting a vulnerability

**Please do not report security issues through public GitHub Issues, PRs, or
Discussions.**

Use one of these private channels:

1. **GitHub Private Vulnerability Reporting** — preferred.
   Go to the repo's **Security → Advisories → "Report a vulnerability"**, or
   visit `https://github.com/poli0981/<repo>/security/advisories/new`.
2. **Email** — `lopop05905@proton.me`. PGP welcome but not required.

In your report include:

- The affected version / commit SHA.
- A description of the issue and its impact.
- Step-by-step reproduction (PoC code is great).
- Any mitigation you already know about.

## What to expect

| When | What |
|---|---|
| Within **72 hours** | Acknowledgement that the report was received |
| Within **7 days** | Initial triage — confirm / dispute / ask for more info |
| Within **30 days** | Fix landed on `main` or a clear timeline for one |
| At fix time | A coordinated GitHub Security Advisory + CVE if warranted |

If we agree the issue is genuine, you'll be credited in the advisory unless you
ask to stay anonymous.

## Scope

In scope:

- Source code in this repo.
- GitHub Actions workflows in `.github/workflows/` and reusable workflows pulled
  from `poli0981/.github`.
- Anything in `docs/` that could leak credentials or enable phishing.

Out of scope:

- Vulnerabilities in upstream dependencies (file those upstream and link the
  advisory back here).
- Social-engineering attacks against the maintainer's social accounts listed in
  `docs/ABOUT_THE_AUTHOR.md`.
- DoS via abuse of free-tier GitHub Actions minutes.

## Hardening already in place

- **CodeQL** runs on every push and weekly (`.github/workflows/codeql-csharp.yml`).
- **Dependabot** opens weekly NuGet + GitHub-Actions update PRs
  (`.github/dependabot.yml`).
- All workflow `permissions:` blocks default to read-only; write scopes are
  granted job-by-job.
- Third-party Actions are pinned to a tagged major version (`@v6`, `@v5`, …)
  rather than `@main` to limit supply-chain risk. Pin to a full SHA in
  downstream repos that handle sensitive data.
- `commit.gpgsign = true` is enforced for maintainer commits.

## Acknowledgements

Researchers who follow this policy will be listed in
[`ACKNOWLEDGEMENTS.md`](ACKNOWLEDGEMENTS.md) with their permission.
