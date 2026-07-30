# Camden Family Trip Planner — Design

## Purpose

A single self-contained `index.html` (styled in the spirit of the
`westport-fishing-report` site) hosted on GitHub Pages for a 17-person family
trip to a house on Lake Megunticook, near Camden, Maine, Aug 1–8, 2026. It
gives the family one link with the week's weather outlook, a rough itinerary,
local activity ideas, a Lake Megunticook fishing report, and a shared,
live-updating packing list.

This is a one-time build for a single trip, not an auto-refreshing site.

## Content sections

1. **Masthead** — trip title, dates, Lake Megunticook / Camden, live countdown
   to Aug 1.
2. **Weather outlook** — best-available forecast for the week, researched at
   build time. Noted as approximate for the back half of the week.
3. **Rough itinerary** — day-by-day Sat→Sat, loose (not rigid) plan: a lake
   day, a Camden village/harbor day, a hike (Mt. Battie / Camden Hills State
   Park), food picks, deliberately open blocks for a 17-person group.
4. **Activity ideas** — grouped into three buckets: Lake stuff, Camden
   village + coast, Food & drink. Notes for kids/teens/adults/grandparents
   where relevant.
5. **Lake Megunticook fishing report** — species, access points, Maine
   freshwater license reminder. Replaces the tide-report section from the
   fishing-report template since this is a lake, not tidal water.
6. **Shared packing list** — categorized (house/kitchen shared gear, lake
   gear, personal, kids' stuff), checkable items, add-item box, optional
   "who's bringing it" field.

## Packing list architecture

Firebase Realtime Database, chosen over anonymous JSON-blob services after
testing:
- `jsonblob.com` — works with zero signup, but blobs hard-expire 24h after
  creation regardless of access. Unusable for a 7-day list.
- `kvdb.io`, `npoint.io` — errored out in testing.

Firebase gives real live sync (push updates, no polling) and is free at this
scale. It requires a one-time ~5 minute setup by the trip owner (create a
free Firebase project, enable Realtime Database in test mode, paste 6 config
values into a clearly marked spot in `index.html`). The page ships fully
wired for this — only the config values are missing until the owner adds
them. Setup steps are written into `README.md`.

No fallback code path is built for "local-only" mode — the owner chose
Firebase, so the page assumes it's configured.

## Data sourcing

Real content pulled at build time (not left as placeholders) using:
- Trail-lookup MCP for Mt. Battie / Camden Hills State Park trail details.
- Itinerary-generator MCP as a scaffold for the rough day-by-day plan.
- Web search (Firecrawl) for current local activities/events, Lake
  Megunticook fishing conditions, and the week's weather forecast.

## Deployment

New public GitHub repo `camden-family-trip-2026` (public — required for free
GitHub Pages, and confirmed fine with the owner since no sensitive info goes
on the page). Contents: `index.html`, `README.md` (Firebase setup steps).
GitHub Pages served from `main`. Repo creation and push happen only after
explicit confirmation from the owner at that step.

## Out of scope

- Auto-refresh automation (GitHub Actions / scheduled content regeneration)
  — this is a one-time build for a single trip.
- Rainy-day/group-games as a dedicated section — owner didn't select this
  focus area; incidental contingency notes may appear inline in the
  itinerary but it's not a standalone feature.
