# FastReport.Base

**Core source files** — NOT a standalone project. Imported via `<Import>` into `FastReport.OpenSource.csproj`.

## STRUCTURE

```
FastReport.Base/
├── Engine/       # Report engine (19 files, ReportEngine.*.cs partials)
├── Export/       # HTML (6 files) + Image (1 file) export filters
├── Data/         # 45 files — XML, CSV, JSON, DB, business objects
├── Barcode/      # 28 files — QR, Aztec, DataMatrix, PDF417, etc.
├── Utils/        # 45 files — helpers, serialization, fonts, images
├── Preview/      # Prepared pages, bookmarks, outline, cache
├── Table/        # Table object (13 files)
├── Code/         # Script compilation (C#/VB code providers)
├── Functions/    # Number-to-words (19 locale variants) + std functions
├── Format/       # 10 format types (date, currency, percent, etc.)
├── Gauge/        # Gauge object (linear, radial, simple)
├── CrossView/    # Cross-tab/cube view (10 files)
├── Matrix/       # Matrix/pivot table (11 files)
├── Import/       # Import from DevExpress, StimulSoft, JasperReports, RDL
└── TypeConverters/ # 12 type converters
```

## WHERE TO LOOK

| Task | Location | Notes |
|------|----------|-------|
| Band rendering | `Engine/` | 19 partials, ReportEngine.*.cs |
| HTML export | `Export/Html/` | 6 files: HTMLExport.cs, Draw, Layers, Styles, Templates, Utils |
| Image export | `Export/Image/` | Single ImageExport.cs |
| QR code | `Barcode/QRCode/` | Has Java migration artifacts (`UPGRADE_TODO`) |
| Serialization | `Utils/FRReader.cs`, `FRWriter.cs` | Custom XML serialization |
| Font handling | `Utils/FontManager*.cs` | Cross-platform font management |

## CONVENTIONS

- **No .csproj** — this is an MSBuild include target, not a real project
- **Partial classes** split between here and `FastReport.OpenSource/` (`.Core.cs` suffix)
- **Async variants** use `.Async.cs` suffix (e.g., `Report.Async.cs`)
- **LangVersion 9.0** inherited from parent project

## ANTI-PATTERNS

- **`FastReport.Base.csproj` is fake** — no SDK, no TargetFramework. Cannot build standalone.
- **`ImageHelper.cs:226`** — Memory leak on cross-platform image conversion
- **`Encoder.cs`** — 5 `UPGRADE_TODO` artifacts from Java-to-.NET migration
- **`TextRenderer.cs:2538-2653`** — 10 boilerplate Dispose TODOs (auto-generated, likely incomplete)
