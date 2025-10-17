# AI Assistant Instructions (Multi-Theme Mumble QSS Repository)

Purpose: Build and maintain multiple SCSS → QSS themes for the Mumble client. Supports:
* Catppuccin Mocha accent variants (Pink, Mauve, Blue, etc.)
* A Firefox‑aligned Pink variant (token overrides from Dark definitions)
* Non‑Catppuccin palettes (Dracula example)
* Experimental text‑centric variant (TextCatppuccin)

All compiled `.qss` artifacts live under `themes/<Family>/`. Vendor upstream lives in `source/Imports/` and MUST NOT be edited directly.

---
## 1. Repository Layout
```
source/
  <Variant>.scss                # Entrypoint (imports definitions + base + optional micro overrides)
  <Palette> Definitions.scss    # Custom definition set (outside Imports)
  Imports/                      # VENDOR (read‑only)
    Base Theme.scss             # Consumes variables to emit selectors
    Dark Definitions.scss       # Upstream dark token map
    Lite Definitions.scss       # Upstream light token map
    Catppuccin Mocha Pink Definitions.scss  # Vendor Catppuccin snapshot
    _variables.catppuccin-mocha-pink.scss   # Module forwarder
    _base-theme.module.scss                 # Module forwarder
themes/
  CatppuccinMocha/ *.qss        # Compiled Catppuccin variants
  Dracula/ Dracula.qss          # Non-Catppuccin example
  TextCatppuccin/               # Experimental text-focused variant
```

---
## 2. Core Authoring Rules
1. Never edit files in `source/Imports/`.
2. One base theme import per entrypoint (legacy `@import 'Imports/Base Theme';`).
3. Order inside entrypoint:
   a. `@use` a definitions file (vendor or cloned).  
   b. Token overrides (redeclare WITHOUT `!default`).  
   c. Import base theme (`@import 'Imports/Base Theme';`).  
   d. (Optional) Tail micro-overrides / accessibility tweaks.
4. If you need to change a value used widely inside the base theme selectors, clone a definitions file instead of post‑overriding.
5. Keep expanded output for diff readability.

---
## 3. Variant Types
### Catppuccin Accent Variant
Pattern:
```scss
@use 'Imports/Catppuccin Mocha Pink Definitions' as *; // or cloned definitions
// token overrides here (if minor)
@import 'Imports/Base Theme';
// micro overrides
```

### Firefox‑Aligned Pink
Uses `@use 'Imports/Dark Definitions'` then introduces Firefox JSON‑derived token values before importing base theme.

### New Catppuccin Accent (Mauve, Blue, etc.)
Clone the Pink definitions to a new `<Accent> Definitions.scss`, change accent progression (default → hover → pressed), create entrypoint, build.

### Non‑Catppuccin (Dracula Example)
Create a complete `Dracula Definitions.scss` defining every token the base expects (surfaces, text, borders, buttons, selection, inputs, slider, tabs, assets, fonts, link). Entrypoint:
```scss
@use 'Dracula Definitions' as *;
@import 'Imports/Base Theme';
```
Compile to `themes/Dracula/Dracula.qss`.

### Experimental (TextCatppuccin)
Maintains its own namespace variables (`$textcat-*`). Follow same ordering and isolation principles.

---
## 4. Build Commands
Run from `source/` (preferred for reliable relative paths):
```
sass --style=expanded CatppuccinMochaPink.scss  ../themes/CatppuccinMocha/CatppuccinMochaPink.qss
sass --style=expanded CatppuccinMochaMauve.scss ../themes/CatppuccinMocha/CatppuccinMochaMauve.qss
sass --style=expanded CatppuccinMochaBlue.scss  ../themes/CatppuccinMocha/CatppuccinMochaBlue.qss
sass --style=expanded Dracula.scss              ../themes/Dracula/Dracula.qss
```
Optional (root path build – ensure correct dest folder):
```
sass --style=expanded ./source/Dracula.scss ./themes/Dracula/Dracula.qss
```

Batch build suggestion (not yet committed): create `scripts/build-all.(ps1|sh)` iterating over known entrypoints.

---
## 5. Token Semantics (Abbrev)
| Category | Examples |
|----------|----------|
| Surfaces | `$base-bg`, `$sub-bg`, `$median-bg`, `$dialog-bg` |
| Text     | `$base-text`, `$sub-text`, `$base-text-disabled` |
| Accent & Selection | `$highlight-bg`, `$highlight-pressed`, `$toolbutton-on`, `$button-focus` |
| Buttons  | `$button-bg`, `$button-hover`, `$button-border` |
| Inputs   | `$input-bg`, `$input-border`, `$input-hover` |
| Scrollbar| `$scrollbar-handle`, `$scrollbar-handle-hover` |
| Slider   | `$slider-handle`, `$slider-fill`, states `-hover`, `-pressed` |
| Tabs     | `$tab-selected`, `$tab-deselected`, `$tab-hover`, `$tab-separator` |
| Assets   | `$checkbox`, `$checkbox-disabled`, `$radio`, `$radio_disabled` |
| Fonts    | `$font-family`, size tokens |
| Links    | `$link` |

Consistent naming: `$role[-state]` (e.g. `$button-hover`, `$slider-handle-pressed`).

---
## 6. When to Clone vs Inline Override
| Need | Action |
|------|--------|
| Just change accent & a few states | Inline token overrides after `@use` |
| Change foundational surfaces (background ramp) | Clone definitions file |
| Introduce new semantic category | Clone + add new tokens (ensure base theme doesn’t expect old names removed) |
| Large palette family (e.g. multi accent set) | One definitions file per accent for clarity |

---
## 7. Clean Build Checklist
1. `cd source/`.
2. Remove stale outputs (optional) `del ..\themes\CatppuccinMocha\<Variant>.qss*`.
3. Entry order: definitions → overrides → base import → micro selectors.
4. Compile with `--style=expanded`.
5. Review warnings: only deprecations from vendor are acceptable (`@import`, slash division, legacy color funcs).
6. No errors: abort & fix on any `Undefined variable` or missing import.
7. Visual/text diff output vs previous commit (only intended token-driven changes).

---
## 8. Quick Troubleshooting
| Symptom | Likely Cause | Fix |
|---------|--------------|-----|
| Undefined variable | Import order reversed | Ensure definitions before base theme |
| Colors unchanged | Override placed after base theme import | Move override above import |
| Import not found | Ran from repo root with relative path wrong | `cd source` or adjust path |
| Wrong assets | Missing `$checkbox` / `$radio` tokens in new theme | Define asset variables |

---
## 9. Adding a New Theme Family (Non‑Catppuccin)
1. Create `<Family> Definitions.scss` with full token coverage.  
2. Create `<Family>.scss` entrypoint (definitions → base).  
3. Build into `themes/<Family>/<Family>.qss`.  
4. Add a README in that folder describing palette & accent semantics.  
5. Don’t reuse Catppuccin‑specific semantic assumptions if not applicable—just keep variable names consistent so `Base Theme.scss` compiles.

---
## 10. Future Module Syntax Migration
When upstream migrates away from legacy `@import`, entrypoints can adopt:
```scss
@use 'Imports/_variables.catppuccin-mocha-pink' as *; // or your cloned definitions
@use 'Imports/_base-theme.module' as *;
```
Until then, retain `@import 'Imports/Base Theme';` for global variable resolution.

---
## 11. Safe Automation Targets
* Creating new definitions & entrypoint files outside `Imports/`.
* Adding build scripts that only invoke `sass` with expanded output.
* Appending micro overrides (focus outlines, small spacing tweaks) after base import.

Avoid automation that: rewrites vendor files, minifies output (diff noise), or restructures folder tree.

---
## 12. Contribution Flow (AI or Human)
1. Identify theme scope (accent tweak vs new family).  
2. Clone or reuse definitions accordingly.  
3. Implement tokens, compile, diff.  
4. Update relevant README / instructions if new family.  
5. Keep PR minimal: only new files + updated compiled QSS.

---
## 13. Out of Scope
* No JS/TS build tooling.  
* No bundlers or package managers needed.  
* No speculative refactors of vendor base.

---
## 14. Ask When Unsure
If a change requires editing `Base Theme.scss` or other vendor imports—stop and request confirmation. Prefer token-level solutions and tail overrides.

---
Happy theming! Maintain clarity: tokens first, base once, overrides minimal.
