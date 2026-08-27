+++
author = "Thomas Evensen"
title = "Version 3.1.1 beta"
date = "2026-08-27"
tags = ["changelog","version 3.1.1 beta"]
categories = ["changelog"]
+++

# RawCull Changelog: v2.3.5 → Latest Commit

RawCull version 3.1.1 is also available via Apple’s TestFlight; please email thomeven@gmail.com if you would like to try it through that service.

<div class="alert alert-secondary" role="alert">

I have joined the beta testing for the RocketTrace app, and several concurrency problems have been resolved, prompting a new build. Antoine van der Lee has produced an impressive application and effort. I will keep testing, and further issues will bring additional updates.

</div>

RawCull 3.1.1 beta requires the latest public or developer beta of macOS 27 (Golden Gate).

**Commit range:** `3b1c41d.. e3a58e1 `  
**Latest commit:** `e3a58e1 ` (`concurrency fixes`, 2026-08-27)  
**Current version:** RawCull 3.1.1 (Build 309)


## 🚀 Major Features

- Introduced the RawCull 3 local AI workflow.
- Added natural-language semantic photo search using CLIP models.
- Added CLIP-powered visual similarity analysis and burst grouping.
- Added SAM 3 Deep Review with subject isolation, detailed scoring, and burst-winner recommendations.
- Added automatic fallback to Apple Vision similarity when the selected CLIP model is unavailable.
- Added persistent storage for AI embeddings, masks, scores, signatures, and burst-review decisions.

## 🧠 AI Models

- Added dedicated AI model settings and model-management views.
- Added model bundle discovery, validation, and resource management.
- Added support for selecting CLIP, SAM 3, and EfficientSAM models.
- Added model-location controls and validation feedback.
- Added model-download and Managed Background Assets infrastructure.
- Added license-acceptance handling for externally distributed models.
- Added model provenance, license notices, manifests, and installation documentation.
- Improved model-download validation, error handling, and test coverage.

## 🔍 Grid and Similarity View

- Added clearer sharpness-scoring controls with scoring, re-scoring, sorting, and cancellation actions.
- Added contextual scoring targets for selected images, filtered images, or the complete catalog.
- Added combined **Index & Find Similar** behavior when CLIP index entries are missing.
- Improved similarity backend status and progress presentation.
- Improved selection coordination and keyboard navigation.
- Refined grid controls, button styling, and rating interactions.

## 🔎 Loupe View

- Added a redesigned metadata panel for the selected image.
- Added expandable and collapsible metadata sections.
- Added keyboard control for showing or hiding metadata.
- Added filename, folder, camera, exposure, rating, and culling information.
- Improved source-image switching and one-to-one zoom rendering.
- Fixed one-to-one zoom so it displays actual image pixels.
- Improved thumbnail and image navigation while using Loupe View.

## 🎞️ Burst View

- Redesigned the burst-culling workspace around a photo-first review experience.
- Redesigned the Burst Groups home screen with a guided **Next up** card, review queues, recent groups, and completion progress.
- Added staged catalog-preparation status for semantic indexing, sharpness scoring, and burst-group discovery.
- Added clearer progress and estimated-time feedback while preparing a catalog.
- Added separate queues for bursts that need review, completed bursts, and all burst groups.
- Added maintenance actions for scoring parameters, catalog re-indexing, and similarity indexing.
- Added clearer burst numbering and frame-position indicators.
- Added navigation back to the burst list.
- Added reviewed and unreviewed burst controls.
- Added keyboard-action guidance for navigation, zoom, source selection, focus, rating, picks, and rejection.
- Added burst-aware metadata to the zoomed image view.
- Added culling status, rating, sharpness, focus, review, and recommendation information.
- Improved comparison-grid navigation and selected-frame handling.

## 🗂️ Sidebar and Navigation

- Refined the main catalog sidebar and file presentation.
- Added clearer empty, filtered, and missing-file states.
- Improved scan progress by reporting the discovered-file count directly.
- Added accurate completed/total progress for thumbnail creation and JPEG export.
- Added image-sorting progress and status indicators.
- Moved additional navigation actions into application menu commands.
- Improved toolbar organization and reduced duplicate controls.
- Refined rating controls, filters, and rating-pin behavior.
- Corrected rated-photo counts so only files with an explicit rating are counted.

## ⚡ Performance and Reliability

- Added replacement-safe thumbnail cache identities.
- Prevented stale cached thumbnails from being reused after files are replaced.
- Added bounded thumbnail preloading to reduce CPU and memory contention.
- Improved thumbnail loading, scanning, and disk-cache coordination.
- Simplified thumbnail and shared-memory cache paths after removing obsolete diagnostics instrumentation.
- Made histogram loading use the latest request when selections change quickly.
- Improved JPEG extraction progress, cancellation handling, and per-file failure reporting.
- Improved AI artifact persistence and cache-boundary handling.
- Improved catalog loading, sorting, saved-file persistence, and culling-state restoration.
- Improved similarity, sharpness, focus, and burst-analysis consistency.

## ♿ Accessibility

- Added bounded accessibility labels, values, and hints across major workflows.
- Improved accessibility for ratings, image controls, focus controls, model management, and comparison views.
- Added accessible descriptions for image sorting and metadata controls.
- Improved keyboard-accessible culling and navigation actions.
- Added accessible progress descriptions to the redesigned Burst Groups workflow.

## 🧪 Testing and Quality

- Added tests for semantic search, Deep Review, and AI integration.
- Added model-download and model-validation test coverage.
- Added thumbnail cache identity and contention tests.
- Added histogram request-ordering tests.
- Added typed AI persistence and cache-boundary tests.
- Added accessibility presentation tests.
- Added Loupe and Burst metadata tests.
- Added focused tests for Burst catalog-preparation presentation and JPEG-export selection behavior.
- Updated culling, burst queue, navigation, concurrency, and thumbnail-provider tests for the latest behavior.
- Removed obsolete diagnostics and contention tests together with the retired diagnostics implementation.
- Updated native Xcode smoke-test enumeration to 177 enabled tests with no duplicate identifiers.
- Expanded release metadata and package migration verification.
- Improved concurrency and Thread Sanitizer stability.

## 🧹 Diagnostics and Code Cleanup

- Removed the Diagnostics menu and the memory, RAW-file, similarity, and semantic-search diagnostics screens.
- Removed the supporting diagnostics logs, view models, cache statistics, and obsolete contention instrumentation.
- Removed unused production APIs and their obsolete test-only coverage.
- Retained active operational logging and intentional test-support APIs.
- Added a cleanup plan documenting completed removals, retained APIs, validation, and remaining non-functional cleanup.

## 🛠️ Build and Documentation

- Updated the project for RawCull 3.0.0, 3.1.0, and 3.1.1.
- Advanced RawCull 3.1.1 from Build 307 to Build 309.
- Updated README branch and release-artifact references to `version-3.1.1`.
- Added a dedicated model-downloader target and supporting entitlements.
- Updated Swift package dependencies and project configuration.
- Added model installation, provenance, and licensing documentation.
- Added Copy Engine design documentation.
- Updated the README with the RawCull 3 architecture, AI workflow, requirements, and build instructions.
- Updated export, release, smoke-test, and performance-test configuration.

## 📦 Version Progression

- Released the initial RawCull 3.0.0 implementation.
- Stabilized AI persistence, thumbnail loading, accessibility, and release validation.
- Advanced the project to RawCull 3.1.0.
- Updated the current release to RawCull 3.1.1, Build 309.
- Updated this changelog through commit `be9d7dd` on 2026-08-26.
