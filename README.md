# Waypoint

A goal app that treats a goal as a **route**, not a to-do list.

You name a destination, break it into waypoints, and say which waypoints depend
on which. Waypoint works out the rest: what's done, what's blocked, and — the
part that matters day to day — what you can actually start right now.

**Live app: https://asx2000.github.io/goals/**

## Install it on your phone

It's a PWA, so it installs from the browser with no app store involved.

**iPhone / iPad** — open the link in Safari, tap **Share**, then
**Add to Home Screen**. It launches full-screen with its own icon and works
offline. (Safari is required for installing; Chrome on iOS can't do it.)

**Android / desktop Chrome** — open the link and use the install button in the
address bar, or *Menu → Install app*.

## How it works

- **Waypoints have dependencies.** Each one lists what must be finished first.
- **State is derived, never set by hand.** A waypoint is *locked* until its
  dependencies are complete, then *ready*, then *in progress*, then *complete*.
  You never maintain a priority field, because the graph already knows.
- **Next up** shows only what's unblocked, in-progress first, then by due date.
  That's the "what do I do today" answer.
- **Loose ends feed the destination automatically** — any waypoint nothing else
  depends on connects straight to the goal.
- **Multiple goals**, one active at a time, switched from the header.

## Your data

Everything is stored in `localStorage` on the device you're using. There's no
account, no server, and nothing leaves your phone or laptop. Two consequences:

- Goals do **not** sync between devices.
- Clearing site data deletes them.

Use **Download JSON** / **Copy JSON** in the goals sheet to keep a backup, and
**Restore** to bring one back.

## Development

No build step, no dependencies, no framework. Static files.

Service workers need a real origin, so `file://` won't work — serve it:

```bash
python3 -m http.server 8099
# then open http://localhost:8099/
```

| File | Purpose |
|---|---|
| `index.html` | The whole app — markup, styles and logic in one file |
| `sw.js` | Service worker; cache-first app shell |
| `manifest.webmanifest` | PWA manifest |
| `icon-*.png`, `apple-touch-icon.png` | Generated from the flag mark in the header |
| `.nojekyll` | Stops GitHub Pages running the files through Jekyll |

**After changing any shell file, bump `CACHE` in `sw.js`** (e.g. `waypoint-v1`
→ `waypoint-v2`), or installed clients keep serving the old build.

Fonts are system stacks (New York / San Francisco / SF Mono on Apple devices) —
nothing is fetched at runtime, so the app looks the same offline.

## Roadmap

Deliberately left out to keep it simple: tags, priorities, time tracking,
recurring tasks, streaks, collaboration. The dependency graph *is* the
prioritisation.

Worth doing next:

- Date intelligence — flag overdue waypoints, and warn when a waypoint is due
  after the destination or after something that depends on it.
- Better layout on dense graphs: sibling labels can collide, and there's no
  crossing reduction between layers.
- Fit-to-view / zoom for goals with many waypoints.
- Undo for deletes.
