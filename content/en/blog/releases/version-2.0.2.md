+++
author = "Thomas Evensen"
title = "Version 2.0.2"
date = "2026-06-09"
tags = ["changelog","version 2.0.2"]
categories = ["changelog"]
+++
 
# RawCull Changelog — v1.9.6 → 2.0.2

Updated on [Apple App Store](https://apps.apple.com/no/app/rawcull/id6759362764?mt=12).
 
 ------------------------------------------------------------------------------------------------------------------------------
 
## ✨ New — Image source toggle in zoom / loupe view
 
 Switch between the embedded JPEG preview and a full-size JPEG created from the RAW file while inspecting an image:
 
  - New `ImageSourceToggleView` with keyboard shortcut support
  - `ZoomPreviewHandler` significantly expanded to manage source switching
  - `ZoomOverlayView` updated with key-action routing
  - `FullSizeJPGDiskCache` extended to support the toggle flow
  - New `DiskCacheAndScanAdmissionTests` covering the cache admission logic
 
 ------------------------------------------------------------------------------------------------------------------------------
 
## ✨ New — Burst Review Queue
 
 A structured review-queue workflow for burst groups:
 
  - New `BurstReviewQueueFilter` (`.all`, `.needsReview`, `.deferred`, `.reviewed`) — filter the culling grid to a specific review state
  - New `BurstReviewQueuePolicy` — stateless logic that derives each group's effective state (high-confidence, no cautions → auto-reviewed; low confidence or open cautions → needs review)
  - New `BurstReviewQueueCounts` value type exposed on `RawCullViewModel`
  - Review-state action buttons in `BurstGroupHeaderView`: **Reviewed**, **Needs Review**, **Defer** — rendered contextually per current state
  - State badge overlay on burst group headers (purple = Needs Review, blue = Reviewed, gray = Deferred)
  - Filter buttons added to `SimilarityGridSelectionView` toolbar — live counts shown inline; filter resets to `.all` on Exit Groups
  - Auto-sharpness scoring toggle removed from toolbar; scoring now triggered automatically via `runWithAutoScoring` before burst analysis runs
  - Review states persisted to the burst analysis disk cache on every state change
 
 ------------------------------------------------------------------------------------------------------------------------------
 
## 🏗️ Refactored — CullingGridView decomposed
 
 The monolithic `CullingGridView` split into focused, independently testable objects:
 
  - `CullingGridSelectionCoordinator` — selection state & keyboard navigation
  - `CullingGridRenderCache` — per-cell image caching (keyed by `CullingGridRenderCacheKey`)
  - `CullingGridProgressOverlay` — loading progress overlay
  - Cell data flow cleaned up: `isSelected`, `ratingValue`, `ratingDisplay`, `ratingColor` now passed directly to `ImageItemView`, removing ViewModel look-ups inside the cell
 
 ------------------------------------------------------------------------------------------------------------------------------
 
## 🏗️ Refactored — ComparisonGridView decomposed and redesigned
 
  - `ComparisonGridDisplayState` — display/layout state extracted from the view
  - `ComparisonGridImageCoordinator` — per-pane image loading coordination extracted from the view
  - Background changed to near-black (`Color.black.opacity(0.97)`) for a cinema-style comparison experience
  - `BurstComparisonEvidenceView` promoted to a floating overlay (`ZStack` + `.zIndex(2)`) so it no longer pushes image panes down
  - Image panes fill the full viewport height instead of being constrained to a fixed 3:2 ratio
  - Scroll indicators hidden; viewport transform resets automatically when the comparison group or active burst group changes
 
 ------------------------------------------------------------------------------------------------------------------------------
 
## 📦 New — RawParserKit SPM package (rsyncOSX/RawParserKit v1.1.0)
 
 All raw-file parsing logic extracted from RawCull into a standalone, reusable Swift package:
 
  - Sony: `SonyMakerNoteParser`, `SonyThumbnailExtractor`, `SonyRawFormat`, `JPGSonyARWExtractor`
  - Nikon: `NikonMakerNoteParser`, `NikonThumbnailExtractor`, `NikonRawFormat`, `JPGNikonNEFExtractor`
  - Shared: `RawFormat`, `RawFormatRegistry`, `RawParserDiagnostics`, `CancellableImageIOWork`, `ThumbnailSharpener`
  - ✨ New in package: `SonyRAWJPEGCreator` — Sony-specific embedded JPEG creator; `ThumbnailError` typed errors
 
 ------------------------------------------------------------------------------------------------------------------------------
 
## 📦 New — RawCullCore SPM package (rsyncOSX/RawCullCore v1.0.0)
 
 Core data models and domain logic extracted into a standalone package:
 
  - Burst: `BurstGroupingEngine`, `BurstRankingEngine`, `BurstAnalysisModels`
  - File model: `RawCullFileItem` (replaces the internal `FileItem`)
  - Histogram: `HistogramCalculator` (replaces the internal `CalculateHistogram`)
  - ✨ New in package: `ExifMetadata`, `FocusPointParser`, `RawCullSourceCatalog`, `SaliencyInfo` — new shared data types

 ------------------------------------------------------------------------------------------------------------------------------

## 🏗️ Refactored — ScanStatsSheetView expanded
 
  - Layout widened from 460 → 800 pt; content reorganised into a two-column `HStack`
  - Left column: Culling Status, Catalog Summary, Sharpness Summary, and a new **Burst Review** section (total groups, needs-review / deferred / reviewed counts)
  - Right column: new **Burst Label Guide** — caption-sized reference grid explaining every confidence and state badge
 
 ------------------------------------------------------------------------------------------------------------------------------
 
## 🧹 Cleanup & dead-code removal
 
  - `FocusPeakingControlsView` and related `BurstAnalysisModels` display-only properties removed (moved to package / no longer needed)
  - `CacheSettingsTab` simplified — removed stale `showResetConfirmation`, `showSaveSettingsConfirmation`, and `cacheConfig` state variables
  - `SharedMemoryCache` — 34 lines of redundant code removed
  - `SettingsViewModel` — dead state and unused helpers pruned
  - `ImageItemView` — substantial simplification following the cell data-flow refactor
  - `SharedMainToolbarContent` — stale toolbar items removed
 
 ------------------------------------------------------------------------------------------------------------------------------
 
## 🧪 Tests — Cleaned up & extended
 
 Tests for code that moved to SPM packages removed from the main test target; new and expanded tests added:
 
  - ➕ `CullingGridCoordinatorTests` — coordinator selection and keyboard-navigation logic
  - ➕ `ZoomOverlayKeyActionTests` (expanded) — image-source toggle key actions
  - ➕ `DiskCacheAndScanAdmissionTests` — full-size JPG disk-cache admission coverage
  - ➕ `CullingModelTests` (expanded) — `BurstWinnerOverride` exact-membership matching; upsert with differing member sets
  - ➕ `RawCullVerifyTestsConcurrencyTests` (expanded) — burst review-state concurrency coverage
  - ➖ Removed (now tested inside their respective SPM packages): `BurstAnalysisTests`, `NikonMakerNoteParserTests`, `SonyMakerNoteParserTests`, `SimilarityScoringTests`, `CancellableImageIOWorkTests`, `ComparisonCandidateInspectorTests`, `LoupeImageKeyActionTests`