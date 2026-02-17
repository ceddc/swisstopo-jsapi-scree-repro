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

## 2026-02-17 - mode list rework for support

### Analysis

- Goal: update the repro mode list to exactly four support modes: upstream-no-scree, adapted v5, adapted v6 with scree, and oom.
- Scope: update `support-repro.html`, add style sources needed by those modes (`styles/v5.json`, `styles/upstream.json`), and refresh README mode documentation.
- Risks: mode builders could accidentally reuse old scree injection behavior and make two modes visually identical.
- Evidence: user asked for a specific 4-item mode list and raised that baseline vs split looked identical.

### Todo Checklist

1. [done] Add required style files for new modes.
2. [done] Rework mode selector and style builders in `support-repro.html`.
3. [done] Update `README.md` to describe the new 4-mode list.
4. [done] Smoke-test all 4 modes and confirm scree layer counts.

## 2026-02-17 - mode wording + scree JSON examples

### Analysis

- Goal: update mode labels/descriptions with support-friendly wording and add before/after scree render JSON snippets in README.
- Scope: text-only changes in `support-repro.html` and `README.md`; no behavior changes.
- Risks: wording drift between UI and README.
- Evidence: user asked for text updates, requested JSON render examples, and explicitly asked to avoid mentioning poor ArcGIS rendering.

### Todo Checklist

1. [done] Persist this plan section before edits.
2. [done] Update mode labels in `support-repro.html` (text only).
3. [done] Update README mode wording and add scree before/after JSON snippets.
4. [done] Verify labels and README content consistency.

## 2026-02-17 - support guidance wording update

### Analysis

- Goal: add clearer support-oriented wording that explains what each of the 4 modes demonstrates, including when the problematic scree fill is hidden vs active.
- Scope: update help text in `support-repro.html` and add matching documentation notes in `README.md`.
- Risks: mismatch between UI help text and README wording.
- Evidence: user asked for better-worded explanation in documentation/help panel and specified the expected 1-4 interpretation.

### Todo Checklist

1. [done] Persist this plan section before edits.
2. [done] Add concise in-page help panel text for modes 1-4.
3. [done] Add matching mode interpretation section in README.
4. [done] Verify help text appears and wording stays consistent.
