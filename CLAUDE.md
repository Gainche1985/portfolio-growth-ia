# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A static portfolio site for a Growth Marketer (French-language content). It is three self-contained HTML files served as-is (e.g. GitHub Pages): no build step, no package manager, no tests, no linter. Edit the HTML directly. Preview by opening a file in a browser or with `python3 -m http.server` from the repo root.

## Pages

- `index.html`: the main portfolio. It uses Tailwind via the Play CDN (`cdn.tailwindcss.com`) with an inline `tailwind.config` that defines the `brand` (teal) and `accent` (amber) colors, plus Chart.js from jsDelivr and Google Analytics (`G-276F274EF7`). The content is organised into anchored `<section>`s (`#growth`, `#ia`, `#ux`, `#product`, `#consentio`, `#roadmap`, `#contact`) linked from the nav. A single inline `<script>` at the bottom holds the data (`growthIdeas` array, rendered into `#growth-list` and filtered by category) and builds the two Chart.js charts. Commit history shows that most changes are content edits to these sections (missions, timeline, roadmap counters).
- `lead-scoring-ia.html`: a standalone ICP lead-scoring tool, linked from `index.html`. Plain CSS (CSS variables) and vanilla JS, with no libraries.
  - **Scoring model:** the 8 criteria are `<select id="cr-*">` elements whose `value` is the point weight, summed by `updateScore()`. `getQualification()` maps the total to tiers (≥90 icp, ≥70 hot, ≥50 warm, ≥30 medium, else cold), and `RECO_MAP` holds each tier's copy. The CSV import scores rows through `CSV_MAPS`, a separate keyword→points table. Keep `CSV_MAPS` in sync with the `<select>` option values when you change weights.
  - **Modes** (`setMode`, `hsMode`): `demo` simulates the push. `webhook` POSTs `{email, score, property}` to a user-supplied n8n/Make URL. `webhook-hubspot` POSTs to the hard-coded Make scenario `MAKE_WEBHOOK_URL`, which writes the HubSpot property `HS_ICP_PROPERTY` (`claude_lead_score_icp`).
- `Flatsy Landing Page.html`: a generated "bundled page" export (≈380 KB, mostly inline/encoded assets with a `__bundler_*` loader). Treat it as a build artifact. Don't hand-edit it; replace it with a fresh export instead. `index.html` links to it by its URL-encoded name (`Flatsy%20Landing%20Page.html`), so keep the filename if you replace it.

## Conventions

- All user-facing copy is in French. Keep it in French, with French typography.
- Each page is a single self-contained file, with inline CSS and JS in the page itself. Don't split them into separate assets unless asked.
