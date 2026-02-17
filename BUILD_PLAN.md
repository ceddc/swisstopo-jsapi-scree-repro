# swisstopo-jsapi-scree-repro build plan

## Analysis

- Goal: create a standalone minimal ArcGIS JS API repro project for support with one map and three modes: baseline v6, scree split-4, and scree OOM.
- Scope: scaffold a new git repo in `/home/ced/dev/projects/swisstopo-jsapi-scree-repro`, add `styles/v6.json`, add one minimal HTML repro page, and document run steps.
- Risks: keeping minimal code while preserving exact scree expression behavior; ensuring style and source references remain valid for support reproduction.
- Evidence: user requested a new project with its own git repo and public-ready support repro structure, but asked to pause before push.

## Todo Checklist

1. [done] Initialize standalone project and git repository.
2. [done] Add `styles/v6.json` from latest v5 adaptations baseline.
3. [done] Implement minimal single-map `support-repro.html` with 3 modes.
4. [done] Add concise `README.md` with run and mode instructions.
5. [done] Verify locally that all modes load and switch correctly.
6. [done] Commit locally and stop before push.
