## Mumble Catppuccin Mocha (Pink Accent) Theme

This repository provides a set of Mumble client themes based on the Catppuccin Mocha color system. You can use the precompiled `.qss` files included in `themes/` or build your own from the Sass sources in `source/`.

### Included artifacts
- `source/` — Sass entrypoints and definition files used to build `.qss` artifacts.
- `themes/` — Precompiled `.qss` files ready for direct installation into Mumble:
  - `themes/CatppuccinMocha/CatppuccinMochaPink.qss`
  - `themes/CatppuccinMocha/CatppuccinMochaMauve.qss`
  - `themes/CatppuccinMocha/CatppuccinMochaBlue.qss`
  - `themes/Dracula/Dracula.qss`
  - `themes/TextCatppuccin/textcatppuccin.qss`

### Quick install (use precompiled files)
1. Copy the `.qss` file you want from the `themes/` folder to a convenient location.
2. Open Mumble → Configure → Settings → Interface.
3. Enable "Use custom theme" (or similar option).
4. Click Browse and select the `.qss` file.
5. Apply and restart Mumble if the UI doesn't refresh immediately.

Notes:
- On Windows, double‑click a `.qss` file in Explorer to confirm its location before selecting it in Mumble.
- Precompiled `.qss` files are ready to use — no additional installation is required.

### Build from source (advanced users)
To rebuild or modify themes, install a Sass tool (dart-sass is recommended). From PowerShell:

```powershell
cd .\source
# Compile the pink accent variant
sass --style=expanded .\CatppuccinMochaPink.scss ..\themes\CatppuccinMocha\CatppuccinMochaPink.qss

# Other examples
sass --style=expanded .\CatppuccinMochaMauve.scss ..\themes\CatppuccinMocha\CatppuccinMochaMauve.qss
sass --style=expanded .\CatppuccinMochaBlue.scss  ..\themes\CatppuccinMocha\CatppuccinMochaBlue.qss
sass --style=expanded .\Dracula.scss              ..\themes\Dracula\Dracula.qss
```

If your Sass install emits `.css`, rename the file extension to `.qss` — the output is compatible.

### Customization tips
- For small token tweaks, edit the appropriate definitions file under `source/` (for example `Catppuccin Mocha Pink Definitions.scss`) and rebuild the matching entrypoint.
- For major surface or token changes, clone an existing definitions file and create a new entrypoint to avoid editing vendor imports in `source/Imports/`.

### Common build issues
- "Undefined variable": run `sass` from inside `source/` so relative imports resolve.
- Colors not changing after edits: ensure overrides are placed before `@import 'Imports/Base Theme';` in the entrypoint.
- Mumble doesn't show the theme: confirm you selected the `.qss` file (not `.scss`/`.css`).

### License & attribution
This project uses a permissive approach compatible with the upstream Mumble theme license. Catppuccin palette tokens are from the Catppuccin project. See individual files for more details.

Attribution:
- Mumble Theme: https://github.com/mumble-voip/mumble-theme
- Catppuccin: https://github.com/catppuccin
# Mumble Catppuccin Mocha (Pink Accent) Theme

A custom Mumble skin variant using the Catppuccin Mocha palette with an emphasis on the Pink accent (`#f5c2e7`).

## Features
- Catppuccin Mocha base & mantle surfaces mapped to Mumble base/sub layers
- Pink accent used for: focus rings, highlights, active selections, toolbutton active states
- Harmonized hover/pressed states using Maroon / Red variants for subtle contrast
- Accessible contrast: Pink on Base background yields >4.5:1 contrast for UI selection states

## File Structure
```
source/
  CatppuccinMochaPink.scss                # Entry point (import + optional overrides)
  Imports/
    Catppuccin Mocha Pink Definitions.scss # Variable definitions (edit here for colors)
    Base Theme.scss                        # (Pulled from upstream project at build time) DO NOT MODIFY
```

## Building
From inside the `source` directory run:

```bash
sass -t expanded CatppuccinMochaPink.scss ../CatppuccinMochaPink.qss
```

If you use `dart-sass` and are on Windows PowerShell:
```powershell
sass --style=expanded .\CatppuccinMochaPink.scss ..\CatppuccinMochaPink.qss
```

If a `.css` file is generated instead of `.qss`, you can rename the extension to `.qss` (syntax is compatible).

## Installation in Mumble
1. Build the theme to generate `CatppuccinMochaPink.qss` in the repository root (alongside `Dark.qss` if present).
2. Open Mumble > Configure > Settings > Interface.
3. Enable "Use custom theme" (or similar, depending on version).
4. Browse and select the generated `CatppuccinMochaPink.qss` file.
5. Restart Mumble if required.

## Customization
Adjust any variable inside `source/Imports/Catppuccin Mocha Pink Definitions.scss`.
For example, to switch link color to Lavender:
```scss
$link: #b4befe; // lavender
```
Rebuild afterward.

## Mapping Reference (Catppuccin → Theme Roles)
| Theme Role | Color (Mocha) |
|------------|---------------|
| base-bg / dialog-bg | Base `#1e1e2e` |
| sub-bg | Mantle `#181825` |
| median-bg | Surface0 `#313244` |
| hover layers | Surface0 / Surface1 |
| borders | Surface1 `#45475a` |
| focus border | Pink `#f5c2e7` |
| selection active | Pink `#f5c2e7` |
| selection inactive | Surface0 `#313244` |
| highlight pressed | Red `#f38ba8` / Maroon `#eba0ac` |
| text | Text `#cdd6f4` |
| disabled text | Overlay0 `#6c7086` |

## Future Enhancements
- Provide light (Latte) variant
- Optional accent switching (Mauve / Blue / Green) via separate definition files
- Custom SVG recoloring (current config still uses default dark assets)

## License
Original upstream Mumble theme is under WTFPL. This variant inherits that permissive basis; Catppuccin palette licensed under MIT. Provide attribution if redistributing.

## Attribution
- Mumble Theme: https://github.com/mumble-voip/mumble-theme
- Catppuccin: https://github.com/catppuccin
