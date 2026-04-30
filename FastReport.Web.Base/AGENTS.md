# FastReport.Web.Base

**Frontend assets + shared C# files** — NOT a standalone project. JS/CSS bundled into `FastReport.Core.Web`.

## STRUCTURE

```
FastReport.Web.Base/
├── wwwroot/
│   ├── js/
│   │   ├── ExportScripts/    # Export settings JS
│   │   ├── main-scripts.js   # Rollup entry point
│   │   ├── httpclient.js     # HTTP client for API calls
│   │   ├── printscript.js    # Print functionality
│   │   ├── searcher.js       # Report search
│   │   └── split.js          # Split pane
│   └── styles.css            # PostCSS entry point
├── package.json              # npm deps (rollup, postcss, cssnano)
├── rollup.config.mjs         # JS bundler config (3 outputs)
├── postcss.config.js         # CSS processor config
├── WebResources.cs           # Locale loading (XML-based)
├── Toolbar.Localization.cs   # Toolbar string resources
└── ScriptSecurity.cs         # Script security event args
```

## WHERE TO LOOK

| Task | Location | Notes |
|------|----------|-------|
| Main JS bundle | `wwwroot/js/main-scripts.js` | Rollup entry → webreport-script.bundle.min.js |
| Print script | `wwwroot/js/printscript.js` | → printscript.min.js |
| CSS source | `wwwroot/styles.css` | PostCSS-processed → styles.min.css |
| Locale system | `WebResources.cs` | XML-based, fallback to built-in English |
| Build config | `rollup.config.mjs` | 3 output bundles, terser minification |

## CONVENTIONS

- **No .csproj** — .cs files linked into `FastReport.Core.Web` via `<Compile Include>`
- **Frontend build**: Triggered by MSBuild targets in `FastReport.Core.Web/FastReport.Web.Shared.targets`
- **Rollup**: 3 IIFE bundles, conditional export settings (open-source vs full)
- **PostCSS**: Autoprefixer + cssnano minification (production only)

## ANTI-PATTERNS

- **No package-lock.json** — non-reproducible npm installs
- **Not a real project** — looks standalone but is only a dependency of Core.Web
- **No TypeScript** — all JS is vanilla/ES5-style
