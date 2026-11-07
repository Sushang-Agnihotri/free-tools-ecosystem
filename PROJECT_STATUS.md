# Project status

*Last updated: 7 November 2026*

Stages: Research → Architecture → Development → Testing → Packaging → Published → Maintenance

| # | Project | Priority | Stage | Repository | Latest release |
|---|---|---|---|---|---|
| 01 | PlainView Ad Blocker | P0 | **Published** | [plainview-adblock](https://github.com/Sushang-Agnihotri/plainview-adblock) | [v1.0.0](https://github.com/Sushang-Agnihotri/plainview-adblock/releases/tag/v1.0.0) |
| 02 | QuickSkip (YouTube Sponsor Skipper) | P0 | **Published** | [quickskip-youtube](https://github.com/Sushang-Agnihotri/quickskip-youtube) | [v1.0.0](https://github.com/Sushang-Agnihotri/quickskip-youtube/releases/tag/v1.0.0) |
| 03 | Tabwise (Smart Tab Manager) | P1 | **Published** | [tabwise](https://github.com/Sushang-Agnihotri/tabwise) | [v1.0.0](https://github.com/Sushang-Agnihotri/tabwise/releases/tag/v1.0.0) |
| 04 | Universal Clipboard Manager | P1 | Research (next) | — | — |
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
- First automated weekly filter release runs Monday 10 November 2026; watch that it succeeds
- Run the Firefox build in a real Firefox (so far it has only passed Mozilla's linter)
- Optional, needs the owner's accounts: Chrome Web Store listing (one-time US$5 developer fee), Microsoft Edge Add-ons (free), Firefox Add-ons (free, required for a permanent Firefox install)
- Possible later work: `$removeparam` support (would overlap with Link Cleaner), procedural cosmetic filters

### Tests
- Unit: 25/25 passing (`npm test`)
- End-to-end in Chrome 156: 21/21 passing (`npm run test:e2e`). Covers network blocking, on-demand, always-on and site-specific hiding, elements added after load, list exceptions, custom filters, pop-up closing, per-site disable, pause, list toggles, and the popup counter.
- GitHub Actions CI (Ubuntu, latest Chrome): 21/21 end-to-end checks passing
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

---

## 02 · QuickSkip: Sponsor Skipper for YouTube

- **Stage:** Published (7 November 2026)
- **Version:** 1.0.0
- **Tech:** Manifest V3 extension, plain JavaScript, no runtime dependencies. Package size 21 KB.

### Completed features
- SponsorBlock lookups by 4-character SHA-256 hash prefix (k-anonymity), 30-minute cache, graceful handling of 404 and server errors
- Per-category mode: skip automatically / show a button / ignore, across all 10 SponsorBlock categories
- Auto-skip with an Undo notice; chains back-to-back segments; jumps to the end for outros
- Skip button mode; mute-type segments muted; "Jump to highlight" button; whole-video sponsor label
- Coloured markers on the progress bar
- Respects deliberate seeks into a segment; inactive while YouTube ads play
- Handles YouTube's in-page navigation (watch, Shorts, embeds, m.youtube.com, music.youtube.com)
- Popup with the current video's segments and time saved; settings page with an optional SponsorBlock mirror
- Chromium and Firefox builds; CI and release workflows

### Remaining
- Confirm live skipping on youtube.com by hand (YouTube blocks playback in automated browsers)
- Test in real Firefox; store listings (owner's accounts)

### Tests
- Unit: 8/8 passing
- End-to-end (Chrome 156, local HTTPS mock of YouTube plus mock SponsorBlock API): 17/17 passing. Covers hash-prefix privacy, auto-skip timing (no frames played inside the segment), Undo, button mode, statistics, deliberate seeks, ad pause and resume, popup, in-page navigation, missing data, server errors, and the off switch.
- Live YouTube: video detection, API lookup and progress-bar markers confirmed on the real site. Playback is blocked by YouTube in automated browsers.
- Firefox `web-ext lint`: 0 errors, 0 warnings

### Performance
- No polling: event-driven, with a single timer for the next segment boundary
- Test session: about 5 ms of total script time; page JS heap about 4 MB

### Known issues
- Segment coverage depends on SponsorBlock volunteers
- No segment submission or voting (by design; the official SponsorBlock extension does this)

### Links
- Repository: https://github.com/Sushang-Agnihotri/quickskip-youtube
- Latest release: https://github.com/Sushang-Agnihotri/quickskip-youtube/releases/tag/v1.0.0
- Downloads: [Chromium zip](https://github.com/Sushang-Agnihotri/quickskip-youtube/releases/download/v1.0.0/quickskip-youtube-1.0.0-chromium.zip) · [Firefox zip](https://github.com/Sushang-Agnihotri/quickskip-youtube/releases/download/v1.0.0/quickskip-youtube-1.0.0-firefox.zip)
- Last maintenance: 2026-11-07

---

## 03 · Tabwise: Smart Tab Manager

- **Stage:** Published (7 November 2026)
- **Version:** 1.0.0
- **Tech:** Manifest V3 extension, plain JavaScript, no runtime dependencies. Package size 23 KB.

### Completed features
- Tab search across windows with keyboard navigation and a global shortcut (Alt+Shift+T)
- Duplicate detection (hash-insensitive by default), badge count, one-click cleanup keeping the best copy, optional switch-to-existing on open
- Group by site, named and coloured group rules, optional automatic grouping (Chromium only; Firefox lacks the API)
- Idle-tab suspending with native `tabs.discard` (never pinned, audible, active or excluded), plus "Suspend others"
- Sessions: save and restore with pins and groups, lazy restore, autosave (last 5, only when changed), import and export with URL sanitising

### Tests
- Unit: 11/11 passing
- End-to-end with real tabs in Chrome 156: 19/19 passing, stable across repeated runs
- Firefox `web-ext lint`: 0 errors. 11 warnings, all tab-group calls, which are disabled on Firefox.

### Performance
- Event-driven, with two background alarms (idle check at most every 5 minutes; autosave every 10 minutes, writing only on change)
- Service worker heap about 1 MB while active

### Known issues
- Suspended tabs reload on click (unsaved form text may be lost); duplicates are matched by exact URL
- Not yet run in real Firefox

### Links
- Repository: https://github.com/Sushang-Agnihotri/tabwise
- Latest release: https://github.com/Sushang-Agnihotri/tabwise/releases/tag/v1.0.0
- Downloads: [Chromium zip](https://github.com/Sushang-Agnihotri/tabwise/releases/download/v1.0.0/tabwise-1.0.0-chromium.zip) · [Firefox zip](https://github.com/Sushang-Agnihotri/tabwise/releases/download/v1.0.0/tabwise-1.0.0-firefox.zip)
- Last maintenance: 2026-11-07
