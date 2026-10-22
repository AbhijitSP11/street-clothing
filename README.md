# Street clothing storefronts

Two self-contained storefront pages (product grid, product sheet, cart drawer, checkout) built as static HTML. No build step, no dependencies.

| Path | Page |
|---|---|
| `/` | Landing page linking to both stores (`index.html`) |
| `/meridian` | Meridian Apparel (`meridian.html`, uses `img/`) |
| `/sidequest` | Sidequest Streetwear (`sidequest.html`) |

```
.
├── index.html        landing page
├── meridian.html     store 1
├── sidequest.html    store 2
├── 404.html          shown for unknown URLs
├── img/              product photography used by meridian.html
└── vercel.json       Vercel config (clean URLs, security + cache headers)
```

## Deploy to Vercel

This is a plain static site, so the project root *is* the output.

**Option A: from GitHub (recommended)**
1. Push this repo, then in Vercel choose **Add New → Project** and import it.
2. Framework Preset: **Other**. Leave **Build Command**, **Output Directory** and **Install Command** empty.
3. Deploy. Every push to `main` redeploys; other branches get preview URLs.

**Option B: from the CLI**
```bash
npx vercel          # first run: log in, link the project, creates a preview deployment
npx vercel --prod   # production deployment
```

## Preview locally

```bash
npx serve .         # clean URLs on by default (/meridian, /sidequest)
# or, with no clean URLs, open http://localhost:PORT/meridian.html
python -m http.server 4173
```

## Notes

- `vercel.json` turns on `cleanUrls` (`/meridian.html` redirects to `/meridian`) and `trailingSlash: false`. The second one matters: `meridian.html` loads images with relative paths (`img/...`), which only resolve correctly when the page is served at `/meridian`, not `/meridian/`.
- Fonts load from Google Fonts. The pages need internet access for their typefaces (they fall back to system fonts offline).
- Images are cached for a day (`/img/*`). If you replace a photo under the same file name, visitors may see the old one for up to 24 hours; rename the file or lower `max-age` in `vercel.json` to avoid that.
- The photos total about 2.8 MB (the hero is 3840 px wide). Compressing them (for example to WebP/AVIF at around 1600 px for the hero) would speed up first load.
- No Content-Security-Policy is set because the pages use inline `<style>`/`<script>` and Google Fonts. Add one if you move to external files.
