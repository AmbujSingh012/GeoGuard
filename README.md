# 🛰️ GeoGuard

**Satellite-based illegal mining detection for field officers.**

GeoGuard monitors registered mining lease boundaries using automated satellite passes, flags unauthorized excavation outside approved limits, and takes an officer through the full case lifecycle — from detection to a downloadable PDF report — without needing to visit every site in person. Access is gated behind an officer login, so every satellite check, imagery upload, and inspection photo is tied to a specific Officer ID.

---

## Why

Field officers responsible for illegal mining enforcement can't physically inspect every lease every day. GeoGuard's **Satellite Watch** answers one question automatically: *"Has anything changed at this site in the last 24 hours?"* — so officers only act on the sites that actually need attention.

## Features

- **Officer login / sign-up** — a 3D flip-card auth screen. Signing up issues a unique **Officer ID** (`GG-OFF-####`) alongside name, department and password; signing in unlocks the rest of the app
- **ID-gated access** — running a satellite check, uploading before/after imagery, or uploading an inspection photo all require a signed-in Officer ID (`requireAuth()`); every audit-log entry and case is stamped with that ID
- **Officer profile page** — a tilting 3D ID-badge card plus that officer's own history: every case they've created and every action they've logged, so an officer (or a supervisor) can see their work record at a glance
- **Satellite Watch** — per-lease monitoring feed showing last satellite pass, freshness (< 24h highlighted), and change-detection status
- **Dashboard** — live, clickable stats (total/active leases, suspected & high-risk alerts, open cases, affected area) that filter through to the real underlying data, with a subtle 3D hover tilt on each stat card
- **Leases registry** — searchable list of all registered mining leases with owner, mineral type, village, khasra number, and lease validity
- **Alerts queue** — suspected boundary violations across the district, risk-tiered (HIGH / MEDIUM / LOW)
- **Change detection workflow** — before/after imagery upload → analysis → boundary overlay → alert → case creation → field inspection → report, with steps locked in order (you can't jump ahead of where you've actually progressed)
- **Case management** — case is auto-assigned to the logged-in officer, with priority, status tracking, audit trail, all persisted locally so progress survives a page refresh
- **GeoJSON export** — download the lease + detected-change boundary for use in GIS tools
- **PDF report export** — generates a real PDF client-side (via jsPDF) with case details, disclaimer, and audit history
- Grounded in real reference systems: India's **Mining Surveillance System** (IBM/BISAG), the **MMDR Act, 1957**, and real satellite revisit cadences (PlanetScope ~daily, Sentinel-2 ~5-day)

## Design

A topographic-survey palette (moss green, clay, ochre, teal) with a static contour-line watermark and slow-drifting satellite illustrations in the background, Fraunces for headlines and IBM Plex Sans/Mono for body text and data. The auth screen and officer ID badge use CSS 3D transforms (`perspective`, `rotateX`/`rotateY`, `preserve-3d`) — a flip-card between sign-in and sign-up, and a badge that tilts toward the pointer — with metric cards getting a matching subtle hover tilt. Built to avoid generic "AI dashboard" defaults — no gradient-everything, no all-caps labels, no uniform card kit.

## Tech

Single self-contained `index.html` — no build step, no framework, no backend.

- Vanilla HTML/CSS/JS
- Browser `localStorage` for officer accounts, session, and case persistence (per-device)
- [jsPDF](https://github.com/parallax/jsPDF) (via CDN) for real client-side PDF generation
- Google Fonts (Fraunces, IBM Plex Sans, IBM Plex Mono) loaded via CDN

### A note on downloads

The "Download PDF report" and "Export boundary (GeoJSON)" buttons detect their environment:

- On a normal web host (GitHub Pages, Netlify, your own server, or opened as a local file), they use a standard Blob + anchor-click download — this works in any modern browser.
- If this file is ever embedded inside a Claude.ai artifact viewer, it instead uses that platform's `downloads` capability (`window.claude.use('downloads')`), since the sandboxed iframe blocks the plain anchor-click method.

No configuration needed either way — the code checks for `window.claude` and falls back automatically.

## Running it

Just open `index.html` in a browser. No install, no server required. On first load you'll land on the sign-up screen — create an officer account to get your Officer ID and access the rest of the app.

## Deploying it

Drag `index.html` onto [Netlify Drop](https://app.netlify.com/drop), or push it to a repo and enable GitHub Pages / Vercel. It's a static site — any static host works.

## Known limitations

This is a front-end prototype, not a production system yet:

- Officer accounts and passwords are stored **in plain text in the browser's `localStorage`** — fine for a demo, not secure enough for real deployment. A production build needs a real backend with hashed/salted passwords and proper session tokens
- Officer accounts are **per-device**, same as case data — not shared across officers or synced to a central database
- Satellite imagery analysis is **simulated**, not a live model inference or a real provider API call
- Case data is stored in browser `localStorage` — it's per-device, not shared across officers or synced to a central database
- GeoJSON export uses a fixed placeholder boundary shape regardless of which lease is selected — illustrative, not real cadastral data
- No password reset / account recovery flow

## Where to get real satellite imagery

The app's imagery panels are currently simulated. For a real deployment, two practical paths:

- **Free** — Sentinel-2 (ESA/Copernicus, 10 m resolution, ~5-day revisit) via [Sentinel Hub](https://www.sentinel-hub.com/) or [Google Earth Engine](https://earthengine.google.com/) — good enough for most boundary-change detection, and already referenced as the app's lower-cadence fallback.
- **Paid, near-daily** — [Planet Labs' PlanetScope](https://www.planet.com/products/planet-imagery/) (3 m resolution, daily revisit) via their Subscriptions API — needed if you want the "changed in the last 24h" promise Satellite Watch makes to actually hold.
- **Government-aligned option** — India's [Bhuvan](https://bhuvan.nrsc.gov.in/) (ISRO/NRSC) portal, though its revisit cadence is coarser than the commercial options above.
- For the **basemap** (the underlying map of the whole area, separate from change-detection imagery), Mapbox or the Google Maps Static/Tiles API are the usual choices.

## Roadmap to production

- Backend service polling a real satellite provider (Planet Subscriptions API or Sentinel Hub Statistical API) on a schedule
- Server-side change-detection model, writing results to a shared database
- Real authentication: hashed passwords, session tokens, and officer accounts stored server-side instead of in `localStorage`
- Multi-user auth so officers/supervisors see the same live case data
- Real GIS boundary ingestion (shapefile/GeoJSON upload for lease boundaries)
- Editable/deletable cases, configurable officer roster, password reset

## License

Add your license of choice here (MIT, Apache-2.0, etc.)
