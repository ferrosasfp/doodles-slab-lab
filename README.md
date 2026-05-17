# Doodles Slab Lab

PSA-style 3D graded slab generator for Doodles NFTs. Drop a token ID, get a community-graded slab card with PNG / GIF / GLB / WebM exports.

**Live:** https://doodles-slab-lab.vercel.app

## What it does

Fetches Doodles NFT metadata + image from IPFS (5-gateway race for sub-second loads), renders a PSA-style 1200×1680 trading card with grade, traits and flavor text, then exports the card or a 3D iridescent slab in five formats:

| Export | Format | Typical size | Render time |
|---|---|---|---|
| Card | PNG | ~170 KB | ~310 ms |
| Slab | PNG | ~350 KB | ~315 ms |
| Slab | GIF (30f loop) | ~1.6 MB | ~1.5 s |
| Slab | GLB (3D model) | ~325 KB | ~310 ms |
| Slab | WebM (video) | ~900 KB | ~3 s |

## Stack

- Vanilla HTML / CSS / JS — no build step, no framework
- Three.js loaded via `<script type="importmap">` (ES module CDN imports)
- `gif.js` for GIF encoding (worker fetched as Blob to avoid CORS)
- `MediaRecorder` + `canvas.captureStream()` for WebM
- `GLTFExporter` from `three/addons` for GLB export
- IPFS fetched via `Promise.any` racing 5 gateways (Pinata, dweb.link, nft.storage, w3s.link, ipfs.io)

## Run locally

```bash
python3 -m http.server 8765
# open http://localhost:8765
```

Any static file server works — the app is fully client-side.

## Deploy

Hosted on Vercel with automatic deploys on push to `main`. Manual deploy:

```bash
vercel deploy --prod --yes
```

## Credits

Inspired by [grading.md.codes](https://grading.md.codes/) by md codes (BAYC/MAYC PSA slab tool).
