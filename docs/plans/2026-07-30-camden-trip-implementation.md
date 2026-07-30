# Camden Family Trip Planner Implementation Plan

**Goal:** Build a single `index.html` family trip page for Camden, Maine (Aug 1–8, 2026) with real local content and a Firebase-backed live shared packing list, ready to push to a new public GitHub repo.

**Architecture:** One self-contained static HTML/CSS/JS file, styled in the visual language of the existing `westport-fishing-report` site (serif/condensed masthead look, light/dark aware CSS variables). No build step, no framework. The only external dependency is the Firebase JS SDK (loaded from Google's CDN) for the packing list.

**Tech Stack:** HTML/CSS/vanilla JS, Firebase Realtime Database (client SDK), GitHub Pages.

This is a content-and-design build, not a logic-heavy feature — there is no unit-testable business logic, so verification steps are "render it and check it in a browser," not automated tests.

---

### Task 1: Research content

Gather real data before writing any HTML:
- Weather outlook for Camden, ME, Aug 1–8, 2026 (web search)
- Mt. Battie / Camden Hills State Park trail details (trail-lookup MCP)
- Lake Megunticook fishing info: species, access/boat launches, ME freshwater license rules (web search)
- Current Camden-area activities/events for early August: harbor, windjammer cruises, lighthouses, farmers market, restaurants/lobster shacks, breweries (web search)
- Draft rough day-by-day itinerary scaffold (itinerary-generator MCP as a starting point, then hand-edit for a loose 17-person-family pace)

Output: notes used directly in Task 3–6, not a separate file.

### Task 2: Scaffold the page shell

**Files:**
- Create: `index.html` (repo root)

Copy the CSS variable/typography/masthead pattern from
`/Users/augie/Claude/Westport Fishing HTML/westport-fishing-site/index.html`
(colors, fonts, `.wrap`, `header.mast`) and adapt the palette toward Maine
coast/lake (pines, water blue, granite) instead of the marsh/brass scheme.
Build masthead with title, dates, live countdown to Aug 1 2026, and empty
`<section>` placeholders for: weather, itinerary, activities, fishing
report, packing list.

**Verify:** Open the file directly in a browser tab (`file://`) — masthead renders, countdown ticks, no console errors.

### Task 3: Weather + fishing report sections

Fill in the weather outlook strip and Lake Megunticook fishing report
sections with the content gathered in Task 1. Note approximate/low-confidence
forecast days explicitly in the UI (e.g. "outlook, not a forecast" past ~5
days out).

**Verify:** Visual check in browser; confirm no placeholder/lorem text remains.

### Task 4: Itinerary + activity ideas sections

Build the day-by-day itinerary (Sat→Sat) and the three activity-idea groups
(Lake stuff / Camden village + coast / Food & drink) with age-group notes.

**Verify:** Visual check; confirm every day Aug 1–8 has an entry and open/flex blocks are visible for a 17-person group.

### Task 5: Packing list UI (Firebase-wired)

**Files:**
- Modify: `index.html`

Build the packing list section: categorized items (house/kitchen shared
gear, lake gear, personal, kids' stuff), checkboxes, an "add item" input,
optional "who's bringing it" text field. Wire it to Firebase Realtime
Database using the JS SDK (CDN `<script type="module">` import), with a
clearly marked `firebaseConfig` object at the top of the script left as
placeholders:

```js
const firebaseConfig = {
  apiKey: "PASTE_API_KEY_HERE",
  authDomain: "PASTE_AUTH_DOMAIN_HERE",
  databaseURL: "PASTE_DATABASE_URL_HERE",
  projectId: "PASTE_PROJECT_ID_HERE",
  storageBucket: "PASTE_STORAGE_BUCKET_HERE",
  appId: "PASTE_APP_ID_HERE",
};
```

Packing items seed data lives in the JS as a default list pushed to the DB
only if it's empty (so re-opening the page doesn't wipe changes). Use
`onValue` for live updates (real-time, not polling) and `set`/`update` on
check/add actions.

**Verify:** With placeholder config, page should show a clear inline banner ("Packing list needs Firebase setup — see README") instead of a silent JS error, until real config is pasted in.

### Task 6: README with Firebase setup steps

**Files:**
- Create: `README.md`

Write numbered steps: create Firebase project at console.firebase.google.com
→ Build → Realtime Database → create in test mode → Project settings → add
a web app → copy the 6 config values → paste into `index.html`'s
`firebaseConfig`. Include the test-mode security rule caveat (open
read/write — fine for a low-stakes family list, not for anything sensitive)
and how to lock it down or delete the project after the trip.

**Verify:** Read through once as if you're the owner with no context — steps should be followable without asking a follow-up question.

### Task 7: Local preview

Serve the folder locally and open in the Browser pane; click through every
section, check a few packing-list boxes, add a test item, confirm no
console errors. Remove any test items added.

### Task 8: Confirm before publishing

Show the owner the finished page locally. Only after explicit confirmation:
create the public GitHub repo `camden-family-trip-2026`, push, enable Pages
from `main`, and hand back the live URL plus the Firebase setup steps to do
before the trip.

---

## Out of scope (per design doc)

- Auto-refresh automation / GitHub Actions
- A dedicated rainy-day/games section
- A "local-only" packing list code path
