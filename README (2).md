# 🛰️ GeoGuard

**Satellite-based illegal mining detection for field officers.**

GeoGuard monitors registered mining lease boundaries using automated satellite passes, flags unauthorized excavation outside approved limits, and takes an officer through the full case lifecycle — from detection to a signed-off PDF report — without needing to visit every site in person.

---

## Why

Field officers responsible for illegal mining enforcement can't physically inspect every lease every day. GeoGuard's **Satellite Watch** answers one question automatically: *"Has anything changed at this site in the last 24 hours?"* — so officers only act on the sites that actually need attention.

## Features

- **Satellite Watch** — per-lease monitoring feed showing last satellite pass, freshness (< 24h highlighted), and change-detection status
- **Dashboard** — live, clickable stats (total/active leases, suspected & high-risk alerts, open cases, affected area) that filter through to the real underlying data
- **Leases registry** — searchable list of all registered mining leases with owner, mineral type, village, khasra number, and lease validity
- **Alerts queue** — suspected boundary violations across the district, risk-tiered (HIGH / MEDIUM / LOW)
- **Change detection workflow** — before/after imagery upload → analysis → boundary overlay → alert → case creation → field inspection → report
- **Case management** — officer assignment, status tracking, audit trail, all persisted locally so progress survives a page refresh
- **GeoJSON export** — download lease + detected-change boundaries for use in GIS tools
- **PDF report export** — generates a clean, print-ready case report via the browser's native print-to-PDF
- Grounded in real reference systems: India's **Mining Surveillance System** (IBM/BISAG), the **MMDR Act, 1957**, and real satellite revisit cadences (PlanetScope ~daily, Sentinel-2 ~5-day)

## Tech

Single self-contained `index.html` — no build step, no framework, no backend.

- Vanilla HTML/CSS/JS
- Browser `localStorage` for case persistence (per-device)
- Google Fonts (Space Grotesk, Inter, Space Mono) loaded via CDN — the only external dependency

## Running it

Just open `index.html` in a browser. No install, no server required.

## Deploying it

Drag `index.html` onto [Netlify Drop](https://app.netlify.com/drop), or push it to a repo and enable GitHub Pages / Vercel. It's a static site — any static host works.

## Known limitations

This is a front-end prototype, not a production system yet:

- Satellite imagery analysis is **simulated**, not a live model inference or a real provider API call
- Case data is stored in browser `localStorage` — it's per-device, not shared across officers or synced to a central database
- No authentication / multi-user support

## Roadmap to production

- Backend service polling a real satellite provider (Planet Subscriptions API or Sentinel Hub Statistical API) on a schedule
- Server-side change-detection model, writing results to a shared database
- Multi-user auth so officers/supervisors see the same live case data
- Real GIS boundary ingestion (shapefile/GeoJSON upload for lease boundaries)

## License

Add your license of choice here (MIT, Apache-2.0, etc.)
