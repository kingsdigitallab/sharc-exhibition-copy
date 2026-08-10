NOTE: this repo and the documentation below was entirely generated with a coding assistant as an experiment to test abilities to staticise a site.

# site2static

Python tool that scrapes the *Making History: Shakespeare and the Royal Family*
exhibition at <https://sharc.kcl.ac.uk/exhibition/> into a portable, self-contained
static HTML mirror that works fully offline (open via `file://` or a local
`python -m http.server`).

## Usage

```bash
pip install -r requirements.txt
python main.py [url] [output_dir]
```

With no arguments it mirrors the SHARC exhibition into `./html/`.

## What it preserves offline

- All pages, navigation, hero carousel, breadcrumbs, next/related-object links.
- "Find out more ..." contextual-object modals (pure in-page HTML).
- Self-hosted fonts (Open Sans / Raleway) and Font Awesome 5 icons.
- Every IIIF image that the source server will serve (stored locally).
- The IIIF "Guided tour" deep-zoom viewer, rebuilt on a vendored
  OpenSeadragon 2.4.2 with a local high-resolution derivative (pan, zoom,
  fullscreen) instead of the live tile stream.
- Third-party embeds (Sketchfab 3D models, YouTube videos) are kept so they
  work online, with an explanatory placeholder shown offline.

## What needs the network (and why)

- **3D models** (Sketchfab) and **videos** (YouTube) are hosted by third
  parties; the embed is retained and a captioned placeholder notes the
  limitation. See `html/OFFLINE_NOTES.md` after a run.
- IIIF images on `rct.resourcespace.com` are region-locked in some networks;
  any that fail are listed in `html/manifest.json` and left as the original
  URL so they still load when online. Re-run from an unrestricted network to
  fill them in.

## Publishing to GitHub Pages

The `html/` folder is committed to the repo (it is the published artifact).
A workflow at `.github/workflows/pages.yml` publishes it to GitHub Pages
whenever `html/**` changes on `main`:

- In the repo **Settings → Pages → Build and deployment → Source**, select
  **GitHub Actions**.
- Push (or merge) a change under `html/` to `main`. The workflow uploads the
  `html/` folder as a Pages artifact and deploys it. You can also run it
  manually via the Actions tab (**workflow_dispatch**).

To refresh the published mirror, regenerate it locally and commit the
updated `html/` folder:

```bash
pip install -r requirements.txt
python main.py
git add html
git commit -m "docs: refresh html mirror"
git push
```

## License

MIT
