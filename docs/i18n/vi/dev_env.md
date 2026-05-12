# Môi trường phát triển

> Toolchain, cấu hình IDE và workflow hàng ngày cho các app C# desktop sinh ra
> từ template này. Phần cứng xem ở [`pc_spec.md`](pc_spec.md). Bản EN:
> [`docs/dev_env.md`](../../dev_env.md).

## IDE

| IDE | Dùng cho |
|---|---|
| **JetBrains Rider 2026.x** | dev hàng ngày: C# 14 / WinUI 3 / refactor / debug |
| **Visual Studio 2026** | XAML hot-reload, đóng gói MSIX, làm việc nặng với designer |
| **VS Code** | edit nhanh, markdown, YAML workflow, Tauri sidecar |

Designer WinUI 3 chạy mượt nhất trên VS 2026 + component **Windows App SDK C# Templates**.

## Toolchain .NET

```powershell
# Kiểm tra
dotnet --info        # mong đợi SDK 10.0.x
dotnet --list-sdks
dotnet workload list # phải có 'windowsappsdk'

# Cài/sửa workload nếu thiếu
dotnet workload install windowsappsdk
```

Mỗi repo pin SDK qua `global.json`:

```json
{
  "sdk": { "version": "10.0.100", "rollForward": "latestFeature" }
}
```

## Central Package Management

Dùng `Directory.Packages.props` để mọi `.csproj` trong solution chia sẻ một bản
version pinned duy nhất. Bump 1 chỗ là Dependabot mở 1 PR gom nhóm.

## Git

- `commit.gpgsign = true` — tất cả commit đều ký.
- Khuyến khích conventional commits (`feat:`, `fix:`, `chore:`…) nhưng không bắt buộc.
- Nhánh mặc định: `main`. Nhánh release: `release/x.y`.
- Quy trình branch & PR xem [`CONTRIBUTING.md`](../../../CONTRIBUTING.md).

## Chạy CI ngay tại máy (parity)

Mô phỏng đúng những gì `.github/workflows/build.yml` chạy trên CI:

```powershell
dotnet restore
dotnet build  -c Release --no-restore
dotnet test   -c Release --no-build --verbosity normal
```

## Auto-update / đóng gói — Velopack

```powershell
dotnet tool update -g vpk
dotnet publish src/App -c Release -r win-x64 --self-contained -o publish/
vpk pack --packId <YourApp> --packVersion <X.Y.Z> --packDir publish/
```

Artifact (`*-full.nupkg`, `*-delta.nupkg`, `Setup.exe`) sẽ được
`.github/workflows/release.yml` đính kèm GitHub Release tự động.

## Tầng tuỳ chọn

| Cần | Thêm |
|---|---|
| Web companion | Node.js 24 LTS — pin vào `.nvmrc` và `package.json#engines` |
| Sidecar native | Rust stable + Tauri 2 — đặt trong thư mục `webapp/` |
| Browser extension | repo anh em riêng — link từ README |
