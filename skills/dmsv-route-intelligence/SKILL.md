---
name: dmsv-route-intelligence
description: >
  Use when the user wants the DMSV Intelligence Command Center for pipeline / linear
  infrastructure routing — e.g. "open the route intelligence dashboard", "show the
  Kerrobert gathering system", "compare route A / B / C", "route score", "corridor
  comparison", "AI route analysis", "proceed to phase 2". Renders a Cowork artifact:
  an executive routing console with a corridor map (layers, KP markers, crossings,
  wetlands, rail, utilities), a six-factor AI route score, route comparison cards,
  advantages / concerns, next-step phases, feature register, terrain profile, full
  comparison table, and a Phase 1 routing memo.
---

# DMSV Route Intelligence (command center artifact)

Render `templates/route-intelligence.html` as a Cowork artifact. It is a complete
executive routing console for a gathering-system corridor study (reference project:
Kerrobert Gathering System, Kerrobert, SK, HUB 1 → Kerrobert Terminal, Phase 1 of 5).

## Procedure
1. Read `templates/route-intelligence.html` (in this skill folder).
2. Call `mcp__cowork__create_artifact` with that file's contents as the HTML body. No
   `mcp_tools` are needed: the page ships with its own demonstration data set.
3. If the user has a real corridor model, replace the `ROUTES` object at the top of the
   script (name, colour, SVG path, length, score, confidence, six factors, feature
   counts and impacts, terrain range, advantages, concerns, summary). Everything else —
   KPI strip, map, cards, score ring, feature register, profile, comparison table,
   memo — derives from `ROUTES` at render time. Keep the route colours; they are a
   validated colour-blind-safe set (A `#3b82f6`, B `#0f9f8f`, C `#a855f7`).

## What the page does
- **Route intelligence** tab: KPI strip (area, selected route, length, corridor width,
  features, score, confidence, stage); layer toggles; satellite / hybrid base map; zoom;
  hover tooltips on routes and features; route comparison cards (click to select);
  AI advantages / concerns / recommendation; next-step phases with **Proceed to Phase 2**.
- **AI analysis**: all three corridors side by side.
- **Features**: full feature register by kilometre post with route / type / text filters.
- **Profile**: large elevation chart with crosshair (KP, elevation, grade).
- **Compare**: every Phase 1 factor for all routes in one table.
- **Reports**: the Phase 1 routing memo, generated from the selected route's data.

## Hard rules
- Use the template as-is; do not redesign the layout.
- The bundled figures are demonstration data for a corridor study. Do not present them
  as survey results for a real client unless the user supplied the numbers.
- Cost is intentionally absent (Phase 5). Do not add cost columns to Phase 1 views.
