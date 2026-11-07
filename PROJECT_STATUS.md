# Project status

*Last updated: 7 November 2026*

Stages: Research → Architecture → Development → Testing → Packaging → Published → Maintenance

| # | Project | Priority | Stage | Repository | Latest release |
|---|---|---|---|---|---|
| 01 | PlainView Ad Blocker | P0 | **Published** | [plainview-adblock](https://github.com/Sushang-Agnihotri/plainview-adblock) | [v1.0.0](https://github.com/Sushang-Agnihotri/plainview-adblock/releases/tag/v1.0.0) |
| 02 | YouTube Sponsor Skipper | P0 | Research (next) | — | — |
| 03 | Smart Tab Manager | P1 | Planned | — | — |
| 04 | Universal Clipboard Manager | P1 | Planned | — | — |
| 05 | Bulk File Renamer | P1 | Planned | — | — |
| 06 | Duplicate File Finder | P1 | Planned | — | — |
| 07 | Image Optimizer | P2 | Planned | — | — |
| 08 | PDF Toolkit | P2 | Planned | — | — |
| 09 | Download Organizer | P2 | Planned | — | — |
| 10 | Link Cleaner | P2 | Planned | — | — |
| 11 | Metadata Cleaner | P3 | Planned | — | — |
| 12 | Website Change Monitor | P3 | Planned | — | — |
| 13 | Screenshot Privacy Tool | P3 | Planned | — | — |
| 14 | Bulk Text Formatter | P3 | Planned | — | — |
| 15 | Video & Audio Transcriber | P3 | Planned | — | — |

---

## 01 · PlainView Ad Blocker

- **Stage:** Published (7 November 2026). Now in maintenance: weekly filter-list releases run automatically.
- **Version:** 1.0.0
- **Tech:** Manifest V3 browser extension, plain JavaScript, no runtime dependencies. Build and test tools run on Node.js 20.

### Completed features
- Network blocking via `declarativeNetRequest`: EasyList, EasyPrivacy, EasyList Cookie List and URLhaus hosts compiled to 14,701 rules
- Custom Adblock Plus → DNR converter (under 1% of list rules unsupported; those are skipped, never broadened)
- Element hiding: site-specific rules on page load, generic rules applied on demand (only rules whose class or id is present on the page), plus a small always-on stylesheet
- List exceptions (`#@#`, `$generichide`, `$elemhide`, `$badfilter`) honoured
- Pop-up ad closing for `$popup` domain filters
- Popup: per-site toggle, pause all, per-page blocked count with per-list breakdown, element picker
- Settings: list toggles, disabled-sites manager, custom filters with error report, export and import
- Chromium and Firefox builds, deterministic zip packaging
- GitHub Actions: CI, tagged releases, weekly filter-list releases

### Remaining
- Run the Firefox build in a real Firefox (so far it has only passed Mozilla's linter)
- Optional, needs the owner's accounts: Chrome Web Store listing (one-time US$5 developer fee), Microsoft Edge Add-ons (free), Firefox Add-ons (free, required for a permanent Firefox install)
- Possible later work: `$removeparam` support (would overlap with Link Cleaner), procedural cosmetic filters

### Tests
- Unit: 25/25 passing (`npm test`)
- End-to-end in Chrome 156: 21/21 passing (`npm run test:e2e`). Covers network blocking, on-demand, always-on and site-specific hiding, elements added after load, list exceptions, custom filters, pop-up closing, per-site disable, pause, list toggles, and the popup counter.
- Firefox: `web-ext lint`, 0 errors. Three warnings, all expected: two APIs Firefox lacks (guarded in code), and a coin-miner false positive because the tracker list blocks coin-miner domains by name.
- `npm audit --omit=dev`: 0 vulnerabilities (no runtime dependencies)

### Performance (Chrome 156, Windows 11, local test server)
- Heavy test page (3,000 elements, 40 tracker requests), median load: 44 ms without, 49 ms with PlainView
- Earlier design using one static generic stylesheet cost about +26 ms per page. Replacing it with on-demand hiding cut that to about +5 ms.
- Service worker heap while active: 13.6 MB (the browser unloads the worker when idle)

### Known issues
- Firefox doesn't expose blocked-request counts, so the popup says so instead of showing a number
- Filter lists refresh only when a new version is installed (Manifest V3 limitation). A weekly automated release covers this.
- YouTube in-video ads aren't reliably blocked (documented)

### Links
- Repository: https://github.com/Sushang-Agnihotri/plainview-adblock
- Latest release: https://github.com/Sushang-Agnihotri/plainview-adblock/releases/tag/v1.0.0
- Downloads: [Chromium zip](https://github.com/Sushang-Agnihotri/plainview-adblock/releases/download/v1.0.0/plainview-adblock-1.0.0-chromium.zip) · [Firefox zip](https://github.com/Sushang-Agnihotri/plainview-adblock/releases/download/v1.0.0/plainview-adblock-1.0.0-firefox.zip)
- Last maintenance: 2026-11-07
