+++
author = "Thomas Evensen"
title = "Version 1.9.4"
date = "2026-05-31"
tags = ["changelog","version 1.9.4"]
categories = ["changelog"]
+++

# RawCull Changelog — v1.8.5 → 1.9.4

Updated on [Apple App Store](https://apps.apple.com/no/app/rawcull/id6759362764?mt=12).

## v1.9.1 → v1.9.4

### 🎯 Sharpness scoring now has intent presets and precision modes

Sharpness scoring is now much more configurable and better tuned for different kinds of photography.

- Added photo-type presets: **Auto**, **Birds/Wildlife**, **Portrait**, **Landscape**, and **Action**.
- Added quality modes: **Fast**, **Balanced**, and **High Precision**. Balanced and High Precision decode larger previews and blend more fine detail for better small-subject ranking.
- Added a scoring source picker: **Embedded Preview** for normal culling speed, or **RAW Demosaic** for slow, final-precision checks.
- These scoring choices are now persisted in settings and restored when a catalog is reopened.

---

### 🔍 Focus mask evolved into focus evidence

The focus mask is now much closer to an explanation overlay for the sharpness score, instead of a generic edge mask.

- Focus evidence now chooses between **AF center**, **AF neighborhood**, **saliency/subject region**, or **global detail**, depending on which region best supports the score.
- Sharp images can now **relax the visual threshold** when needed, so the overlay stays visible when RawCull says the image is sharp.
- Aperture-aware blur gating and revised high-ISO handling make wildlife, portrait, and deep-DoF scenes behave more consistently.
- The system now records richer diagnostics such as **winning region**, **rendered region**, **overlay style**, **patch rankings**, **AF distance**, **spatial alignment**, and **confidence reason**.

---

### 📊 Better burst ranking and comparison evidence

Burst ranking is now more nuanced when multiple images in a burst are very close.

- Burst ranking now blends **absolute sharpness** with **burst-relative sharpness** when there is a meaningful spread inside the group.
- Very small sharpness spreads no longer create artificial separation between near-identical frames.
- High-confidence **Keep Best** decisions still require strong absolute sharpness evidence.
- Comparison view now surfaces **subject-sharpness rank** and **subject/global deltas** against the burst winner.
- Candidate Inspector now shows **burst-relative sharpness** alongside **subject**, **global**, and **AF-detail** breakdowns.

---

### 🛠️ Stability and persistence improvements

- Burst analysis cache now invalidates when scoring intent, quality, source, thumbnail size, or related scoring parameters change.
- Settings loading/saving was tightened so persisted scoring settings are not overwritten during initial load.
- Shared memory cache diagnostics and counters were improved and covered by new tests.
- Version metadata advanced to **1.9.4 / build 94**.

---

### 🧪 Tests

- `SharpnessScoringTests` expanded heavily around RAW demosaic selection, focus-evidence region choice, visibility relaxation, patch ranking heuristics, focus-failure classification, aperture hints, ISO ramps, and concurrent scoring.
- `BurstAnalysisTests` added coverage for burst-relative sharpness and comparison summaries.
- New concurrency tests cover cache counters and settings persistence.

## v1.9.0 → v1.9.1

### 🏷️ Burst grid label legend

A legend panel now appears above the grouped burst grid so the burst badges are explained in-place.

- The legend currently documents: **Best frame found**, **Compare first**, **Needs manual review**, **Manual**, **Applied**, **Suggested best**, and **Check frame**.
- The legend is only shown while burst groups are visible in **Similarity Grid**, so it does not clutter the normal grid.

---

### ✅ Batch selection and rating by visible badge

The standard culling grid can now batch-select visible thumbnails by their current badge labels.

- Badge buttons show the visible count for each currently present burst/subject badge.
- Clicking a badge button selects all matching thumbnails; holding **⌘ Command** adds or removes them from the current selection.
- A compact rating picker plus **Apply** button lets you rate the whole selection in one action.
- These controls are hidden while burst groups are displayed.

---

### 🔄 Burst mode now survives view changes

Burst analysis and burst mode are no longer discarded when moving between the main view modes.

- Burst state is preserved when switching between **Loupe**, **Grid**, and **Similarity Grid**.
- Burst-group rendering is now gated by `showsBurstGroups`, so grouped bursts are only shown where they belong: **Similarity Grid**.
- This prevents grouped-burst UI from leaking into unrelated views while still preserving the active burst-analysis state.

---

### 🧹 Cleanup

- Marketing version updated to **1.9.1**.
- Burst display logic was centralized through `showsBurstGroups`.
- Stale internal markdown documents were removed from `documents/`.

---

### 🧪 Tests

- `BurstAnalysisTests` now verify the legend label set and ensure grouped-burst visibility survives view switching correctly.

## v1.8.5 → v1.9.0

### ✋ Manual burst winner override

You can now override the algorithm’s automatic burst winner and pick the keeper yourself.

- **Set Manual Winner** appears in burst comparison.
- Manual winners are clearly labeled in both comparison and grid views.
- Manual overrides are persisted in `savedfiles.json`, survive reloads, and are reattached when bursts regroup.
- **Keep Best** respects the manual winner when one is active.

---

### 🧠 Safer burst culling decisions

Burst culling is now more conservative about when automatic actions are allowed.

- **Keep Best** and **Keep Top 2** are only available when the burst is safe for one-click culling and sharpness scores are present.
- Medium-confidence bursts now push you toward comparing finalists first.
- Low-confidence bursts ask for manual review instead of presenting an unsafe automatic choice.
- Burst keyboard shortcuts now respect the same safety rules.

---

### 🔎 Better burst comparison workflow

Burst comparison gained a much richer inspection flow.

- Added a **Candidate Inspector** side panel with ranking evidence, EXIF summary, candidate reasons/cautions, and rank tables.
- Comparison view can focus on finalists, return to the active burst group, and clearly shows when a **manual winner** is active.
- Comparison navigation and image-loading behavior were refined to make burst review smoother.

---

### 🧾 Clearer burst presentation in the grid

Burst headers and recommendation copy were rewritten to be easier to understand at a glance.

- Burst headers now use clearer decision text such as **Best frame found**, **Compare before deleting**, **Needs manual review**, and **Manual winner: frame X**.
- Human-readable explanations replace raw internal reasoning.
- Thumbnail badges more clearly distinguish suggested frames, manual winners, and applied actions.

---

### 🔄 Burst reanalysis

A new **Reanalyze Bursts** action lets you throw away stale cached analysis and recompute from scratch for the current catalog.

- The action deletes the saved burst-analysis cache for the catalog before rerunning analysis.
- It is disabled while grouping or analysis is already in progress.

---

### 🎭 Focus mask on demand

Focus mask generation is now done only when you actually ask for it.

- Focus masks are generated when the overlay is toggled on, instead of being precomputed for every image.
- In-flight mask work is cancelled if you toggle the overlay off.
- Mask state resets cleanly when switching images.

---

### 🔍 Zoom and inspection improvements

- Zoom now preserves **scale** while navigating between images and recenters the pan offset.
- RAW diagnostics presentation is now driven through `RawCullViewModel.presentRawDiagnostics()`.
- The main window now starts in **detail-only** mode.

---

### 🐛 Fixes & internal improvements

- Version metadata advanced through **1.8.6**, **1.8.7**, **1.8.8**, **1.8.9**, and **1.9.0**, with **build 90** at v1.9.0.
- `BurstAnalysisCache` gained explicit deletion support for reanalysis.
- Burst review states were expanded to track manual overrides and applied actions.
- Logger identifiers were corrected in `RequestThumbnail`.
- Burst presentation helpers were cleaned up for Swift 6.

---

### 🧪 Tests

- `BurstAnalysisTests` expanded for grouping, confidence handling, manual winners, reanalysis, and safe one-click culling.
- New `ComparisonCandidateInspectorTests` cover ranking evidence and EXIF presentation.
- `CullingModelTests` cover manual-winner persistence and pruning.
- `ComparisonGridNavigationTests`, `SharpnessScoringTests`, and `ZoomOverlayKeyActionTests` gained regression coverage.