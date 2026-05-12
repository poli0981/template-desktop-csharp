# Cấu hình máy phát triển

> Tài liệu này mô tả phần cứng & phần mềm dev đang dùng cho **toàn bộ** dự án
> của mình, không chỉ riêng template này. Bản tiếng Anh:
> [`docs/pc_spec.md`](../../pc_spec.md).

## Máy chính

| Thành phần | Chi tiết |
|-----------|---------|
| **OS** | Windows 11 Pro 25H2 Insider Preview (Dev Channel) |
| **Build** | 26300.8376 |
| **CPU** | Intel Core i7-14700KF |
| **GPU** | NVIDIA GeForce RTX 5080 (16 GB VRAM) |
| **RAM** | 32 GB DDR5 |
| **Lưu trữ** | SSD 1 TB |
| **IDE** | JetBrains IDEs 2026.x (bản trả phí) + Visual Studio Code |

## Thiết bị di động để test web / extension

| Máy | Hệ điều hành | Trình duyệt |
|---|---|---|
| iPhone 14 Pro | iOS 26.x | Chrome, Brave |
| iPhone 13 Pro Max | iOS 26.x | Chrome, Brave |

## Toolchain cài sẵn trên máy

Đây là **toàn bộ** runtime/SDK đang có, từng dự án sẽ pin riêng trong
`global.json` / `Directory.Packages.props` / `package.json` / `Cargo.toml`.
Cái nào dự án không dùng thì không cần ghi vào `dev_env.md` của dự án đó.

- **.NET SDKs** — 8.x, 9.x, 10.x, 11.x (preview)
- **Python** — 3.12.x và 3.14.x
- **Node.js** — ≥ 24.x LTS (hiện đang dùng 24 LTS "Krypton")
- **Rust** — stable channel qua `rustup`
- **Git** — bản stable mới, **bật GPG signing** (`commit.gpgsign=true`)

Khi dự án nào dùng phiên bản mới hơn bảng trên thì sẽ bổ sung vào dòng tương ứng.
