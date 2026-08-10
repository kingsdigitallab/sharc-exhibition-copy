# Offline mirror notes

This folder is a static, self-contained copy of the *Making History:
Shakespeare and the Royal Family* exhibition at
<https://sharc.kcl.ac.uk/exhibition/>.

## Opening it

- Double-click `index.html` (opens via `file://`), **or**
- Run `python -m http.server` in this folder and open <http://localhost:8000/>.

All internal links and assets use depth-relative paths, so no server is
required.

## What works fully offline

- Every page, the navigation menu, hero carousel, breadcrumbs, and
  next/related-object links.
- The "Find out more about ..." contextual-object modals (pure in-page HTML,
  no network calls).
- All text, layout, CSS, fonts (Open Sans / Raleway), and icons
  (self-hosted Font Awesome 5).
- Images: every IIIF thumbnail, hero image, and object image that could be
- IIIF "Guided tour" deep-zoom viewer: replaced with a local high-resolution
  derivative served to a vendored copy of OpenSeadragon 2.4.2. You still get
  pan, zoom-in/out, and fullscreen. It is single-resolution (limited to the
  downloaded high-res image) rather than pixel-level deep-zoom, because
  mirroring the full tile pyramid for every image is impractical.
  downloaded is stored under `_assets/`.

## What needs an internet connection (and why)

These features are third-party embeds that cannot run from a local copy
because the media/viewer is hosted by a third party. The embed is kept intact
so it works when you are online, and a captioned placeholder explains it.

- **3D models** (Sketchfab `<iframe>`): the model and its WebGL viewer are
  hosted on sketchfab.com. To make these fully offline you would need to
  download the model (e.g. a `.glb` from Sketchfab's download API, which may
  require an account) and embed it with a local viewer such as
  [<model-viewer>](https://modelviewer.dev/).
- **Videos** (YouTube `<iframe>`): the video stream is served by YouTube. To
  make these fully offline you would need to download each video (e.g. with
  `yt-dlp`) and swap the `<iframe>` for a local `<video>` tag.

## Possible gaps

- Some IIIF images on `rct.resourcespace.com` are served only to certain
  regions/networks; if a derivative could not be fetched it is listed in
  `manifest.json` (under `failures`) and the original URL is left in place,
  so it still loads when you are online. Re-running the scraper from a
  network with access to the Royal Collection Trust IIIF server will fill
  these in.
- The `og:image` / Twitter card image for a few object pages points at a
  broken path on the live site itself; these are metadata-only and left as
  the original URL.
- Google Analytics and the cookie-consent banner were intentionally removed
  (no value offline); harmless stubs are injected so the site JavaScript
  initialises cleanly.

See `manifest.json` for the full list of successfully mirrored assets and any
failures.
