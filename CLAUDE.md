# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Two standalone HTML calculators for IKEA BESTÅ furniture wall installation — no build system, no dependencies, no bundler. Each file is a complete, self-contained app.

- **`index.html`** — Suspension rail calculator for BESTÅ cabinet frames (60 cm and 120 cm wide). Computes exact screw heights and horizontal positions for the BESTÅ wall suspension rail.
- **`tv-bench.html`** — Wall bracket calculator for BESTÅ TV benches (120 cm and 180 cm wide). Covers both wall-hung and floor-standing-with-tether modes.

## Running locally

Open either file directly in a browser, or use the VS Code Live Server extension (configured on port 5501):

```
# Via Live Server (VS Code extension)
# Right-click index.html or tv-bench.html → "Open with Live Server"
# Runs at http://localhost:5501
```

No install step, no build step, no server required.

## Architecture

Each HTML file contains all CSS, HTML markup, and JavaScript inline — no external JS files, no frameworks, no modules. Vanilla JS only.

**`index.html` key constants (all measurements in mm internally):**
- `RAIL_SCREW_OFFSET_FROM_TOP = 37` — empirically confirmed offset from cabinet top to rail screw centre
- `FRAME_ACTUAL` — actual frame dimensions (598 mm for 60 cm nominal, 1196 mm for 120 cm nominal)
- 4 mm gap is added between adjacent frames when computing cumulative positions
- 120 cm cabinets require 2 rails at ¼ and ¾ of actual width

**`tv-bench.html` key constants:**
- `BRACKET_CONFIGS` — sourced from IKEA assembly PDFs (AA-1402060 for 120 cm, AA-1402080 for 180 cm)
- Bracket screw offset is always 80 mm from bench bottom
- `LEG_HEIGHT_MM = 100` — default STUBBARP/NANNARP leg height for floor-standing mode

**Unit handling:** All internal calculations use mm. The `toMM()` / `fmt()` / `fromMM()` functions handle conversion for display and input. The unit toggle (`mm` / `cm` / `in`) is stateful and affects both input interpretation and output display. Inch output uses Unicode fraction characters (½, ¼, ¾, etc.) rounded to nearest ⅛".

**Diagram rendering:** Results include an inline SVG wall diagram built by string concatenation — `xScale`/`yScale` closures map mm values to SVG pixel coordinates with explicit margins.

## Important domain constraints

- The BESTÅ suspension rail system **only works** with the 38 cm and 64 cm tall cabinet frames, not TV benches — this distinction is prominently warned in both calculators.
- Only the **38 cm tall** TV bench can be wall-hung; the 64 cm bench is legs-only.
- TV bench bracket spacing (600 mm / 514 mm) rarely aligns with standard stud patterns (406 mm / 610 mm North American, 400 mm / 600 mm European).
