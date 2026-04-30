# FastReport.OpenSource

**Main library project** — compiles to `FastReport.dll`, NuGet package `FastReport.OpenSource`.

## STRUCTURE

```
FastReport.OpenSource/
├── Engine/       # ReportEngine.Core.cs, .OpenSource.cs, .Dialogs.OpenSource.cs
├── Export/       # ExportBase.OpenSource.cs + HTMLExport.OpenSource.cs
├── Data/         # Csv, DataConnectionBase, TableDataSource .Core.cs partials
├── Preview/      # PageCache.Core.cs, PreparedPage.OpenSource.cs
├── Utils/        # Config, ExportsOptions, RegisteredObjects .Core/.OpenSource.cs
├── Code/         # Ms/ (C# code provider)
├── CrossView/    # CrossViewHelper.Core.cs, CrossViewObject.Core.cs
├── Table/        # TableBase.Core.cs, TableCellData.Core.cs
├── Dialog/       # DialogPage.Core.cs
└── Matrix/       # (empty — no OpenSource extensions)
```

## WHERE TO LOOK

| Task | Location | Notes |
|------|----------|-------|
| Partial class extensions | `*.Core.cs` files | Mirror FastReport.Base structure |
| OpenSource-specific overrides | `*.OpenSource.cs` files | Conditional compilation for OPENSOURCE |
| Registered objects | `Utils/RegisteredObjects.Core.cs` | What's available in open-source build |
| Config | `Utils/Config.Core.cs`, `Config.OpenSource.cs` | Cross-platform config |

## CONVENTIONS

- **Partial class pattern**: `*.Core.cs` extends `FastReport.Base/*.cs` classes
- **`*.OpenSource.cs`**: OpenSource-specific overrides (vs commercial version)
- **AssemblyName = `FastReport`**, PackageId = `FastReport.OpenSource` — mismatch
- **LangVersion 9.0** pinned in .csproj
- **Conditional `OPENSOURCE`** constant in .csproj

## ANTI-PATTERNS

- **AssemblyName/PackageId mismatch**: `FastReport.dll` vs `FastReport.OpenSource` NuGet
- **Imports `FastReport.Base.csproj`** via `<Import>` — non-standard shared code pattern
- **No net8.0/net9.0 targets** — only net6.0 (out of support)
