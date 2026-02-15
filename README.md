# EventPulse — Calendar Invite + QR Generator

EventPulse is a redesigned static web app that helps users create beautiful, shareable event invites.

## Features

- Modern, responsive UI optimized for desktop and mobile.
- One-click `.ics` file generation.
- One-click Google Calendar link generation.
- QR code generation for event links (now includes **title, time, location, and description**).
- Auto-end-time logic: when users choose a start time, end time defaults to **+1 hour**.
- Web Share support for mobile-friendly sharing.
- Live event preview panel.
- PWA basics (`manifest.json` + `service-worker.js`).
- SEO improvements (`meta` tags, Open Graph, Twitter tags, JSON-LD, `robots.txt`, `sitemap.xml`).

## Project structure

- `index.html` — Main app UI, styling, and logic.
- `service-worker.js` — Cache-first shell plus runtime caching.
- `manifest.json` — PWA metadata.
- `robots.txt` — Search crawler rules.
- `sitemap.xml` — Sitemap entry.

## Run locally

This is a static app, so any static server works.

```bash
python3 -m http.server 4173
```

Then open:

```text
http://localhost:4173
```

## Security audit summary

### Risks found in original implementation

1. **ICS injection risk**
   - User-supplied text was inserted into ICS fields without escaping control characters used by the ICS format.
2. **Weak input validation**
   - Missing checks for invalid or reversed date ranges.
3. **Potential unsafe URL handling**
   - Some interactions did not enforce safer window opening behavior.
4. **Unbounded reminder input**
   - No practical max guard on reminder minutes.

### Mitigations implemented

1. **ICS escaping implemented**
   - Added text escaping for backslashes, commas, semicolons, and newlines before writing `SUMMARY`, `DESCRIPTION`, and `LOCATION`.
2. **Validation enforcement**
   - Added explicit checks for required fields and start/end ordering.
3. **Safer tab opening**
   - `window.open` now uses `noopener,noreferrer`.
4. **Bounded numeric input**
   - Reminder clamped to `0..40320` minutes.
5. **Reduced DOM risk**
   - User-facing previews use `textContent`.

## SEO checklist implemented

- Unique title and meta description.
- Keywords, robots, canonical tags.
- Open Graph + Twitter tags.
- Structured data (JSON-LD `SoftwareApplication`).
- `robots.txt` and `sitemap.xml`.

## Notes

- QR code generation uses `qrcode` from jsDelivr.
- Service worker enables offline shell caching after first load.


## Additional hardening patches

- Added a baseline Content Security Policy in `index.html` to reduce script/object/frame attack surface.
- Replaced remote icon/preview assets with local SVG assets under `assets/` to reduce third-party dependency/privacy leakage.
- Tightened service worker app-shell cache scope (removed unnecessary README caching).
- Switched canonical and sitemap URLs to absolute production-style URLs.

> **Important:** replace `https://eventpulse.example.com` with your real deployed domain before production release.
