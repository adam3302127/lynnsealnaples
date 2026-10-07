# lynnsealnaples.com — project context for Claude

Website for Lynn Seal, PA, REALTOR® (Downing-Frye Realty, Inc., Naples FL). Adam Wockston (her son) owns the repo and the Cloudflare domain account; Lynn is the registrant and the audience.

## How the site works
- Static HTML, no framework. `node build.js` renders `listings/<slug>/`, `neighborhoods/treviso-bay/`, the three generated blocks in `index.html` (between `<!-- build:listings|sold|communities -->` markers), `sitemap.xml` and `robots.txt` from `data/*.json` + `templates/`.
- `node build.js --inline dist-preview` writes inlined copies for the Claude artifact preview (https://claude.ai/artifact/NT36qzd2WT5Hv4chBpPcgo). `dist-preview/` is gitignored.
- Shared CSS/JS: `assets/tokens.css` (design tokens, light+dark), `assets/site.css`, `assets/site.js`. Listing pages add `assets/listing.css` + `assets/listing.js`. The home page must load `assets/site.js` (it once carried a stale inline copy; never reintroduce inline page scripts).
- Images: run `python3 tools/images.py` after adding JPEGs; it writes `.webp` and `-480.webp` next to each. Listing photos live in `assets/listings/<slug>/NN.jpg` (page listings) or `assets/listings/<slug>.jpg` (sold flyers).
- Deploy: push to `main` → `.github/workflows/pages.yml` → GitHub Pages → https://lynnsealnaples.com. Custom domain and Enforce HTTPS are set in repo Settings → Pages (only the repo owner can change them; the API is blocked for Claude).
- DNS: Cloudflare (Adam's personal account), DNS-only records: four A records to GitHub Pages IPs + CNAME www → adam3302127.github.io.

## Rules that came from Adam
- Fair Housing: describe the property, never the buyer ("relocating buyers", not "families").
- No stock photos presented as her listings; any virtually staged image carries a visible label (`"label"` field on the photo).
- No IDX/MLS feed without an explicit go-ahead (costs money; options in LAUNCH-GUIDE.md).
- Palette is white + navy + brass, "old school". Header stays a short horizontal band on phone and desktop. Don't restyle without being asked.
- Reel on the home page is self-hosted (`assets/lynn-reel.mp4` + `.webm`); no video slot on listing pages (removed on request).
- Contact-section photo is downtown Naples at dusk, not a beach. Pier photos: sunset band + "Your Home?" card.
- Every inner page has a "Back to all listings" / "Back to home" bar under the nav.

## Facts to verify with Lynn before leaning on them
- "Downing-Frye Top Producer" tier (Platinum?) and "Treviso Bay's Top-Selling Agent" wording.
- Which phone number is public: (810) 691-6829 is used site-wide.

## Gotchas
- Zillow serves listing photos at most 1024px (`-cc_ft_1536` returns 1024); Redfin `mbphotov3/genMid` gives ~1080px; Downing-Frye sold pages serve one 1024px photo. Lynn's own headshot is still web-resolution — ask her for originals.
- Wikimedia Commons rate-limits aggressively (429); fetch thumbnails at listed sizes with 20s+ pauses, or scrape the File: page via Firecrawl for license/author.
- The repo's Playwright Chromium has no H.264 decoder; test video with the WebM source. Launch with `--headless=new` and `ignoreDefaultArgs: ['--headless=old','--headless']`.
- FormSubmit (`formsubmit.co/ajax/lynnsealnaples@gmail.com`) needs a one-time activation click by Lynn after the first submission.
