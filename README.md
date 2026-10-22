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
| `brand/` | The official Gstore artwork: `logo-512.png` and `logo-256.png` for display, `apple-touch-icon.png` at 180px, `favicon-32.png` for the tab. |

Plain static HTML and CSS. No framework, no build step, no npm dependencies,
no external fonts, CDNs or trackers. The type is a system font stack, so the
only assets the page fetches are its own stylesheet and the logo. The one
third-party request is the download counter calling GitHub's public releases
API (see below); nothing about the visitor is sent with it.

The logo is used as supplied. It already carries its own corner radius,
shading and halo, so do not mask it, add a border or put a drop shadow
behind it, and do not display it above 512px, which is the largest size
in the folder. The page shows it at 26px in the header and 96px in the
hero, against the page background rather than on a flat white panel. The
blue, cyan and green sampled from it live in `styles.css` as `--grad-a`,
`--grad-m` and `--grad-b`.

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

Nothing on the page needs editing. The three download buttons point at
`/releases/latest/download/<file>` with version-less filenames
(`Gstore-macos-arm64.dmg`, `Gstore-macos-intel.dmg`,
`Gstore-windows-x64.zip`), so they always resolve to the newest release
as long as the release assets keep those names. Attach the assets under
those exact filenames and the page follows along.

No prose names a version number any more, so nothing goes stale. The
anchors deliberately carry no `download` attribute: it does nothing
cross-origin, and GitHub already sends the right `Content-Disposition`
header.

## The download counter

The line under the Download heading is GitHub's own figure, fetched at page
load from `GET /repos/Parthivsurya/Gstorage/releases` and summed over the
`download_count` of every asset of every release. So it is a running total
across versions, it counts downloads rather than unique people, and it only
moves when someone pulls a release asset — cloning the repo or building from
source does not register.

Three things to know:

- It is purely additive. The element ships `hidden`, and the script reveals it
  only after a successful response, so with JavaScript off, GitHub down, or
  the rate limit hit, the page looks exactly as it did before.
- The endpoint needs no key but allows 60 calls an hour per IP. Each visitor
  spends their own budget, and the answer is cached in `localStorage` for an
  hour, so a repeat visit makes no request at all. A stale cached number is
  shown if a later fetch fails.
- A zero total stays hidden, so the line does not appear before the first
  release has any downloads.

If you would rather the page make no third-party request at all, delete the
second block in the `<script>` at the bottom of `index.html` along with the
`p.dl__count` markup; the counter is self-contained.

## Checks before deploying

The page is hand validated: HTML tag balance, every in-page anchor resolving
to a real `id`, no reference to a file that does not exist, and the layout
holding at 375px wide. Worth re-running by eye after any edit, in both
color schemes.
