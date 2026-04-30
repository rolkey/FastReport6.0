# FastReport.Core.Web

**ASP.NET Core Razor class library** — WebReport UI, API controllers, DI services, middleware.

## STRUCTURE

```
FastReport.Core.Web/
├── Application/          # WebReport.cs, toolbar, cache, localization, designer settings
│   ├── Cache/            # 7 files — memory, distributed, legacy cache implementations
│   ├── Infrastructure/   # Middleware, DI extensions, ControllerBuilder, ControllerExecutor
│   ├── Toolbar/          # ToolbarButton, ToolbarElement, ToolbarInput, ToolbarSelect
│   ├── Localizations/    # 12 export settings localization classes
│   └── ReportExporter/   # Export orchestration + strategies
├── Controllers/
│   ├── Designer/         # ConnectionsController, DesignerReportController, UtilsController
│   ├── Preview/          # 6 controllers: Dialog, Export, GetPicture, GetReport, Print, Service
│   └── Resources/        # ResourcesController (SVG icons)
├── Services/
│   ├── Abstract/         # 8 interfaces (IConnectionsService, IReportService, etc.)
│   ├── Implementation/   # 8 implementations
│   └── Helpers/          # IntelliSense helper + models
├── Templates/            # 9 .cs template files (body, main, toolbar, tabs, etc.)
└── Resources/            # 23 SVG icons
```

## WHERE TO LOOK

| Task | Location | Notes |
|------|----------|-------|
| WebReport class | `Application/WebReport.cs` | Main entry point, partial class (482 lines) |
| Middleware setup | `Application/Infrastructure/FastReportMiddleware.cs` | Request pipeline |
| DI registration | `Application/Infrastructure/FastReportServiceCollectionExtensions.cs` | `AddFastReport()` |
| Controller dispatch | `Application/Infrastructure/ControllerBuilder.cs` | Reflection-based, has broken ValueTask<> |
| Cache | `Application/Cache/` | 3 implementations + distributed cache support |
| Export settings UI | `Templates/ExportSettings/` | 12 export format settings |

## CONVENTIONS

- **LangVersion 9.0** pinned in .csproj
- **Conditional DESIGNER/DIALOGS** constants gate features
- **Frontend build**: MSBuild targets in `FastReport.Web.Shared.targets` run npm/PostCSS/Rollup
- **Linked files**: `FastReport.Web.Base/*.cs` linked via `<Compile Include>` in .csproj

## ANTI-PATTERNS

- **44+ `[Obsolete]` APIs** — massive migration from flat properties to nested Toolbar/Designer objects
- **`ControllerBuilder.cs:208`** — ValueTask<> handling broken (`// TODO: NOT WORKING`)
- **`ControllerExecutor.cs:38`** — Missing DI scope for controller endpoints
- **`WebReport.cs:325`** — `ReportLoad()` and `RegisterData()` are stubbed/unimplemented
- **No package-lock.json** — frontend npm installs non-reproducible
