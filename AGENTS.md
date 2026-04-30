# PROJECT KNOWLEDGE BASE

**Generated:** 2026-04-30
**Commit:** f9bc28b
**Branch:** master

## OVERVIEW

FastReport OpenSource — .NET report generator library (C#, .NET 6). Band-oriented engine with 13 band types, multi-format export (HTML/Image/PDF via plugin), web designer/preview via ASP.NET Core middleware.

## STRUCTURE

```
FastReport/
├── FastReport.Base/       # Core source files (imported as MSBuild include, NOT standalone project)
├── FastReport.OpenSource/ # Main library project (FastReport.dll) — partial class extensions
├── FastReport.Core.Web/   # ASP.NET Core Razor class library — WebReport, controllers, services
├── FastReport.Web.Base/   # Frontend assets (JS/CSS) + shared .cs files linked into Core.Web
├── FastReport.Compat/     # Compatibility shims (GDI+, compiler, type converters)
├── Extras/
│   ├── Core/FastReport.Data/   # 15+ DB connector plugins (Postgres, MsSql, MySql, etc.)
│   ├── Core/FastReport.Plugin/ # Plugins (WebP)
│   └── OpenSource/             # PDF export plugin + ReportBuilder
├── Demos/                 # MVC, SPA (React/Vue/Angular), Console, Avalonia demos
├── Tools/                 # Test project (xUnit, minimal coverage)
├── Localization/          # 28 .frl locale files
└── Pack/                  # NuGet packaging assets + custom Cake build script
```

## WHERE TO LOOK

| Task | Location | Notes |
|------|----------|-------|
| Report engine | `FastReport.Base/Engine/` | 19 files, ReportEngine.*.cs partials |
| Export filters | `FastReport.Base/Export/` | HTML (6 files), Image (1 file) |
| Data sources | `FastReport.Base/Data/` | 45 files — XML, CSV, JSON, DB, business objects |
| Barcode rendering | `FastReport.Base/Barcode/` | 28 files — QR, Aztec, DataMatrix, PDF417, etc. |
| Web report UI | `FastReport.Core.Web/Application/` | WebReport.cs, toolbar, cache, localization |
| Web API controllers | `FastReport.Core.Web/Controllers/` | Designer (3), Preview (6), Resources (1) |
| Web DI/services | `FastReport.Core.Web/Services/` | Abstract (8 interfaces) + Implementation (8 classes) |
| Frontend JS/CSS | `FastReport.Web.Base/wwwroot/` | Rollup-bundled, PostCSS-processed |
| Build scripts | `Pack/BuildScripts/` | Custom Cake-in-C# task runner |
| Tests | `Tools/FastReport.Tests.OpenSource/` | 3 test files, xUnit 2.3.1 |

## CONVENTIONS

- **Partial class split**: `FastReport.Base/*.cs` (main impl) + `FastReport.OpenSource/*.Core.cs` or `*.OpenSource.cs` (extensions). Inconsistent suffixes.
- **LangVersion**: Pinned to C# 9.0 (prevents file-scoped namespaces, global usings, record structs)
- **AssemblyName != PackageId**: Compiles to `FastReport.dll`, NuGet package is `FastReport.OpenSource`
- **Strong-name signing**: All assemblies signed via `FastReport.OpenSource.snk`
- **Conditional compilation**: `CROSSPLATFORM`, `OPENSOURCE`, `DESIGNER`, `DIALOGS`, `DEBUG` constants
- **Frontend build**: MSBuild targets run npm install → PostCSS → Rollup before `BeforeBuild`
- **No root Directory.Build.props**: Metadata duplicated across sub-project props files

## ANTI-PATTERNS (THIS PROJECT)

- **`FastReport.Base.csproj` is NOT a real project** — no SDK, no TargetFramework. Imported via `<Import>` into `FastReport.OpenSource.csproj`. Cannot build independently.
- **No CI/CD pipeline** — zero GitHub Actions, Azure Pipelines, or any CI config
- **Hardcoded version** (`2021.4.15`) in `pack.sh`/`pack.bat`
- **44+ `[Obsolete]` APIs** in Web layer — massive migration from flat properties to nested Toolbar/Designer objects
- **`ControllerBuilder.cs:208` — ValueTask<> handling broken** (marked `// TODO: NOT WORKING`)
- **`ImageHelper.cs:226` — Memory leak** on cross-platform image conversion path
- **`ControllerExecutor.cs:38` — Missing DI scope** for controller endpoints
- **Directory names with spaces**: `Demos/OpenSource/Console apps/`
- **18 `.sln` files** — each demo has its own solution

## COMMANDS

```bash
# Build main library (net6.0)
dotnet build FastReport.OpenSource.sln -f net6.0 -c Debug

# Run tests
dotnet test Tools/FastReport.Tests.OpenSource/FastReport.Tests.OpenSource.csproj -f net6.0

# Pack NuGet packages
bash pack.sh
# or
pack.bat
```

## NOTES

- .NET 6 is out of support (Nov 2024) — no net8.0/net9.0 targets exist
- Test packages are ancient: xUnit 2.3.1 (2017), Microsoft.NET.Test.Sdk 15.8.0 (2018)
- No `package-lock.json` for frontend — non-reproducible npm installs
- QR code encoder has Java-to-.NET migration artifacts (`UPGRADE_TODO` in Encoder.cs)
