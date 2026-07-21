# BanG Dream! Screenshot Search

A local Raycast extension for searching and copying screenshots from BanG Dream! anime series.

The first version includes **MyGO!!!!!** and **Ave Mujica**. Screenshot metadata and remote image URLs are generated from the public [Ave Mujica Screenshot Search repository](https://github.com/serser322/ave-mujica-images), which powers [ave-mujica-images.pages.dev](https://ave-mujica-images.pages.dev/).

## Features

- Search Traditional Chinese dialogue across all included series.
- Filter by series, favorites, or recently used screenshots.
- Browse the full catalog in paginated batches.
- Press `Enter` to download the source JPG to one temporary file and copy it to the macOS clipboard.
- Keep favorite IDs and the 30 most recently copied IDs in Raycast LocalStorage.

The extension does not bundle, persist, or cache upstream screenshots. Raycast and macOS may maintain their own network or clipboard caches.

## Local Development

Requirements:

- macOS with Raycast installed
- Node.js 22.22.2 through `nvm`

```bash
nvm use
npm install
npm run dev
```

Validation commands:

```bash
npm test
npm run lint
npm run build
```

## Updating the Catalog

Run the following command when the upstream project adds or changes screenshots:

```bash
npm run sync-data
```

The sync script resolves the upstream `main` commit, parses its TypeScript metadata, verifies that every entry has matching WebP and JPG assets, and writes a deterministic manifest pinned to that commit. To reproduce a specific version:

```bash
node scripts/sync-data.mjs --commit <40-character-sha>
```

The generated manifest contains metadata and remote URLs only. It does not contain image bytes.

## Local Data

Raycast LocalStorage contains only JSON arrays under these keys:

- `favorites.v1`
- `recent.v1`

The clipboard image uses a content-addressed temporary path:

```text
/tmp/bang-dream-screenshot-search/clipboard-<content-hash>.jpg
```

Changing the path with the image content prevents apps such as LINE from reusing a stale file-path cache. After a successful copy, older clipboard images are removed, so the directory retains only the current JPG.

## Attribution and Rights

See [NOTICE.md](NOTICE.md). The MIT license in this repository covers this extension's source code only. It does not grant rights to BanG Dream! names, logos, characters, animation footage, or screenshots.
