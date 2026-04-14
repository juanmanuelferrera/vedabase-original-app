# Vedabase Original — Desktop App

Offline desktop reader for the original, unchanged books of His Divine Grace A.C. Bhaktivedanta Swami Prabhupada. Built with [Tauri](https://tauri.app/).

## What's Included

- 25 books (Bhagavad-gita, Srimad-Bhagavatam cantos 1-10, Sri Caitanya-caritamrta, and 22 more)
- 3,700+ lecture/conversation transcripts
- 6,500+ letters
- Full-text search across all content
- Devanagari script for BG, SB, NOI verses
- Dark mode, font size, justify, serif/sans settings

## Architecture

```
dist/
  index.html          — Single-page app (pure CSS, no frameworks)
  books/*.json         — Book data (generated from R2 HTML + SQLite)
  images/              — Book covers
scripts/
  extract-verses.js    — Extract verses from vedabase-site R2 HTML
  generate-html.js     — Generate book JSON from SQLite DB
  build-db.sh          — Build SQLite from vedabase-site SQL data
src-tauri/
  src/lib.rs           — Tauri entry point (minimal, no backend logic)
  tauri.conf.json      — App config
```

All content is embedded in the app binary — no internet required.

## Prerequisites (build only)

These are needed to build the app, not to run it. The final `.dmg` is a standalone native app with no dependencies.

- [Node.js](https://nodejs.org/) 18+ (for data extraction scripts)
- [Rust](https://rustup.rs/) 1.77+ (for Tauri compilation)
- [Tauri CLI](https://tauri.app/start/) 2.x
- `vedabase-site` repo at `../vedabase-site/` (source of R2 HTML and SQL data)

## Build

```bash
# 1. Build SQLite database from vedabase-site data
./scripts/build-db.sh

# 2. Extract verses from R2 HTML (preserves devanagari, italics, paragraphs)
node scripts/extract-verses.js

# 3. Generate remaining books from SQLite (transcripts, letters, etc.)
node scripts/generate-html.js

# 4. Copy book covers
cp ../vedabase-site/public/images/en-*.jpg dist/images/

# 5. Build release
npm run build
```

Output: `src-tauri/target/release/bundle/dmg/Vedabase Original_0.1.0_aarch64.dmg` (~66 MB)

## Development

```bash
npm run dev
```

## Related Projects

- [vedabase-site](https://github.com/juanmanuelferrera/vedabase-site) — The web version at [vedabase.site](https://vedabase.site)
- [vedabase-original](https://github.com/juanmanuelferrera/vedabase-original) — Corrected markdown source texts
- [vedabase-app](https://github.com/juanmanuelferrera/vedabase-app) — Previous desktop app (v3.0)
