# swisstopo-jsapi-scree-repro

Minimal ArcGIS JS API repro project for scree `fill-pattern` behavior.

## Files

- `support-repro.html`: single-map repro page with mode switch.
- `styles/upstream.json`: upstream swisstopo style.
- `styles/v5.json`: adapted v5 style baseline.
- `styles/v6.json`: adapted v6 style baseline.

## Modes

- `1) Default swisstopo upstream (Mapbox GL style)`: load upstream style with all `scree_z*` layers removed.
- `2) Adapted swisstopo style (advanced expressions refactored)`: load adapted v5 style as-is.
- `3) Adapted style + scree relief points`: load adapted v6 style and inject split scree layers (4 weights per scree zoom layer).
- `4) OOM crash version`: load adapted v6 style and inject upstream scree expression layers (`scree_z11`, `scree_z13`, `scree_z15`, `scree_z17`).

### Mode interpretation (support)

- `1` and `2`: the problematic scree fill is not visible.
- `3`: scree is enabled through adapted rendering (split constant-pattern layers).
- `4`: no scree adaptation from the initial style; original scree expression path is active and may lead to browser OOM.

## Scree render JSON (before/after fix)

Before (OOM expression path):

```json
{
  "id": "scree_z11",
  "type": "fill",
  "source": "relief_v1.0.0",
  "source-layer": "scree",
  "paint": {
    "fill-pattern": [
      "match",
      ["get", "weight"],
      15,
      "scree_small_1",
      10,
      "scree_small_2",
      5,
      "scree_small_3",
      1,
      "scree_small_4",
      ""
    ]
  }
}
```

After (split fix path):

```json
[
  {
    "id": "scree_z11_split_w15",
    "filter": ["==", "weight", 15],
    "paint": { "fill-pattern": "scree_small_1" }
  },
  {
    "id": "scree_z11_split_w10",
    "filter": ["==", "weight", 10],
    "paint": { "fill-pattern": "scree_small_2" }
  },
  {
    "id": "scree_z11_split_w5",
    "filter": ["==", "weight", 5],
    "paint": { "fill-pattern": "scree_small_3" }
  },
  {
    "id": "scree_z11_split_w1",
    "filter": ["==", "weight", 1],
    "paint": { "fill-pattern": "scree_small_4" }
  }
]
```

Runtime counts in this repro:

- OOM mode injects 4 scree layers (`scree_z11`, `scree_z13`, `scree_z15`, `scree_z17`).
- Split mode injects 16 scree layers total (4 zoom layers x 4 weights).

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
