# EventPulse Technical Notes

## Stack & Architecture

- Single-page static app (`index.html`) with embedded CSS/JS.
- PWA support through `manifest.json` and `service-worker.js`.
- QR generation via `qrcode` browser library.

## Security Notes

- ICS text escaping applied to user-provided fields.
- Input validation for required fields and start/end ordering.
- Safe `window.open(..., "noopener,noreferrer")` usage.
- Reminder value bounded to a safe range.
- Baseline CSP configured in HTML head.

## SEO Notes

- Meta description, Open Graph, Twitter cards, canonical URL.
- JSON-LD (`SoftwareApplication`) structured data.
- `robots.txt` and `sitemap.xml` included.

## Caching / Offline

- Service worker uses app-shell caching and runtime caching.
- Cache versioning enables controlled refresh behavior.

## Important Deployment Note

Before production, replace placeholder domain references:

- `https://eventpulse.example.com/`
