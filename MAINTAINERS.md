# Maintainers

This file lists the people responsible for steering this template (and the
repos forked from it). For routing of PR reviews see
[`.github/CODEOWNERS`](.github/CODEOWNERS) — CODEOWNERS is enforced by GitHub,
this file is the human-readable narrative.

## Active maintainers

| Handle | Role | Areas | Contact |
|---|---|---|---|
| [@poli0981](https://github.com/poli0981) (SkullMute) | Lead maintainer | everything | `lopop05905@proton.me` |

## How decisions get made

Solo project today. Decisions made by the lead maintainer in:

1. **GitHub Issues** for bugs / concrete features.
2. **GitHub Discussions** for direction questions ("should we adopt X?").
3. **PR comments** for design questions tied to a specific change.

There is no formal RFC process. If you have a large change in mind, open a
Discussion first so we can agree on scope before code is written.

## Adding a new maintainer

A contributor becomes eligible after:

- 3+ merged PRs of meaningful size, and
- demonstrating they understand the template's "stay on LTS, stay quiet"
  philosophy (no dependency rollbacks, no green-test-by-disabling-it patterns).

To propose someone, open an Issue tagged `maintainership` that includes:

- the candidate's GitHub handle,
- which areas they'd cover (CI, docs, UI, packaging, …),
- a link to the PRs that earned the proposal.

The lead maintainer responds within 14 days.

## Stepping down

Open a PR removing yourself from this file and from
[`.github/CODEOWNERS`](.github/CODEOWNERS). No drama, no need to justify.

## Emergency contact

If the lead maintainer is unreachable for **30+ days** and there's a critical
security advisory or build break:

1. Open a public Issue tagged `emergency-handover`.
2. Email the address above.
3. After 14 more days of no response, the community may fork. The template
   license (chosen per-repo) governs what you can do with the fork.
