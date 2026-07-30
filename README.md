# Camden Family Trip 2026

A one-page trip planner for the family's week at the Lake Megunticook house
near Camden, Maine (Aug 1–8, 2026): weather outlook, a loose day-by-day plan,
local activity ideas, a Lake Megunticook fishing report, and a shared
packing list everyone can check off together.

This is a one-time build for a single trip — it does not auto-refresh.

## Packing list

Already set up — the packing list is backed by a free Firebase Realtime
Database (project `camden-trip-2026`), so everyone's checkmarks sync live
across devices. Nothing to configure; it just works when the page loads.

Test mode security rules (open read/write to anyone with the URL — fine
for a low-stakes shared list) expire 2026-08-29, well past the trip.

### If you ever need to redo this setup

The steps below are kept for reference in case the Firebase project gets
deleted or you want to point the page at a different one.

1. Go to **[console.firebase.google.com](https://console.firebase.google.com)**
   and sign in with any Google account.
2. Click **Add project**, give it any name (e.g. `camden-trip-2026`), and
   finish the creation wizard (you can skip Google Analytics).
3. In the left sidebar, go to **Build → Realtime Database**, click
   **Create Database**, choose any location close to you, and start it in
   **test mode**. Test mode means the database is open read/write to
   anyone with the URL — fine for a low-stakes shared packing list, not
   something you'd use for sensitive data.
4. Click the gear icon next to **Project Overview → Project settings**.
   Under **Your apps**, click the **`</>`** (web) icon to register a new
   web app (any nickname is fine, no need to set up Hosting).
5. Firebase will show you a `firebaseConfig` object with six values:
   `apiKey`, `authDomain`, `databaseURL`, `projectId`, `storageBucket`,
   `appId`. Copy them.
6. Open `index.html` in this repo, find the `firebaseConfig` object near
   the bottom (search for `PASTE_API_KEY_HERE`), and replace the six
   placeholder strings with your real values.
7. Save, commit, and push. Reload the live page — the packing list banner
   should disappear and the status line should say "Live — N items".

### Locking it down / cleaning up after the trip

Test mode database rules expire automatically after 30 days, after which
reads/writes will start failing. That's fine for a one-week trip, but if
you want to tidy up sooner: delete the Firebase project from the console
(Project settings → General → scroll down), or tighten the Realtime
Database rules under **Build → Realtime Database → Rules**.

## Local preview

```
python3 -m http.server 8000
```

then open `http://localhost:8000`. Opening `index.html` directly via
`file://` will NOT work — the packing list uses an ES module import that
browsers block on the `file://` protocol, so you'll only see a static
page with no packing list.

## Structure

- `index.html` — the entire site (HTML/CSS/JS, no build step, no
  dependencies besides the Firebase SDK loaded from Google's CDN)
- `docs/plans/` — design and implementation notes from when this was built
