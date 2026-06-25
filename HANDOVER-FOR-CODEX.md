# Segedunum — standalone product (handover for CodeX)

**Date:** 2026-06-25 · **For:** CodeX, to push to **git + Cloudflare Pages** · **Owner:** Richard Patterson

## What this is
A self-contained, static product of the **"Stand on the Site" Segedunum fort reconstruction** — the 1 m LiDAR terrain + museum-accurate fort recon at Wallsend. It was lifted **verbatim** out of `STRUCTURES/wall-interactive/site.html` and packaged to stand alone.

- **Entry:** `index.html` → forces `site.html?site=segedunum`.
- **The experience is LOCKED.** `site.html` is a byte-identical copy of the source (verified by SHA256 at extraction). Do **not** edit `site.html` or the Segedunum data. If a change is ever needed, branch a copy — keep this one pristine.
- **Provenance:** source `wall-interactive/site.html`, SHA256 `93b7f0c026edf7ce1a516f077fa20304aa675bcee6acb848e44990eb583fe2a8`.

## Layout
```
segedunum/
├── index.html        entry → site.html?site=segedunum
├── terrain.html      safe redirect for the copied nav's old 3D Landscape tab
├── site.html         the product (LOCKED, verbatim copy)
├── data/
│   ├── sites/
│   │   ├── manifest.json     trimmed to Segedunum only (picker shows just Segedunum)
│   │   ├── fort-outlines.json
│   │   └── segedunum/        1 m height + DSM + drapes (aerial/topo/hillshade) + buildings + reliefs
│   └── terrain/
│       └── wallgis-curtain-bng.geojson   the surveyed Wall line (WallGIS, CC-BY 4.0)
└── HANDOVER-FOR-CODEX.md
```

## Dependencies
- **Three.js 0.160.0 via CDN** (jsdelivr) — needs internet at runtime; nothing to vendor.
- Everything else is local in `data/`. No build step — it is plain static files.

## Run locally
Already wired into `.claude/launch.json` as **`segedunum`** on **http://localhost:8135/** (`python3 -m http.server 8135 --directory <this folder>`). Or from this folder: `python3 -m http.server 8135`.

## Push to git
```bash
cd "STRUCTURES/segedunum"
git init && git add -A && git commit -m "Segedunum — standalone Stand-on-the-Site product"
# create the remote (gh or web), then:
git remote add origin <repo-url> && git push -u origin main
```

## Deploy to Cloudflare Pages
Static site — no framework. Pages/wrangler account is **`10df6e58`** (the wrangler-OAuth one used for Pages).
```bash
cd "STRUCTURES/segedunum"
npx wrangler pages deploy . --project-name segedunum --branch main --commit-message "Segedunum standalone"
```
After deploy, hash-verify a key file against the live URL (per the project's deploy discipline), e.g.:
```bash
shasum -a 256 site.html
curl -sL "https://segedunum.pages.dev/site.html" | shasum -a 256
```
If they differ, the deploy didn't take — re-run wrangler.

## Attribution / rights (already embedded in-page)
- Terrain: **Environment Agency 1 m LiDAR**. Wall line: **WallGIS © Newcastle University, CC-BY 4.0**.
- Drapes: Esri World Imagery (aerial), OpenTopoMap (CC-BY-SA), OS data © Crown copyright.
