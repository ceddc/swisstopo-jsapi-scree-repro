# swisstopo-jsapi-scree-repro

Minimal ArcGIS JS API repro project for scree `fill-pattern` behavior.

## Files

- `support-repro.html`: single-map repro page with mode switch.
- `styles/v6.json`: latest v5 adaptation style copied as v6 baseline.

## Modes

- `v6 baseline` (default): load `v6.json` as-is.
- `v6 + scree split-4`: inject all upstream scree zoom layers, each split into 4 constant-pattern layers (`weight` 15/10/5/1).
- `v6 + scree OOM`: inject all upstream scree layers (`scree_z11`, `scree_z13`, `scree_z15`, `scree_z17`) with original `match(get("weight"), ...)` patterns.

## Technical analysis (for support)

Observed issue:

- In high-motion navigation scenarios, the data-driven scree pattern path can destabilize rendering and lead to browser/GPU process failure before a clean JS exception.

High-signal render path:

1. `VectorTileLayerView2D.renderChildren` / `_doRender`
2. `TileInfoViewPOT` evaluates scree `fill-pattern` expression (`match(get("weight"), ...)`)
3. `spriteMosaic.bind(...)` pattern atlas binding
4. `submitDrawMeshUntyped(...)` draw submission fanout
5. `mapViewDeps.updateFBOs` + texture upload path (`Texture._texImage`)

Working hypothesis:

- The expression-driven scree pattern rendering path creates high per-frame pressure (pattern resolution/binding + draw submission + render-target churn) during aggressive camera updates.
- Split mode removes this data-driven hot path by converting each scree layer to constant-pattern layers with explicit `weight` filters.
- OOM mode keeps upstream expression semantics to preserve the failing path for vendor debugging.

This repo is designed to isolate that behavior with minimal code and a single map.

## Run locally

From this folder:

```bash
python3 -m http.server 8080
```

Open:

```text
http://localhost:8080/support-repro.html
```

## GitHub Pages

- Workflow: `.github/workflows/pages.yml`
- Root page: `index.html` redirects to `support-repro.html`
- On each push to `main`, GitHub Pages deployment runs automatically.
