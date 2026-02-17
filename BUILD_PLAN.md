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

## 2026-02-17 - scree parity correction for support repro

### Analysis

- Goal: make split/oom modes technically accurate and visibly distinct by using the full upstream scree set (`scree_z11`, `scree_z13`, `scree_z15`, `scree_z17`).
- Scope: update only `support-repro.html` to inject all scree layers in OOM mode and split each scree layer by weight in split mode.
- Risks: adding too much logic could reduce clarity; keep helpers short and explicit.
- Evidence: user reported no visible baseline/split difference and asked whether all scree layers are present in OOM mode.

### Todo Checklist

1. [done] Persist this plan update before editing code.
2. [done] Update repro mode builders to use all upstream scree layer definitions.
3. [done] Show mode status with scree layer counts/ids for quick verification.
4. [done] Re-run smoke checks for baseline, split, and oom.

## 2026-02-17 - support README + GitHub Pages publish

### Analysis

- Goal: add a clear technical summary of the OOM pattern-rendering issue in `README.md`, then publish this repo publicly with an automated GitHub Pages workflow.
- Scope: update docs, add a minimal Pages workflow and index entrypoint, verify locally, commit, create public GitHub repo, and push.
- Risks: incomplete Pages setup can publish a blank root path; missing permissions in workflow can block deployment.
- Evidence: user asked for README technical analysis first, then a simple GitHub Pages publication workflow and public GitHub push.

### Todo Checklist

1. [done] Persist this plan section before implementation.
2. [done] Add technical OOM pattern-rendering analysis section to `README.md`.
3. [done] Add minimal GitHub Pages workflow and root entrypoint.
4. [done] Verify local serving still works for root and support page.
5. [done] Commit changes, create public GitHub repo, and push (no extra features).

## 2026-02-17 - GitHub Pages enablement fix

### Analysis

- Goal: fix failed first Pages deployment by enabling Pages site initialization in workflow.
- Scope: update `.github/workflows/pages.yml` only.
- Risks: none beyond rerun latency.
- Evidence: workflow failed at `actions/configure-pages` with "Get Pages site failed ... repository has Pages enabled".

### Todo Checklist

1. [done] Add `enablement: true` to `actions/configure-pages` step.
2. [done] Push fix and verify next Pages run passes.
