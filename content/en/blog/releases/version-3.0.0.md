+++
author = "Thomas Evensen"
title = "Version 3.0.0"
date = "2026-08-31"
tags = ["changelog","version 3.0.0"]
categories = ["changelog"]
+++
 
# RawCull Changelog: v2.3.5 → v3.0.0

Updated on [Apple App Store](https://apps.apple.com/no/app/rawcull/id6759362764?mt=12).

*Important*: This release is the final version for *macOS 26*. The next version, 3.2.0, will introduce AI features exclusively available on macOS 27 upon its release. Version 3.0.0 will remain available on GitHub for an extended period following the release of macOS 27. The Apple App Store version will only support macOS 27 once version 3.0.0 is released.

<div class="alert alert-secondary" role="alert">

Version 3.0.0 on the Apple App Store is built and uploaded to App Store Connect for release on the App Store by utilizing Xcode Cloud. I have installed macOS 27 developer beta on both my Macs and cannot build and upload a release version directly from Xcode 27 beta. The version 3.0.0 on GitHub is built by Xcode 27 beta, and if you have some doubts about it, please install this version from the Apple App Store.

Version 3.0.0 is based on the latest version 3.2.0 with all AI features removed. There have been several updates since the last release, version 2.3.5.

</div>

 ------------------------------------------------------------------------------------------------------------------------------
  
# 📸 RawCull 3.0.0 Changelog

## 🍂 macOS Tahoe

- Updated RawCull for macOS Tahoe 26.
- Retained Apple Silicon as the supported architecture.
- Updated project settings, dependencies, metadata, entitlements, and release configuration.
- Updated the application version to 3.0.0.

## 👁️ Vision similarity

- Rebuilt image similarity around Apple Vision feature prints supplied by PhotoAnalysisKit.
- Vision is the sole similarity backend.
- Added a dedicated Vision similarity service and simplified runtime composition.
- Added bounded parallel indexing, progress reporting, cancellation, and partial-failure handling.
- Added finite-distance validation to prevent invalid similarity results.
- Added cache-first loading so previously indexed photographs can be reused.
- Added generation tracking so cancelled or superseded operations cannot publish stale results.
RawCull 3.0.0 contains no CLIP, semantic search, SAM, Deep Review, downloadable models, model licences, or external model runtime.

## 📸 Burst analysis and review

- Reorganized burst processing around a dedicated BurstAnalysisCoordinator.
- Added clearer lifecycle handling for catalog preparation, indexing, grouping, ranking, and cancellation.
- Added an explicit catalog-preparation screen before burst analysis.
- Added more reliable reuse of compatible saved burst analysis.
- Added a choice between using an existing index and performing a full re-index.
- Improved review-queue counts and filtering.
- Preserved manual winner selection, ratings, rejection, undo, and review state.
- Prevented older analysis generations from overwriting newer results.
- Improved progress presentation during sharpness scoring, similarity indexing, and grouping.

## 🔍 Loupe and metadata

- Corrected Actual Pixels to display one source-image pixel per display point.
- Improved Actual Pixels centring around autofocus points.
- Added safe clamping for focus points near image edges.
- Added protection against invalid image and viewport geometry.
- Added F to show or hide the focus map.
- Added A to show or hide camera autofocus points.
- Added E to show or hide image metadata.
- Retained J for embedded JPEG and R for developed RAW previews.
- Added a collapsible metadata panel containing file, camera, exposure, rating, focus, sharpness, and histogram information.
- Added Show in Finder and Open RAW File actions to the metadata panel.
- Replaced the older file inspector with the integrated metadata presentation.

## 🖼️ Histograms and previews

- Made histogram loading cancellation-safe.
- Added latest-result-wins behavior when navigating quickly between photographs.
- Removed the crash path for failed image conversion.
- Histograms now clear safely when an image cannot be loaded.
- Improved comparison-image loading and cancellation.
- Added richer metadata and histogram information to burst comparison views.
- Improved source selection between thumbnails, embedded JPEGs, and developed RAW previews.

## ⭐ Rating and selection

- Rating filters now activate only when the selected catalog contains explicit ratings.
- Improved multi-selection behavior in thumbnail and culling grids.
- Improved Command-click, Shift-click, and keyboard selection handling.
- Preserved selection correctly while navigating grids and comparison views.
- Improved rating application across multiple selected photographs.
- Improved rating, keeper, and rejection presentation.
- Prevented selection state from becoming detached from updated catalog records.

## 📁 Catalogs and saved files

- Improved the main catalog sidebar and its empty states.
- Saved catalogs no longer cause an incorrect “No Files Found” message.
- Converted saved catalog and file rows to native button interactions.
- Improved selection persistence in the Saved Files window.
- Added clearer VoiceOver descriptions for saved catalogs and records.
- Improved handling of damaged saved-state files.
- Added an option to archive damaged persistence data and reset safely.
- Improved atomic writing of settings and saved data.

## ⌨️ Menus and shortcuts

- Added File › Add Catalog… with ⌘O.
- Added menu commands for copying tagged files and showing saved files.
- Added Actions › Extract JPGs with ⌘J.
- Added Actions › Abort task with ⌘K.
- Simplified the main toolbar by moving secondary actions into menus and contextual panels.
- Added a redesigned, context-sensitive About window.
- The About window now documents shortcuts for browsing, rating, previews, burst groups, burst review, and manual comparison.

##⚡ Cache and performance

- Added source-aware thumbnail cache identities.
- Thumbnail keys now include source metadata, requested size, purpose, and orientation policy.
- Replacing a photograph at the same path no longer reuses its stale thumbnail.
- Improved separation between thumbnail, full-size preview, similarity, and burst-analysis caches.
- Added bounded thumbnail preloading during catalog scans.
- Reduced duplicate thumbnail decoding and scan/grid contention.
- Improved memory- and disk-cache consistency.
- Added reusable per-file Vision artifact storage.
- Added a new Vision-specific cache schema and compatibility signature.
- Old or incompatible similarity caches are treated as regenerable cache misses.
- Catalogs, ratings, source photographs, and exported files remain outside cache cleanup.

## ♿ Accessibility

- Added clearer VoiceOver labels, values, hints, and selected states.
- Improved accessibility for thumbnails, ratings, focus controls, image sources, saved files, and comparison images.
- Improved keyboard navigation throughout catalog and review workflows.
- Preserved established single-click and double-click image behavior.
- Added accessible descriptions for progress and review states.
- Improved focus handling when moving between grids, previews, and sheets.

## 🧹 Streamlining

- Removed the standalone RAW diagnostics panel.
- Removed the standalone memory diagnostics console.
- Replaced the old file inspector with the integrated metadata panel.
- Removed obsolete cache statistics and diagnostic infrastructure.
- Consolidated similarity and burst-related implementation under the new Intelligence structure.
- Simplified settings while retaining cache, thumbnails, focus, and general configuration.

## 🛡️ Reliability

- Improved structured cancellation across scanning, extraction, thumbnail generation, sharpness scoring, similarity indexing, and burst analysis.
- Improved source fingerprint validation for cached artifacts.
- Corrupt or incompatible cache records now behave as cache misses rather than breaking catalog loading.
- Improved atomic cache and settings writes.
- Improved persistence error reporting and recovery.
- Strengthened security-scoped file access handling.
- Improved task isolation and reduced the risk of stale UI updates.

## 🧪 Tests and release verification

- Added Vision similarity service and persistence tests.
- Added burst coordinator lifecycle and compatibility tests.
- Added catalog-preparation presentation tests.
- Added thumbnail identity and same-path replacement tests.
- Added histogram cancellation and latest-result tests.
- Expanded grid selection, comparison navigation, rating, and persistence tests.
- Added metadata-panel and keyboard-shortcut tests.
- Added accessibility presentation tests.
- Added release metadata and smoke-manifest integrity tests.
- Strengthened concurrency and Thread Sanitizer coverage.
- Added automatic verification that release test manifests remain complete.
