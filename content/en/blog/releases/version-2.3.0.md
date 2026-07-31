+++
author = "Thomas Evensen"
title = "Version 2.3.0"
date = "2026-07-19"
tags = ["changelog","version 2.3.0"]
categories = ["changelog"]
+++
 
# RawCull Changelog: v2.3.0 → Latest Commits 

 ------------------------------------------------------------------------------------------------------------------------------
  
## 🚀 Version 2.3.0

Updated  on [Apple App Store](https://apps.apple.com/no/app/rawcull/id6759362764?mt=12).

### 🧠 Photo Analysis

- Migrated focus masks, sharpness scoring, calibration, saliency analysis, and Vision feature prints to PhotoAnalysisKit.
- Added a RawCull adapter supporting embedded previews and demosaiced RAW inputs.
- Focus analysis now incorporates EXIF ISO, aperture, and autofocus-point information.
- Replaced RawCull’s local focus-analysis engine and Metal kernels with the reusable package implementation.
- Updated photo-type presets and quality settings to use PhotoAnalysisKit configurations.

### 🔍 Similarity Analysis

- Migrated similarity embeddings and distance calculations to PhotoAnalysisKit’s Vision feature-print backend.
- Replaced archived Vision observations with portable, Codable feature-print data.
- Updated the embedding pipeline to version 2.
- Continued applying subject-mismatch weighting when ranking visually similar images.

### 💾 Cache Management

- Redesigned Cache Settings with separate **In Memory** and **On Disk** sections.
- Added live cache sizes, item counts, configured limits, available memory, and storage paths.
- Added independent purge controls for:
  - Thumbnail cache
  - Full-size JPEG preview cache
  - Burst analysis cache
- Added confirmation dialogs and progress indicators while clearing caches.
- Added disk-usage reporting and full clearing support for burst-analysis snapshots.

### 📸 Burst Review

- Added an animated progress counter during sharpness scoring and similarity indexing.
- Added estimated time remaining for burst analysis.
- Integrated progress directly into the Burst Home status banner.
- Improved progress accessibility with completed and total counts plus ETA descriptions.
- Moving to the next burst now automatically defers the current group when it has no explicit rating.
- Explicit ratings—including rejected and zero-star ratings—prevent automatic deferral.
- Removed the separate **Set pick** action from the burst workspace.

### 🧪 Testing and Reliability

- Added PhotoAnalysisKit integration coverage.
- Expanded sharpness, calibration, cache-purging, cancellation, and burst-navigation tests.
- Added tests ensuring only genuinely unrated burst groups are automatically deferred.
- Updated the test target for Swift 6 with complete strict-concurrency checking.
- Applied SwiftLint cleanup across the new integration.

### 📚 Documentation

- Reworked the README with updated architecture, dependency responsibilities, analysis pipelines, caching, testing, and build information.
- Added documentation covering the PhotoAnalysisKit integration and future AI architecture.
- Removed obsolete documentation and legacy README images.

### ⚠️ Upgrade Notes

- Existing similarity embeddings use the previous pipeline format and must be regenerated.
- Legacy sharpness-cache signatures remain readable but are treated as stale, causing analysis to be rebuilt.
- The project identifies itself as **2.3.0**, but a `v2.3.0` Git tag has not yet been created.
