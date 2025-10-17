# Mumble Catppuccin Mocha

Mumble themes using the Catppuccin Mocha palette (several accent variants) and a few non‑Catppuccin examples.

This repository contains both the source Sass files (in `source/`) and precompiled `.qss` theme files (in `themes/`) so you can either use the themes directly or recompile them after customizing tokens.

Quick highlights
- Precompiled themes: `themes/CatppuccinMocha/*.qss`, `themes/Dracula/Dracula.qss`, `themes/TextCatppuccin/textcatppuccin.qss`
- Source Sass: `source/` contains entrypoints and definition files. Vendor imports are under `source/Imports/` and should not be modified directly.

Quick install (use precompiled `.qss`)
1. Copy a `.qss` file from the `themes/` directory to your machine.
2. In Mumble: Configure → Settings → Interface → Enable custom theme → Browse → select the `.qss` file.
3. Apply and restart Mumble if required.

Build from source (PowerShell / dart-sass)
```powershell
cd .\source
# Build one variant
sass --style=expanded .\CatppuccinMochaPink.scss ..\themes\CatppuccinMocha\CatppuccinMochaPink.qss

# Build all (example) - run each entrypoint you need
sass --style=expanded .\CatppuccinMochaMauve.scss ..\themes\CatppuccinMocha\CatppuccinMochaMauve.qss
sass --style=expanded .\CatppuccinMochaBlue.scss  ..\themes\CatppuccinMocha\CatppuccinMochaBlue.qss
sass --style=expanded .\Dracula.scss              ..\themes\Dracula\Dracula.qss
```

Customization notes
- Edit token values in `source/` (prefer cloning vendor definitions when changing foundational surfaces).
- Keep the order: definitions → overrides → `@import 'Imports/Base Theme';` → micro overrides.

License & attribution
See individual files for license text. Upstream Mumble theme is permissive; Catppuccin assets are from the Catppuccin project.

Contributions
- Open a PR with small, focused changes: new definitions, new compiled `.qss` and a README update for new themes.
