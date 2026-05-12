# GitHub Workflows — tham chiếu

Tổng kết mọi file YAML trong `.github/workflows/` của template này, làm gì,
và cần secret / setting nào để chạy. Bản EN: [`docs/WORKFLOWS.md`](../../WORKFLOWS.md).

Hai loại:

- **Local** — logic nằm trong repo này.
- **Reusable caller** — shim mỏng gọi
  `poli0981/.github/.github/workflows/<name>.yml@main`. Chỉnh input ở caller;
  chỉnh logic ở repo org.

---

## CI & quality

| Workflow | Loại | Trigger | Tác dụng |
|---|---|---|---|
| [`build.yml`](../../../.github/workflows/build.yml) | Local | push / PR / manual | Restore → build → test trên `windows-latest` với .NET 10. Upload kết quả test. |
| [`codeql.yml`](../../../.github/workflows/codeql.yml) | Caller → `codeql-csharp.yml` | push / PR / weekly thứ 2 04:00 UTC | Static analysis, query suite `security-extended,security-and-quality`. |
| [`lint.yml`](../../../.github/workflows/lint.yml) | Local | PR / push | `markdownlint-cli2` + `actionlint` cho YAML workflow. |

## Pipeline release

| Workflow | Loại | Trigger | Tác dụng |
|---|---|---|---|
| [`release-drafter.yml`](../../../.github/workflows/release-drafter.yml) | Local | push `main`, PR | Giữ draft release luôn cập nhật từ title PR đã merge. |
| [`release.yml`](../../../.github/workflows/release.yml) | Local | tag `v*.*.*` / manual | Build, test, publish + `vpk pack`, gắn vào GitHub Release, mở Discussion `Announcements`. |
| [`announce-release.yml`](../../../.github/workflows/announce-release.yml) | Caller → `announce-release.yml` | release published | Báo lên Discord. |
| [`notify-release-pipeline.yml`](../../../.github/workflows/notify-release-pipeline.yml) | Caller → `notify-release-pipeline.yml` | mọi workflow xong | Cập nhật trạng thái pipeline release lên Discord. |

## Notification & ops

| Workflow | Loại | Trigger | Tác dụng |
|---|---|---|---|
| [`notify-ci-failure.yml`](../../../.github/workflows/notify-ci-failure.yml) | Caller → `notify-ci-failure.yml` | mọi workflow xong | Cảnh báo Discord khi workflow trên `main` thất bại. |

## Tự động hoá cộng đồng

| Workflow | Loại | Trigger | Tác dụng |
|---|---|---|---|
| [`stale.yml`](../../../.github/workflows/stale.yml) | Local | daily 02:00 UTC | Mark + đóng issue/PR im lặng. Bỏ qua label `pinned`, `security`, `keep-open`. |
| [`lock-threads.yml`](../../../.github/workflows/lock-threads.yml) | Local | daily 03:00 UTC | Khoá issue/PR đã đóng sau 30 ngày, Discussion sau 60 ngày. |
| [`labeler.yml`](../../../.github/workflows/labeler.yml) | Local | PR open/sync | Auto-label PR theo path (config: [`.github/labeler.yml`](../../../.github/labeler.yml)). |
| [`security-advisory-discussion.yml`](../../../.github/workflows/security-advisory-discussion.yml) | Local | `repository_dispatch: security-advisory-published` / manual | Mở Discussion công khai khi Advisory được publish. |

## Cần cấu hình mỗi repo

### Secrets (Settings → Secrets and variables → Actions)

| Secret | Dùng cho | Bắt buộc? |
|---|---|---|
| `GITHUB_TOKEN` | hầu hết workflow | tự động có sẵn |
| `OPS_GH_TOKEN` | `dependabot-summary`, `weekly-digest` ở repo `poli0981/.github` | chỉ khi opt-in |
| `DISCORD_*_WEBHOOK` | notify / announce ở `poli0981/.github` | chỉ khi opt-in |
| `CODE_SIGN_PFX_BASE64` + `CODE_SIGN_PASSWORD` | `release.yml` (stub) | tuỳ chọn |
| `VELOPACK_FEED_TOKEN` | `release.yml` (stub) | tuỳ chọn |

### Repo settings

- **Bật Discussions**. Tạo category: `Announcements`, `Security`, `Q&A`,
  `Ideas`, `Show & tell`.
- **Branch protection `main`**: bắt buộc check `build` + `codeql`, signed
  commit, linear history, 1 review (hoặc 0 nếu solo).
- **Code scanning default setup** chạy song song được, nhưng nên ưu tiên
  workflow-driven (`codeql.yml`).
- **Dependabot alerts** bật; `dependabot.yml` đi kèm thêm phần version-update.

### Reusable workflow của org nằm ở [`poli0981/.github`](https://github.com/poli0981/.github)

Nếu anh fork template sang org khác, một trong hai cách:

1. Sửa `uses:` ở các caller workflow trỏ về `.github` repo của anh, **hoặc**
2. Inline body của reusable workflow vào caller (copy từ
   `https://github.com/poli0981/.github/blob/main/.github/workflows/<name>.yml`).
