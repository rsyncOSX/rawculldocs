+++
author = "Thomas Evensen"
title = "Version 2.2.1"
date = "2026-06-27"
tags = ["changelog","version 2.2.1"]
categories = ["changelog"]
+++

# RawCull Changelog — v2.1.8 → 2.2.1

To be submitted for update on [Apple App Store](https://apps.apple.com/no/app/rawcull/id6759362764?mt=12).

 ------------------------------------------------------------------------------------------------------------------------------

### ✨ Added

- Added native parsing and display of rsync itemized output.
- Added categorized output rows for added, updated, deleted, metadata-only, and unknown rsync changes.
- Added support for both rsync and openrsync itemized output formats.
- Added burst-analysis regression documentation in `issues.md`.

### 🧠 Burst Grouping

- Reworked burst-analysis task ownership so cancellation now controls the full analysis pipeline.
- Added generation and catalog checks to prevent old burst-analysis work from publishing stale results.
- Added completed-analysis scope tracking so review-state saves use the same file set that produced the analysis.
- Added a complete burst similarity cache signature including sensitivity, Vision revision, thumbnail size, embedding pipeline version, and grouping config.
- Incremented burst-analysis cache schema to invalidate incompatible older cache files.
- Restored burst review states by exact group membership instead of transient group IDs.
- Excluded singleton groups from burst review queues and review counts while keeping them visible in the grid.
- Simplified burst group headers to focus on opening, reviewing, and deferring groups.
- Simplified burst comparison overlay with a lighter back/inspector hint control.

### ⚡ Performance & Reliability

- Moved burst cache JSON encoding and decoding off `MainActor`.
- Added cancellation checks around cache load/save operations.
- Reworked similarity embedding generation to stay within structured concurrency.
- Prevented cancelled or superseded similarity indexing jobs from committing stale embeddings.
- Added score revision tracking to sharpness scoring.
- Improved culling grid cache invalidation by tracking full group membership, visible file identity, score revision, and max score.

### 🐛 Fixed

- Fixed cancelled burst analysis applying late cache results.
- Fixed catalog cancellation leaving burst-analysis state behind.
- Fixed cache reuse after changing burst sensitivity or embedding configuration.
- Fixed review states moving to the wrong burst after regrouping.
- Fixed singleton images appearing as reviewable burst work.
- Fixed stale culling grid render cache results when middle files, group membership, or sharpness scores changed.
- Fixed review-state persistence overwriting cache snapshots with the wrong analysis scope.
- Fixed main-thread stalls from large burst cache serialization.

### 🧹 Maintenance

- Removed the `RsyncAnalyse` package dependency.
- Updated app version/build settings to `2.2.1` / build `221`.
- Updated README release information for `v2.1.8`.

