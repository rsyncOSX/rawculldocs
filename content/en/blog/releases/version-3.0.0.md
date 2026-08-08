+++
author = "Thomas Evensen"
title = "Version 3.0.0 beta"
date = "2026-08-08"
tags = ["changelog","version 3.0.0 beta"]
categories = ["changelog"]
+++
 
# RawCull Changelog: v3.0.0 beta → Latest Commits 

The version 3.0.0 beta requiere macOS 27 (Golden Gate) latest beta either public or developer.

These commits are for version 3.0.0 beta and the AI-version of RawCull, the future and next version of RawCull.

A total of **17 commits** were pushed to the `rsyncOSX/RawCull` repository today. The primary focus was on **Version 3.0.0 stabilization**, fixing critical UI/concurrency race conditions, hardening AI release gates, and executing a comprehensive accessibility audit.

---

## 🚀 Features & Enhancements

- **feat: add bounded accessibility semantics** ([`a62f8a9`](https://github.com/rsyncOSX/RawCull/commit/a62f8a9))
  - **What it does:** A comprehensive accessibility pass across the entire app. Introduced `RawCullAccessibilityPresentation.swift` and updated numerous views (Grids, Zoom, Settings, Focus, Saved Files) to ensure proper VoiceOver labels, traits, and hints for assistive technologies.
- **fix: render actual pixels at one-to-one scale** ([`6faaedd`](https://github.com/rsyncOSX/RawCull/commit/6faaedd))
  - **What it does:** Updated `ZoomOverlayView` so that 100% (1:1) zoom now correctly renders actual source pixels rather than scaled display pixels. This is crucial for accurate manual sharpness checking.

## 🐛 Bug Fixes

- **fix: make thumbnail cache identity replacement-safe** ([`fc24ce5`](https://github.com/rsyncOSX/RawCull/commit/fc24ce5))
  - **What it does:** Refactored `ThumbnailCacheKey` and disk/memory cache managers. If a RAW file is replaced or modified on disk, the cache will now correctly invalidate the old thumbnail instead of serving a stale preview.
- **fix: bound thumbnail contention without coupling AI** ([`6a68322`](https://github.com/rsyncOSX/RawCull/commit/6a68322))
  - **What it does:** A major concurrency and performance overhaul. Added a `ThumbnailPreloadGate` and bounded thumbnail generation tasks to prevent CPU/memory contention when loading large catalogs, completely decoupling it from AI processing logic.
- **fix: make histogram loading latest-wins** ([`302a7ff`](https://github.com/rsyncOSX/RawCull/commit/302a7ff))
  - **What it does:** Fixed a race condition in `HistogramView`. Rapidly clicking through photos could cause a slow-loading histogram from a *previous* photo to overwrite the *current* photo's histogram. Now properly implements "latest-wins" task cancellation.
- **chore: minor UI and ViewModel fixes** ([`f76b1ac`](https://github.com/rsyncOSX/RawCull/commit/f76b1ac))
  - **What it does:** A sweep of minor fixes across `MemoryDiagnosticsViewModel`, `ThumbnailCacheKey`, `AIModelDownloadsView`, and various thumbnail/rating UI components to clean up edge cases and state bindings.

## 🧪 Testing & Quality Assurance

- **test: harden AI release gates** ([`a0d17e5`](https://github.com/rsyncOSX/RawCull/commit/a0d17e5))
  - **What it does:** Added `SmokeManifestIntegrityTests` and updated the `Makefile` to strictly enforce that experimental AI features remain gated/blocked in release builds until fully validated.
- **test: verify typed AI persistence matrix** ([`859f160`](https://github.com/rsyncOSX/RawCull/commit/859f160))
  - **What it does:** Added extensive tests (`TypedAIPersistenceMatrixTests`, `AICacheBoundaryTests`) and updated `PerFileAnalysisArtifactStore` to ensure AI cache boundaries and persistence are strictly typed and safe from corruption.
- **test: stabilize thumbnail contention under TSan** ([`c86906b`](https://github.com/rsyncOSX/RawCull/commit/c86906b))
  - **What it does:** Updated `ThumbnailContentionTests` to pass reliably under the Thread Sanitizer (TSan), ensuring the new thumbnail bounding logic is completely free of data races.
- **test: record integrated version 3 regression** ([`28cfdf5`](https://github.com/rsyncOSX/RawCull/commit/28cfdf5))
  - **What it does:** Documented the Phase 9 integrated regression test suite to ensure V3 stability moving forward.

## 📚 Documentation & Release Prep

- **build: prepare blocked version 3 release handoff** ([`53890a3`](https://github.com/rsyncOSX/RawCull/commit/53890a3))
  - **What it does:** Added Phase 10 release handoff documentation and updated the `Makefile` and `README` to prepare for the V3 blocked release.
- **docs: align version 3 release metadata** ([`4f29912`](https://github.com/rsyncOSX/RawCull/commit/4f29912))
  - **What it does:** Added official V3 release notes, updated project metadata, bumped versions in the Xcode project (`project.pbxproj`), and added `ReleaseMetadataTests` to prevent future metadata drift.
- **docs: add Code Review and Issues documentation** ([`ffec0de`](https://github.com/rsyncOSX/RawCull/commit/ffec0de), [`8529b78`](https://github.com/rsyncOSX/RawCull/commit/8529b78))
  - **What it does:** Integrated the comprehensive static deep code review (the one we just translated!) into `Docs/issues.md` and added historical update notes for version 3.0.0 beta.
- **docs: record version 3 stabilization baseline & phases** ([`505d0a7`](https://github.com/rsyncOSX/RawCull/commit/505d0a7), [`b8b4e2e`](https://github.com/rsyncOSX/RawCull/commit/b8b4e2e), [`1ecb75e`](https://github.com/rsyncOSX/RawCull/commit/1ecb75e))
  - **What it does:** Created the foundational documentation for the Version 3.0.0 stabilization effort, including the Phase 0 baseline, V3 update notes, and structural documentation cleanup.
  
