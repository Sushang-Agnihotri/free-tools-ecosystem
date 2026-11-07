# Roadmap

Priorities follow the mission brief (P0 first). Each project ships only after it's built, tested and documented. We don't publish placeholders.

## Now
1. **PlainView Ad Blocker 1.0.0:** published. Next: test in real Firefox and list it in the browser stores.
2. **YouTube Sponsor Skipper (P0):** next build. Plan: Manifest V3 extension, SponsorBlock API using the privacy-preserving hash-prefix lookup (the first 4 characters of the SHA-256 of the video ID, so the server never learns which video you watch), auto-skip or skip-button per category, minimal content script on youtube.com only.

## Next (P1)
3. Smart Tab Manager: browser extension (tabs, tabGroups, sessions APIs)
4. Universal Clipboard Manager: Python + PySide6 desktop app with a local SQLite history
5. Bulk File Renamer: Python + PySide6, preview and an undo journal
6. Duplicate File Finder: Python + PySide6, size → partial hash → full hash pipeline, deletes to the Recycle Bin only

## Later (P2/P3)
Image Optimizer, PDF Toolkit, Download Organizer, Link Cleaner, Metadata Cleaner, Website Change Monitor, Screenshot Privacy Tool, Bulk Text Formatter, Video & Audio Transcriber.

Shared building blocks will appear as these are built: one PySide6 app shell and a PyInstaller portable-build pipeline for the desktop tools, and a shared extension build and test harness for the browser tools.

## How new ideas are evaluated
For every candidate we ask: how many people benefit, how often the problem happens, time saved, whether it runs locally without paid services, maintenance burden, existing open-source alternatives, and what would make ours meaningfully better. If a good open-source tool already exists and we can't clearly improve on it, we link to it instead of building a copy.
