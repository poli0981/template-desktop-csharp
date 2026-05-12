# Architecture

The architecture that repos forked from this template are expected to grow
into. The template itself ships **no application code**, so this is a *target*
shape rather than a description of what's on disk today.

## Project layout

```text
src/
├── App/                       # WinUI 3 startup project — main executable
│   ├── App.xaml.cs            # composition root: DI container, logging, lifecycle
│   ├── Views/                 # XAML Pages + Windows + UserControls
│   ├── ViewModels/            # CommunityToolkit.Mvvm ObservableObject classes
│   ├── Controls/              # Reusable custom controls / templated controls
│   ├── Styles/                # Fluent 2 resource dictionaries, dark/light, accent
│   ├── Strings/               # .resw localization (en-US, vi, …)
│   ├── Assets/                # icons, images, fonts
│   └── App.csproj
├── Core/                      # platform-agnostic business logic
│   ├── Models/                # records / DTOs
│   ├── Services/              # interfaces + implementations
│   └── Core.csproj
└── Platform/                  # Windows-specific glue: tray, Shell, registry, COM
    └── Platform.csproj

tests/
├── App.Tests/                 # ViewModel + Views smoke tests
├── Core.Tests/                # business-logic units (most of the coverage)
└── Platform.Tests/            # Windows-specific integration tests

webapp/                        # OPTIONAL — Tauri 2 sidecar if the app has a web UI
docs/                          # everything you're reading
```

`App` references `Core` and `Platform`; `Platform` references `Core`. Nothing
in `Core` may reference WinUI / `Microsoft.UI.Xaml` — that boundary is what
keeps the business logic testable headless.

## Dependency layout

```text
                        ┌──────────────┐
                        │     App      │  WinUI 3, Fluent 2, MVVM Toolkit
                        └──┬────────┬──┘
                           │        │
                  ┌────────┴──┐  ┌──┴───────────┐
                  │  Platform │  │     Core     │
                  │  (Win-OS) │  │  (POCO/IO)   │
                  └────────┬──┘  └──┬───────────┘
                           │        │
                           └────┬───┘
                                ▼
                        Microsoft.Extensions.*
                        (DI, Configuration, Logging, Hosting)
```

## Composition root

`App.xaml.cs` builds an `IHost`:

```csharp
public partial class App : Application
{
    public IHost Host { get; }

    public App()
    {
        InitializeComponent();
        Host = Microsoft.Extensions.Hosting.Host.CreateApplicationBuilder()
            .ConfigureServices((ctx, s) =>
            {
                s.AddSingleton<IClock, SystemClock>();
                s.AddTransient<MainViewModel>();
                s.AddTransient<MainWindow>();
            })
            .Build();
    }

    protected override void OnLaunched(LaunchActivatedEventArgs args)
        => Host.Services.GetRequiredService<MainWindow>().Activate();
}
```

Keep DI registrations in `App.xaml.cs` until they exceed ~30 lines, then split
into extension methods on `IServiceCollection` per feature area.

## State management

- **ViewModels** are `ObservableObject` from CommunityToolkit.Mvvm. Use
  `[ObservableProperty]` and `[RelayCommand]` source generators — no manual
  `INotifyPropertyChanged` plumbing.
- **App state** that outlives a window lives in singleton services in `Core`.
- **Persisted state** (window position, recent files, user prefs) goes through
  `Microsoft.Extensions.Configuration` with a JSON provider rooted at
  `%LOCALAPPDATA%\<AppName>\settings.json`.

## Offline-first contract

- Every feature must have an offline behavior. Default to "works fully offline"
  unless the feature is fundamentally networked (auto-update, license check).
- Network calls are confined to `Core.Services.Network.*` and gated by a
  `INetworkAvailability` check. Throwing `HttpRequestException` in a ViewModel
  is a code-review red flag.
- Auto-update (Velopack) checks at startup with a short timeout and silently
  retries later; it never blocks the UI.

## Auto-update flow

```text
┌─ App start ─┐    ┌─ UpdateService.CheckForUpdate ─┐    ┌─ download delta ─┐
│             │ →  │ Velopack: HEAD /RELEASES       │ →  │ apply on quit    │
└─────────────┘    └────────────────────────────────┘    └──────────────────┘
        ↑                                                          │
        └──────────── relaunch into new version ←──────────────────┘
```

The `UpdateService` is registered in DI and started by a `BackgroundService`.
It never throws to the UI thread; failures are logged and surfaced as an
in-app status indicator.

## Logging

- `Microsoft.Extensions.Logging` interfaces only in app code.
- Provider: Serilog → file sink at `%LOCALAPPDATA%\<AppName>\logs\app-.log`,
  rolling daily, 14-day retention.
- Console sink in `Debug` builds.
- Structured fields preferred: `Logger.LogInformation("Updated to {Version}", v);`.

## Testing strategy

| Layer | Framework | Goal |
|---|---|---|
| `Core` unit tests | xUnit + FluentAssertions + NSubstitute | high coverage on business logic |
| `Platform` integration | xUnit + temp dirs / fake registry | smoke-test Windows-specific paths |
| `App` ViewModel | xUnit + MVVM Toolkit's `IMessenger` test helpers | command + property tests |
| UI smoke | WinAppDriver / Playwright for .NET | optional, run on `windows-latest` |

## What stays out of `src/`

- Build scripts — `tools/` or root `.github/workflows/`.
- Sample data — `tests/Fixtures/`, never embedded in production binaries.
- Generated code — output to `obj/` (already gitignored).
- Third-party binaries — pulled via NuGet, never vendored in the repo.
