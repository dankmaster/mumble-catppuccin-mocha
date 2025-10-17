# Catppuccin Mocha Variant Builds

This folder can host compiled QSS outputs for multiple Catppuccin Mocha accent variants.

## Available Variants
- CatppuccinMochaPink.qss – Pink accent (Firefox-aligned token overrides based on Dark definitions).
- CatppuccinMochaMauve.qss – Mauve accent progression (Mauve → Lavender hover → Blue pressed).
- CatppuccinMochaBlue.qss  – Blue accent progression (Blue → Lavender hover → Sapphire pressed).

## Building
Run from repository root (or `source/` as noted) using dart-sass:

```
# From repo root
sass --style=expanded ./source/CatppuccinMochaPink.scss   ./CatppuccinMochaPink.qss
sass --style=expanded ./source/CatppuccinMochaMauve.scss  ./CatppuccinMochaMauve.qss
sass --style=expanded ./source/CatppuccinMochaBlue.scss   ./CatppuccinMochaBlue.qss

# Recommended (ensures relative Imports/ resolution):
cd source
sass --style=expanded CatppuccinMochaPink.scss  ../CatppuccinMochaPink.qss
sass --style=expanded CatppuccinMochaMauve.scss ../CatppuccinMochaMauve.qss
sass --style=expanded CatppuccinMochaBlue.scss  ../CatppuccinMochaBlue.qss
```

## Notes
- Keep builds in expanded form for diff clarity.
- Do not modify files inside `source/Imports/` – clone definitions outside for new accents.
- Each new accent should have: `YourVariant Definitions.scss` + `YourVariant.scss` entrypoint.
