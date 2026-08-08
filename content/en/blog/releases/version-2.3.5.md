+++
author = "Thomas Evensen"
title = "Version 2.3.5"
date = "2026-08-07"
tags = ["changelog","version 2.3.5"]
categories = ["changelog"]
+++
 
# RawCull Changelog: v2.3.5 → Latest Commits 

Updated on [Apple App Store](https://apps.apple.com/no/app/rawcull/id6759362764?mt=12).

*Important*: This release is the final version for *macOS 26*. The next version, 3.0.0, will introduce AI features exclusively available on macOS 27 upon its release. Version 2.3.5 will remain available on GitHub for an extended period following the release of macOS 27. The Apple App Store version will only support macOS 27 once version 3.0.0 is released.

<div class="alert alert-secondary" role="alert">

Version 2.3.5 on the Apple App Store is built and uploaded to App Store Connect for release on the App Store by utilizing Xcode Cloud. I have installed macOS 27 developer beta on both my Macs and cannot build and upload a release version directly from Xcode 27 beta. The version 2.3.5 on GitHub is built by Xcode 27 beta, and if you have some doubts about it, please install this version from the Apple App Store.

</div>

 ------------------------------------------------------------------------------------------------------------------------------
  
  # 📸 RawCull 2.3.5 Changelog

RawCull 2.3.5 is a maintenance and stabilization update for macOS 26. It improves Actual Pixels inspection, histogram reliability, thumbnail-cache correctness, catalog-loading behavior, persistence testing, accessibility, and release validation.

This release introduces no new AI requirements.

## ✨ Highlights

- Corrected Actual Pixels inspection to provide genuine 1:1 image inspection.
- Removed a possible histogram conversion crash.
- Prevented older histogram calculations from replacing newer results.
- Added source-aware and size-aware thumbnail-cache identities.
- Prevented thumbnail grids from competing with catalog preloading.
- Expanded similarity-artifact persistence and cancellation coverage.
- Improved accessibility throughout saved files and culling controls.
- Strengthened smoke, Thread Sanitizer, stress, and Release build gates.

## 🔍 Actual Pixels Inspection

- Defined Actual Pixels as one source-image pixel per display point before backing-scale conversion.
- Removed the previous additional 60% scaling factor.
- Changed the fit-relative scale calculation to `1.0 / fitScale`.
- Added validation for:
  - Non-positive image and viewport dimensions.
  - Non-finite dimensions and scale values.
  - Non-finite autofocus coordinates.
  - Missing autofocus metadata.
- Invalid geometry now produces a finite, centered fallback instead of an invalid transform.
- Added focused coverage for:
  - Landscape and portrait images.
  - Fit-upscaled images.
  - Mismatched aspect ratios.
  - All four clamped viewport edges.
  - Missing and invalid focus coordinates.
  - Invalid image and viewport dimensions.
- Preserved the existing `Z` keyboard shortcut and Actual Pixels terminology.

## 📊 Histogram Reliability

- Replaced separate histogram-loading paths with one task lifecycle tied to the selected image.
- Histogram bins are cleared immediately when:
  - The selected image changes.
  - The selected image becomes unavailable.
  - Image conversion fails.
- Removed the reachable `fatalError` from histogram image conversion.
- Conversion failures are now logged as recoverable display errors.
- Histogram calculation runs through structured concurrent work.
- Added generation checking so an older calculation cannot replace results for a newer image.
- Added cancellation checks before publishing histogram data.
- Improved `FileInspectorView` image handling:
  - Image state is now private.
  - Stale images are cleared when file identity changes.
  - Cancelled thumbnail requests cannot publish late results.
- Added deterministic tests for:
  - Nil-image clearing.
  - Conversion failure.
  - Successful histogram generation.
  - Slow-old-image versus fast-new-image supersession.

## 🖼️ Thumbnail Cache Correctness

- Introduced schema-v3 thumbnail request identities.
- Thumbnail identities now include:
  - Standardized source path.
  - Source file size.
  - Source modification time.
  - Thumbnail purpose.
  - Requested maximum pixel size.
  - Thumbnail-cache schema version.
- Separated grid thumbnails from preview thumbnails.
- A small grid thumbnail can no longer satisfy a larger preview request.
- Cached images must have sufficient decoded dimensions for the request.
- Applied the same complete identity across:
  - Disk cache.
  - Memory cache.
  - Catalog scanning.
  - Grid loading.
  - Preview loading.
  - Comparison loading.
  - Zoom-preview paths.
- URL-only requests read source metadata before attempting reuse.
- Requests without reliable source metadata bypass cache reuse instead of accepting potentially stale path-only entries.
- Replacing a file at the same path now produces a different thumbnail identity.
- Retained atomic JPEG writes.
- Cancelled writes no longer leave incomplete cache artifacts.
- Old schema-v2 cache files remain removable but cannot be loaded as schema-v3 results.
- Cache clearing removes both old and current thumbnail schemas.
- Added coverage for:
  - Stable request keys.
  - Same-path source replacement.
  - Missing source metadata.
  - Grid and preview separation.
  - Decoded-size suitability.
  - Old-schema rejection.
  - Cross-schema clearing.
  - Cancellation during disk writes.
  - Corrupt cached JPEG recovery.

## ⚡ Catalog Loading and Grid Contention

- Added a low-risk catalog-preload grid gate.
- Normal, rated, and similarity thumbnail grids are not constructed while the selected catalog is actively preloading.
- This prevents grid requests from competing with the batch thumbnail preload for the same catalog.
- The grid automatically becomes available when preload:
  - Completes.
  - Is cancelled.
  - Is superseded.
  - No longer belongs to the selected catalog.
- Catalog progress and cancellation controls remain available while the grid is gated.
- Removed the obsolete grid-contention TODO from `ThumbnailLoader`.
- Added process-wide thumbnail extraction diagnostics for:
  - Extraction starts.
  - Extraction completions.
  - Cancellations.
  - Concurrent duplicate starts.
  - Coalesced waiters.
  - Currently active work.
  - Peak active work.
- Exposed the new counters in Memory Diagnostics and exported TSV reports.
- Added deterministic tests for active, inactive, mismatched, cancelled, and superseded catalog states.

## 💾 Similarity Persistence Verification

- Expanded regression coverage for per-file similarity artifacts.
- Added verification that a persistence write failure:
  - Keeps successfully generated session results available in memory.
  - Leaves the failure diagnosable.
- Added deterministic partial-commit cancellation coverage:
  - Already committed records remain valid.
  - Later records are not committed after cancellation.
- Verified that clearing analysis artifacts does not modify:
  - Ratings.
  - Settings.
  - Independent user-data files.
- Verified progress and phase state reset after:
  - Successful indexing.
  - Partial generation failure.
  - Persistence failure.
  - Cancellation.
  - Superseded indexing.
- Added an actor-isolated test hook for deterministic partial-commit cancellation.
- The hook is disabled in production and does not change production commit ordering.
- Added explicit proof that thumbnail purpose, size, and schema changes do not alter similarity-artifact identity.
- Existing coverage continues to verify:
  - Artifact round trips.
  - Corrupt-record isolation.
  - Incompatible-record rejection.
  - Pruning and clearing.
  - Changed and added sources.
  - Legacy migration.
  - Partial generation failure.
  - Structured cancellation.
  - Latest-generation-wins behavior.

## ♿ Accessibility and Interaction

- Converted saved catalog and file-record rows from gesture-only containers to native plain buttons.
- Preserved:
  - Row-wide click targets.
  - Hover rendering.
  - Selection behavior.
  - Split-view navigation.
- Added explicit accessibility names, values, selected traits, and actions to:
  - Normal thumbnails.
  - Rated thumbnails.
  - Comparison thumbnails.
  - Rating and reject controls.
  - Image-source controls.
  - Focus-mask controls.
  - Focus-point controls.
  - Burst-review controls.
- Added selected accessibility traits to reviewed and deferred burst controls.
- Added named actions for selecting and opening images.
- Added accessible file and rating descriptions.
- Hid decorative selection marks, rating strips, glyphs, and dividers when they duplicated spoken information.
- Preserved intentional single-click and double-click behavior in:
  - Normal image tiles.
  - Rated image tiles.
  - Comparison panes.
  - File details.
  - Zoom overlays.

## 🧪 Test and Release Gates

- Made `Smoke.xctestplan` the sole smoke-test selector.
- Smoke membership is now controlled by Swift Testing’s `.smoke` tag.
- Removed the duplicate Makefile smoke-suite allow-list.
- New smoke tests only need the `.smoke` tag; the Makefile no longer needs to be edited.
- Proved that a deliberately failing smoke-tagged test makes the smoke command fail.
- Removed all temporary failure-proof code after validation.
- Kept smoke tests parallel for fast feedback.
- Serialized the complete Thread Sanitizer plan because it deliberately exercises process-wide cache, settings, and singleton state.
- Replaced a scheduler-speed-dependent persistence test with deterministic actor synchronization.
- Clarified that the performance command is an extreme-concurrency stress gate rather than a timing benchmark.
- Updated the README and test architecture documentation to describe:
  - Smoke-test selection.
  - Full Thread Sanitizer coverage.
  - Dedicated stress testing.
  - Test isolation expectations.

## ✅ Automated Validation Results

The final automated validation completed successfully:

- Smoke tests:
  - 93 unique tests.
  - 101 concrete parameterized cases.
- Full Thread Sanitizer suite:
  - 270 unique tests.
  - 295 concrete parameterized cases.
  - No Thread Sanitizer diagnostics.
- Extreme-concurrency stress gate:
  - 1 selected test passed.
- Exact-`Package.resolved` Release build:
  - arm64 build passed.
  - No new RawCull compiler or concurrency warnings.
- The only observed build warning was the existing App Intents metadata notice caused by the absence of an `AppIntents.framework` dependency.

## 🧰 Version and Build Metadata

Verified in both Debug and Release configurations:

- Marketing version: `2.3.5`
- Proposed build number: `335`
- Minimum system version: `macOS 26.2`
- Supported architecture: Apple Silicon `arm64`
- Bundle identifier: `no.blogspot.RawCull`

Built application inspection confirmed:

- `CFBundleShortVersionString`: `2.3.5`
- `CFBundleVersion`: `335`
- `LSMinimumSystemVersion`: `26.2`
- Executable format: arm64 Mach-O

The About window reads version and build information directly from the application bundle.

## 📦 Package Documentation

Updated the README package table to match `Package.resolved`:

- RawParserKit `1.2.8`
- RawCullCore `1.1.2`

All seven directly documented package names and versions now agree with the resolved package file.

## 🧭 RawCull 3.0.0 Handoff

- Added a separate AI-preserving stabilization plan for RawCull 3.0.0.
- The plan was based on read-only inspection of the current `main` and `version-3.0.0` implementation.
- Every 2.3.5 requirement is classified for 3.0.0 as:
  - Apply unchanged in behavior.
  - Adapt to the AI architecture.
  - Already resolved and requiring verification.
  - Superseded by a 3.0.0 implementation.
  - Not applicable to macOS 27.
- The plan protects:
  - PhotoAIKit.
  - Core AI model resources.
  - CLIP similarity.
  - Semantic search.
  - Vision fallback.
  - SAM 3 Deep Review.
  - Typed similarity artifacts.
  - Model downloads and licence acceptance.
  - AI settings and diagnostics.
- The 3.0.0 plan explicitly prohibits:
  - Merging the 2.3.5 branch into the AI code line.
  - Cherry-picking the maintenance implementation.
  - Mechanically transplanting 2.3.5 code.
  - Replacing AI implementations with older Vision-only implementations.
- No 3.0.0 production code, tests, dependencies, project settings, model resources, or branch pointers were changed.

## ⚠️ Known Limitation

During selected-catalog preload, thumbnail grids are temporarily gated instead of coalescing scan and UI extraction tasks through a shared request registry.

The grid resumes automatically when preload completes, is cancelled, or is superseded.

## 🖥️ Compatibility

- Requires macOS 26.2 or later.
- Requires an Apple Silicon Mac.
- Adds no new AI or model-download requirement.
- Continues to use the macOS 26/Vision-based application architecture.

## 🚦Release Status

The implementation and automated gates are complete, but distribution remains pending the final manual release matrix.

Before publishing, the release still requires:

- Physical-display Actual Pixels verification.
- VoiceOver and keyboard-only verification.
- Real RAW replacement testing.
- Real-catalog scan-contention measurements.
- Upgrade and restart testing using copied RawCull 2.3.3 data.
- Testing on minimum-supported macOS 26.2.
- Testing on the latest available macOS 26 update.
- Clean-account installation and launch testing.
- Confirmation that App Store Connect build `231` is unused.
- Developer ID signing and hardened-runtime verification.
- Notarization and stapling.
- Gatekeeper assessment.
- DMG generation and SHA-256 publication.
- App Store upload.
- Creation of the final `2.3.5` tag at the exact tested commit.

## 📝 Implementation Commits

- `5da6ff4` — Record RawCull 2.3.5 stabilization baseline
- `53c3057` — Fix actual-pixel viewport behavior and tests
- `7ba0a32` — Make histogram loading cancellation safe
- `5e802e6` — Make release test gates authoritative
- `2f9b033` — Make thumbnail cache identity source aware
- `4874fa1` — Gate thumbnail grids during catalog preload
- `dea2b9b` — Close similarity persistence regression gaps
- `36157c9` — Improve bounded accessibility semantics
- `ab711df` — Update RawCull 2.3.5 metadata documentation
- `de58ca3` — Stabilize integrated release verification
- `f2f86b6` — Prepare release and AI-safe 3.0.0 handoff
