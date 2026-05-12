# Developer Machine Specifications

> This document describes the hardware and software environment used by the
> maintainer across **all** of their projects, not just this template. It is
> mirrored to Vietnamese in [`i18n/vi/pc_spec.md`](i18n/vi/pc_spec.md).

## Primary developer machine

| Component | Details |
|-----------|---------|
| **OS** | Windows 11 Pro 25H2 Insider Preview (Dev Channel) |
| **Build** | 26300.8376 |
| **CPU** | Intel Core i7-14700KF |
| **GPU** | NVIDIA GeForce RTX 5080 (16 GB VRAM) |
| **RAM** | 32 GB DDR5 |
| **Storage** | 1 TB SSD |
| **IDE** | JetBrains IDEs 2026.x (paid) + Visual Studio Code |

## Mobile test devices

Used for verifying any web companion / browser-extension layer:

| Device | OS | Browsers |
|---|---|---|
| iPhone 14 Pro | iOS 26.x | Chrome, Brave |
| iPhone 13 Pro Max | iOS 26.x | Chrome, Brave |

## Companion docs in repos derived from this template

- [`docs/pc_spec.md`](pc_spec.md) — this file, public hardware spec (EN).
- [`docs/dev_env.md`](dev_env.md) — IDE + language toolchains + dev workflow (EN).
- [`docs/i18n/vi/pc_spec.md`](i18n/vi/pc_spec.md), [`docs/i18n/vi/dev_env.md`](i18n/vi/dev_env.md) — Vietnamese mirrors.
- `webapp/TAURI.md` — only present when a project ships a Tauri 2 sidecar.

## Toolchain versions installed on the dev machine

These are the **superset** available locally; each individual project pins what it
actually uses in `global.json` / `Directory.Packages.props` / `package.json` /
`Cargo.toml`. Anything not used by a project does not need to be listed in that
project's own `dev_env.md`.

- **.NET SDKs** — 8.x, 9.x, 10.x, 11.x (preview)
- **Python** — 3.12.x and 3.14.x
- **Node.js** — ≥ 24.x LTS (currently 24 LTS "Krypton")
- **Rust** — stable channel via `rustup`
- **Git** — recent stable, **GPG signing enforced** (`commit.gpgsign=true`)

Projects that adopt a newer language / runtime than what's listed here will add an
entry to this table when they land.
