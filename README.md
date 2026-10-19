# Gstore landing page

The marketing page for **Gstore**, a local-only desktop app that pools the
free storage across your own Google accounts into one drive.

- App and source: https://github.com/Parthivsurya/Gstorage
- Releases (what the download links point at): https://github.com/Parthivsurya/Gstorage/releases/latest

## What is in here

| File | Purpose |
| --- | --- |
| `index.html` | The whole page. One file, including the small platform-detect script at the bottom. |
| `styles.css` | Design tokens and layout. Light and dark come from `prefers-color-scheme`. |
| `favicon.svg` | The Gstore mark: rounded square, blue to green gradient, white G. |

Plain static HTML and CSS. No framework, no build step, no npm dependencies,
no external fonts, CDNs or trackers. The type is a system font stack, so
nothing is fetched at runtime.

## Run it locally

Open the file:

```sh
open index.html          # macOS
```

Everything is relative, so the `file://` URL works. If you want a server
(for correct caching behavior or to test on a phone on the same network):

```sh
python3 -m http.server 8080
# then visit http://localhost:8080
```

## Deploy

Vercel, as a static site with zero configuration.

1. Import the repo at https://vercel.com/new.
2. Framework preset: **Other**. Build command: none. Output directory: the
   repo root.
3. Deploy. Vercel serves `index.html` at `/`.

There is no `vercel.json`, because nothing needs configuring. Pushing to
`main` redeploys.

## When Gstore ships a new version

Two things in `index.html` carry the version number, and both are plain text:

1. The line under the Download heading ("Version 0.0.2...").
2. The three asset filenames, for example `Gstore-0.0.2-arm64.dmg`.

The links themselves point at `/releases/latest`, so they never need
updating. The filenames are shown only so a visitor knows which asset to
pick on the GitHub release page, and which of the two macOS builds matches
their chip.

## Checks before deploying

The page is hand validated: HTML tag balance, every in-page anchor resolving
to a real `id`, no reference to a file that does not exist, and the layout
holding at 375px wide. Worth re-running by eye after any edit, in both
color schemes.
