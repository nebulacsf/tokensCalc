# AGENTS.md

## What this is
Single-file static app: `index.html` is the entire AI token-cost calculator. UI copy and comments are Spanish. There is no `package.json`, build step, test suite, linter, or local dependency; Tailwind and fonts load from CDNs.

## Workflow
- Edit `index.html` directly and reload the page. There are no install/build/test/lint commands to run.
- To preview locally: `python3 -m http.server 8000`, then open http://localhost:8000. (Opening the file via `file://` also works.)
- Do not introduce a toolchain or split files for small changes; the single-file design is intentional.

## Non-obvious details
- Tailwind is configured inline in `<head>` via `tailwind.config`. The `brand-*` colors and `dark-*` colors (`bg-dark-card`, `border-dark-border`, `bg-dark-input`, `bg-dark-accent`, `bg-dark-bg`) are custom, not stock Tailwind. Keep that config intact and in sync with any new classes.
- Prices are USD per 1M tokens; `calcSingleCost` divides token counts by `1000000`.
- `PRESETS` (bottom script) is the source of truth for model names/prices. Preset buttons are generated with inline `onclick="selectPreset(...)"`, so the handler must stay exposed on `window`.
- The three usage profiles (`quick`, `std`, `cx`) are wired by hardcoded IDs in three coupled places: the HTML markup, the `attachEventListeners` element array, and `calculateCosts`. Adding or renaming a profile/token field requires updating all three.
- `formatUSD` intentionally varies decimal precision by volume and switches to 6 decimals for amounts below `$0.0001`.
- All new user-facing text should be Spanish to match the existing UI.
