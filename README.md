# Asset library — AI Garage (Paper Cut-Out)

This folder is the channel's **permanent visual identity**: the icon vocabulary and the one recurring character. It is published to GitHub Pages so every render can fetch it by URL.

**Nothing here is regenerated per episode.** If the character changes, the channel changes. Treat this folder as versioned production assets.

---

## 1. What's in it

```
assets/
├── manifest.json          <- the authority on what exists (fetch this first)
├── icons/                 <- 14 icons, .svg + .png
│   ├── microchip.svg / .png
│   ├── cargo_ship.svg / .png
│   ├── tariff_tag.svg / .png
│   ├── inflation_chart.svg / .png
│   ├── currency_stack.svg / .png
│   ├── factory.svg / .png
│   ├── document.svg / .png
│   ├── bank.svg / .png
│   ├── oil_derrick.svg / .png
│   ├── parliament.svg / .png
│   ├── world_map.svg / .png
│   ├── chart_down.svg / .png
│   ├── battery.svg / .png
│   └── people.svg / .png
├── characters/            <- 3 poses of one figure, .svg + .png
│   ├── explaining.svg / .png
│   ├── worried.svg / .png
│   └── pointing.svg / .png
└── paper/
    └── sheet.svg / .png
```

- **SVG** is what the renderer fetches from the hosted library — vector, 512×512, transparent background. It is sharper than a raster at thumbnail scale and the whole library is ~45 KB.
- **PNG** copies (512×512, RGBA, alpha 0 at the corners) stay in the package for offline raster work and the contact sheet. `paper/sheet.png` is the opaque cream background sheet — alpha 255 everywhere, by design.
- The hosted repo can materialise the PNG copies itself via `.github/workflows/deploy-assets.yml`; until that runs, fetch the `.svg`.
- **SVG** is both the editable source and the hosted format. Edit it, re-run the rasteriser, commit both.
- The character is one figure in three poses — mustard circle head, terracotta trapezoid body, navy strip limbs with navy paper fasteners. **Never redraw it in code.** Fetch it.

---

## 2. Hosting

Hosting is documented once, in **`../SETUP-ASSETS.md`** — it covers the repo, GitHub Pages, the three URLs, where to paste them, and troubleshooting. This section only records what this folder contains.

The manifest is the authority on what exists:

```
manifest.json          palette, canvas size, icon_names, character_poses, base_urls
icons/                 14 icons, .svg + .png
characters/            3 poses of one figure, .svg + .png
paper/                 sheet.svg + sheet.png
```

## 3. Extending the library

Add icons **per pocket**, not per episode. When the channel opens a new subject pocket (energy, labour, housing, trade), draw that pocket's icons once and reuse them forever.

### The rules

- **Silhouette first.** An icon must be identifiable in black and white at 64 px. If it isn't, redraw it.
- **One noun per icon.** The icon names the thing. It never carries the argument — that is the counter, the flow strip and the policy sheet's job.
- **Match the existing construction.** Flat paper shapes, cut edges, palette colours only, a subtle inner shadow. Look at `icons/factory.svg` and `icons/factory.png` and copy that level of finish.
- **Never a real logo, emblem, flag or banknote.** Generic stand-ins only.
- **Keep the palette.** Use only the nine colours in `manifest.json → palette`.

### The workflow

```bash
# 1. add the SVG to assets/icons/<name>.svg
# 2. re-render every PNG (regenerates all, so the hand-cut character stays consistent)
python3 make_assets.py
python3 rasterize.py
# 3. rebuild the contact sheet and look at it
#    -> ASSET-SHEET.png
# 4. update the icon list inside make_assets.py, then re-run both scripts
# 5. commit and push -> GitHub Pages rebuilds in ~1 minute
```

`make_assets.py` uses a **seeded** jitter, so all shapes share one hand-cut character across regenerations. Edit an SVG directly only if you are happy to re-run the generator and overwrite it.

### Checking your work

After any change, open `ASSET-SHEET.png` and confirm:
- every icon is legible at thumbnail size;
- no icon looks like a real brand;
- the three character poses read as the same person;
- the palette matches `manifest.json`.

---

## 4. Regenerating everything from scratch

```bash
cd /workspace/output/channel-prompt
python3 make_assets.py     # writes SVG + manifest.json
python3 rasterize.py       # writes PNG via headless Chromium
python3 build_example.py   # writes examples/worked-episode/episode.srt + examples/worked-episode/episode.scenes.json
```

`rasterize.py` needs Chromium via Playwright. It is already working in this workspace.

---

## 5. Licence and attribution

These assets are original work made for this channel. Publish them freely — but do not resell or licence them separately, and do not present them as stock art.
