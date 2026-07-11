# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Screen Time Reality Check — a single-file static web app (`index.html`, German UI). You log your daily screen time for 7 days; after the seventh entry the app switches to a dashboard that translates the week's total into tangible equivalents (book pages, walks, meals, time outside). No build step, no package manager, no tests. Tailwind comes from the Play CDN (`cdn.tailwindcss.com`); a small inline `<style>` block covers what Tailwind utilities can't (slider thumb, fill transitions, glass effect).

## Developing

Open `index.html` directly in a browser, or serve the folder:

```
python -m http.server 8000
```

## Architecture

Everything lives in `index.html`: markup, a `<style>` block, and one inline `<script>`.

- **Two views, one page**: `#logView` (day entry) and `#dashboardView` (weekly evaluation) are toggled with the `hidden` class. `init()` decides which to show: fewer than 7 saved entries → log view, otherwise dashboard. Every state change (save, delete, reset) funnels back through `init()`.
- **State** is a plain array of hour numbers in `localStorage` under `screen_time_visual_entries` — index = day. Individual entries can be removed from the sidebar list; "Neu starten" clears the key.
- **Entry UI**: a 0–6h range slider (0.25 steps) with quick-set buttons; its value drives the CSS `--fill` custom property (slider track gradient), the phone-mockup fill height, and the live "Gegenwert" preview numbers.
- **Conversion rates** are inline in `updateSlider()` and `renderDashboard()`: pages = minutes × 0.3, walks = minutes / 30, meals = minutes / 45, books = total hours / 6, outside time = hours 1:1. Keep the two places consistent if you change them.
- **Chart bars** are created at `height: 0` and set to their real height inside `requestAnimationFrame` so the CSS transition animates them on render.

## Context

This project is featured on the ayaankhan-portfolio site (ayaankhan.dev), which shows a screenshot of this app as a project thumbnail. Visual changes here may warrant refreshing that screenshot (`img/project-screentime.png` in that repo).
