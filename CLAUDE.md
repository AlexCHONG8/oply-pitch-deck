# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Oply is an EFB (Empty Fruit Bunch) strand board — a plywood/MDF replacement manufactured from oil palm waste. Developed by Reimagine ME Sdn. Bhd. (Malaysia). The project is in pre-commercial stage with FRIM and MTIB certified test results validating performance.

## Repository Purpose

This is a research, documentation, and investor-facing materials project. It contains:
- Investor pitch deck and investment memo as self-contained HTML presentations
- Test certification documents (FRIM, MTIB) — scanned image PDFs, not text-extractable
- Market research data and analysis (Markdown)
- An investor study report comparing Oply against other agri-waste MDF/plywood replacements
- Reference material (Green Pulse magazine article on LIGNO50N)
- Supporting images (product photos, test data scans, competitor logos, maps)

## HTML Presentations

Two self-contained HTML files (~1640 lines each) are the primary investor deliverables. They include inline CSS/JS (no external dependencies except Chart.js and Google Fonts CDN). **No build step required** — open directly in a browser.

**`oply_pitch_deck.html`** — 30-slide McKinsey-style PE/VC pitch deck. Dark theme (navy #0B1221, gold #C9A84C, teal #00BFA5). Uses Chart.js for 12 charts. Architecture:
- Slides are `<section class="slide" data-slide="N">` elements, 1-indexed via `data-slide`
- Navigation: `goTo(index)` takes 0-based DOM index; keyboard arrows, bottom nav tabs, swipe
- Charts: lazy-initialized in `initCharts(slideIdx)` via `chartMap` (keys are 0-based DOM indices mapping to canvas IDs like `chart3`, `chart7`)
- Map (Slide 21): real Borneo SVG (`images/borneo_blank_map.svg`) with CSS-filter darkening and absolute-positioned HTML overlay markers using percentage coordinates

**`Oply_Investment_Memo_CN.html`** — Chinese-language investment memo. Similar structure and styling.

**Critical implementation note:** When editing chart initialization, the `chartMap` keys must be `data-slide value minus 1` (0-based DOM index), not the `data-slide` number itself. Off-by-one here causes charts to render on the wrong slide or not at all.

## CSS Design System

The HTML files use a comprehensive inline CSS system with CSS custom properties:
- **Colors**: `--bg` (#0B1221), `--surface` (#131B2E), `--gold` (#C9A84C), `--green` (#00BFA5), `--red` (#FF5252)
- **Layout classes**: `.split-60-40`, `.split-40-60`, `.split-50-50` (grid columns with gaps)
- **Grid system**: `.grid-2`, `.grid-3`, `.grid-4`, `.grid-5` (auto-responsive grids)
- **Components**: `.card`, `.kpi`, `.pillar`, `.process-step` (reusable UI patterns)
- **Responsive**: Media queries at 768px for mobile adjustments

Common layout pattern: `.split-XX-XX` creates proportional grid layouts (e.g., `.split-60-40` = 3fr/2fr columns). All layout classes use `flex:1; min-height:0` to enable proper flex child sizing.

## Chart.js Configuration

Charts are configured with consistent styling using shared `defaultOpts`:
- **Color scheme**: Gold for primary data, green for positive metrics, red for costs/negatives
- **Typography**: DM Sans for labels, JetBrains Mono for numbers/values
- **Grid**: Subtle gray (`rgba(30,42,69,.6)`) with consistent styling
- **Responsive charts**: Set `maintainAspectRatio: false` and parent containers with explicit heights

When adding charts, follow the pattern in existing `case` statements: copy `defaultOpts`, then override specific properties (plugins, scales, datasets).

## Key Files

**Reports:**
- `Oply_Study_Report.md` — Main investor study report (cost-first MVP analysis, 13 sections + appendices, ~6,200 words)
- `Oply_Competitive_Analysis_and_USP.md` — Case studies per feedstock, pros/cons, 5-pillar USP, patent landscape, barriers to entry, China investor strategy (~3,900 words)
- `competitive_landscape_analysis.md` — Detailed competitor profiles (Samling, Daiken, Evergreen, Chinese imports, European players)

**Research data:**
- `market_research_agricultural_waste_raw_materials.md` — Raw material pricing for 7 agri-waste feedstocks, transportation costs, competitor landscape

**Source documents:**
- `OPLY_FRIM_260429_181427.pdf` — FRIM test report (scanned images, use pdftoppm + vision to extract)
- `OPLY_MTIB_260429_181528.pdf` — MTIB test report (scanned images)
- `Test Results for OPly against traditional plywood_260429_181652.pdf` — Text-extractable comparison data
- `5563b0fcbc4e0c690d1bc5246bb23f8f.jpg` — Green Pulse magazine article on LIGNO50N

**Parsed PDF data:** `parsed-pdfs/` contains JPEG extractions from scanned PDFs (FRIM, MTIB, test comparison), plus a `manifest.json` with file metadata.

**Images:** `images/` contains product photos, test data scans, aerial plantation shots, and `logos/` with competitor/institution logos (Daiken, Evergreen, FRIM, MIDA, MPOB, MTIB, RECODA, Samling, Sarawak Energy, Verra). `borneo_blank_map.svg` is from Wikimedia Commons (CC BY-SA 3.0).

## Working with PDFs

The FRIM and MTIB PDFs are scanned documents (image-based, not text-based). To extract data:
- Use `pdftotext` (from poppler) for text-based PDFs
- Use `pdftoppm -jpeg -r 300 input.pdf output` to convert scanned PDFs to images, then use vision analysis
- Do not rely on the Read tool's built-in PDF rendering for these files

## Data Conventions

- All costs in USD unless noted; Malaysian Ringgit (RM) conversions at ~RM 4.40/USD
- Board dimensions referenced as 4'×8'×12mm (standard sheet size)
- Oply density range: 600-725 kg/m³; use 650 kg/m³ for calculations
- Resin content: 11% PMDI by weight (optimized parameters from samples 70 & 71)
- Testing standards: BS EN 310, 317, 319, 311, 312

## MVP Recommendation

The study report identifies **Sarawak, Malaysia** as the top-ranked MVP location (weighted score 4.80/5.00), leveraging hydropower (28 sen/kWh), SCORE industrial land (RM 2.50/sq ft), 100% Pioneer Status, Bintulu Free Zone port (5-14 days to Shanghai), and 7-8M tonnes/year EFB supply. Key competitors in Sarawak: Samling (~100K m³/yr MDF, Miri), Daiken (~120K m³/yr MDF, Bintulu) — both use wood fiber only, no agri-waste R&D.

## Investment Context

- Target investment: $12-18M for 60,000 m³/year plant
- Malaysia carbon tax coming 2026; carbon credits via Verra VMR0009
- Malaysia actively courting Chinese biomass investment (Bloomberg Dec 2024)
- MPOB has MoA with Reimagine ME — institutional backing
- No other company produces EFB strand board in Malaysia — competitive white space

## Deployment & Development Workflow

**GitHub Pages deployment**: The repository is deployed to GitHub Pages at https://alexchong8.github.io/oply-pitch-deck/. 
- **Main branch**: `main` 
- **Remote**: `https://github.com/AlexCHONG8/oply-pitch-deck.git`
- **Deployment**: Push to `main` branch triggers automatic GitHub Pages build
- **No build process**: Static HTML files are served directly

**Development workflow**:
1. Edit HTML files directly — no build step required
2. Test changes by opening files in a browser (use `file://` protocol)
3. Commit changes: `git add <files>` and `git commit -m "description"`
4. Deploy: `git push origin main`
5. Changes go live within seconds on GitHub Pages

## Common Issues & Solutions

**Charts not rendering**:
- Check browser console (F12) for Chart.js errors
- Verify `chartMap` key is `data-slide value minus 1` (0-based DOM index)
- Ensure canvas element ID matches the chart ID in `chartMap`
- Hard refresh browser (Ctrl+Shift+R / Cmd+Shift+R) to clear cache

**Layout overflow/truncation**:
- Add `flex-wrap: wrap` to KPI rows with 4+ items
- Use `min-height` on cards/process-steps for consistent sizing
- Check for missing `flex:1` on grid children
- Verify container heights are explicitly set (e.g., `height: 320px`)

**Map positioning issues**:
- City positions use percentage coordinates: `top: X%; left: Y%`
- Ensure cities are positioned on land, not in water (check Borneo geography)
- Bintulu (central-north Sarawak coast), Kuching (southwest), Miri (north near Brunei)
- Shipping route should go directly north from Bintulu, not northeast

**Image/PDF issues**:
- FRIM/MTIB PDFs are scanned images — use `pdftoppm -jpeg -r 300` for conversion
- Images in `images/` directory are referenced relatively in HTML
- SVG map allows CSS filtering and overlay positioning
- Logos are in `images/logos/` subdirectory

## Slide Structure Patterns

**Standard slide template**:
```html
<div class="slide" data-slide="N">
  <h3>Section Name</h3>
  <h2>Slide Title</h2>
  <div class="split-60-40" style="margin-top:16px">
    <!-- Content here -->
  </div>
  <div class="slide-sources">
    <span class="source-label">Sources:</span>
    <!-- Source links -->
  </div>
</div>
```

**Key slide numbering**: `data-slide` attributes are 1-indexed for human readability, but JavaScript uses 0-based DOM indices. When adding charts, always use `data-slide value minus 1` in the `chartMap`.

**Common slide layouts**:
- `.split-60-40`: Main content + sidebar (common for charts with explanations)
- `.split-50-50`: Equal split (good for comparisons)
- `.grid-2/.grid-3/.grid-4`: Card grids (good for KPIs, features, pillars)
- `.timeline` (custom): Vertical process flows
- `.kpi-row`: Horizontal metric displays (add `flex-wrap: wrap` for 4+ items)

**Common development commands**:
- `git status` — check modified files
- `git add oply_pitch_deck.html` — stage pitch deck changes
- `git commit -m "Description"` — commit with message
- `git push origin main` — deploy to GitHub Pages
