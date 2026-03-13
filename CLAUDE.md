# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**BELGIUM E-SPOT** — A single-page electricity price dashboard for the Belgian EPEX SPOT Day-Ahead market. Displays hourly prices correlated with solar radiation for Louvain-la-Neuve.

The entire application lives in a single file: `index.html` (~940 lines of HTML, CSS, and JavaScript with no build step).

## Development

**Local server:**
```bash
python -m http.server 8080
# Visit http://localhost:8080
```

**Deploy to production:**
```bash
netlify deploy --dir . --prod
```

No build, no package manager, no linting toolchain — changes to `index.html` are immediately reflected on reload.

## Architecture

`index.html` is organized into three sections:

1. **CSS** (lines ~12–393): Custom properties for theming, glassmorphism styles, responsive grid layouts. Breakpoints at 768px (tablet) and 480px (mobile).

2. **HTML** (lines ~396–493): Header with live clock, weather bar, 4 KPI cards, day-selector with date picker, Chart.js canvas, heatmap strip, solar radiation strip.

3. **JavaScript** (lines ~494–939): Vanilla ES6, no frameworks.
   - Fetches Belgium Day-Ahead prices from `api.energy-charts.info`
   - Fetches solar radiation from `api.open-meteo.com`
   - Falls back through CORS proxies (codetabs.com → corsproxy.io) when direct fetch fails
   - Renders a dual-axis Chart.js line chart (prices left axis, solar radiation right axis)
   - Updates KPI cards: current price, weighted average, daily min/max
   - Handles date navigation (prev/next/today buttons and date picker)
   - Manages hover/selection state across chart, heatmap cells, and solar strip

## External Dependencies (CDN only)

- **Chart.js** — charting
- **Bootstrap Icons** — icon set
- **Google Fonts** — Inter, JetBrains Mono

## Deployment

Hosted on Netlify. Config in `.netlify/netlify.toml` publishes from the project root with no build command.

## Automatisation active

La commande `/loop` suivante a été lancée dans cette session :

```
/loop 5 minutes tu fais un push et commit si tu le juges nécessaire
```

Cela déclenche toutes les 5 minutes un commit + push automatique vers GitHub si des fichiers ont été modifiés.
