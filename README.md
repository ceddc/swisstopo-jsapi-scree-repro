# swisstopo-jsapi-scree-repro

Minimal ArcGIS JS API repro project for scree `fill-pattern` behavior.

## Files

- `support-repro.html`: single-map repro page with mode switch.
- `styles/v6.json`: latest v5 adaptation style copied as v6 baseline.

## Modes

- `v6 baseline` (default): load `v6.json` as-is.
- `v6 + scree split-4`: inject 4 constant-pattern scree layers (`weight` 15/10/5/1).
- `v6 + scree OOM`: inject one data-driven scree layer with `match(get("weight"), ...)`.

## Run locally

From this folder:

```bash
python3 -m http.server 8080
```

Open:

```text
http://localhost:8080/support-repro.html
```
