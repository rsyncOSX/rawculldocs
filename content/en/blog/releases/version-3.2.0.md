+++
author = "Thomas Evensen"
title = "Version 3.2.0 beta"
date = "2026-08-30"
tags = ["changelog","version 3.2.0 beta"]
categories = ["changelog"]
+++

# RawCull Changelog: v3.1.1 → v3.2.0 beta

RawCull 3.2.0 beta is the modular-AI development release based on RawCull 3.1.1. RawCull 3.2.0 is available through Apple’s TestFlight; please email thomeven@gmail.com if you would like to try that build.

<div class="alert alert-secondary" role="alert">

RawCull’s AI implementation has been reorganized behind focused feature boundaries for similarity, semantic search, burst analysis, Deep Review, model management, persistence, and presentation. This is a structural refactor: DataComp CLIP powers similarity and semantic search, SAM 3 powers Deep Review, and existing behavior and persisted data formats are preserved.

</div>

RawCull 3.2.0 beta requires macOS 27 (Golden Gate), an Apple Silicon Mac, and Xcode 27 when building from source.

**Commit range:** `3b1c41d..56931c2`
**Latest commit:** `56931c2` (`update docs`, 2026-08-30)
**Current version:** RawCull 3.2.0 (Build 350)

## 🚀 Major Features

- Introduced the RawCull 3 local AI workflow.
- Added natural-language semantic photo search using DataComp CLIP.
- Added DataComp CLIP-powered visual similarity analysis and burst grouping.
- Added SAM 3 Deep Review with subject isolation, detailed scoring, and burst-winner recommendations.
- Added persistent storage for AI embeddings, masks, scores, signatures, and burst-review decisions.
- Modularized the complete AI application layer while preserving the RawCull 3.1.1 workflow, ranking policies, cache formats, preferences, and migration behavior.

## 🧩 Modular AI Architecture

- Added one application-owned `RawCullIntelligenceRuntime` that creates and retains stable AI feature objects for the app session.
- Replaced settings callbacks with revisioned, typed AI configuration snapshots that reject stale updates and avoid unnecessary restarts.
- Split model discovery, downloads, licence acceptance, progress, cancellation, retry, removal, and installed-model locations into a focused model-management component.
- Added dedicated semantic-search and similarity feature surfaces so views no longer inspect providers, embeddings, repositories, or backend descriptors directly.
- Moved burst computation into a dedicated coordinator with typed pipeline values, cache restoration, compatibility migration, grouping, ranking, lifecycle handling, and review-state preservation.
- Added a focused Deep Review controller for availability, request construction, operation lifecycle, cancellation, and burst-specific winner application.
- Reorganized 26 AI source files under `RawCull/Intelligence` into Composition, Contracts, Similarity, SemanticSearch, BurstAnalysis, DeepReview, ModelManagement, Persistence, and Presentation areas.
- Kept the intelligence boundary application-local after evaluating a separate Swift package; the current code still depends intentionally on app-owned catalog snapshots, paths, resources, policy callbacks, and Background Assets wiring.
- Removed temporary view-model constructors, runtime model exposure, and semantic-search, similarity, and Deep Review forwarding APIs after callers migrated to the focused features.
- Added exact import-boundary enforcement for all concrete AI backends and PhotoAIKit products. SwiftUI views and general model code no longer import restricted AI modules.
- Kept persistence modularization Phase 9 deferred to avoid mixing storage API changes with the completed structural refactor. Existing on-disk formats remain unchanged.

## 🧠 AI Models

- Added dedicated AI model settings and model-management views.
- Added model bundle discovery, validation, and resource management.
- Added support for DataComp CLIP and SAM 3 models.
- Added model-location controls and validation feedback.
- Added model-download and Managed Background Assets infrastructure.
- Added licence-acceptance handling for externally distributed models.
- Added model provenance, licence notices, manifests, and installation documentation.
- Improved model-download validation, retry behavior, error handling, and test coverage.
- Fixed modular-runtime and model-download integration after the `Intelligence` file reorganization.
- Documented the version coordination required across RawCull, the downloader extension, Background Assets manifests, and model bundles.

## 🔍 Grid and Similarity View

- Added clearer sharpness-scoring controls with scoring, re-scoring, sorting, and cancellation actions.
- Added contextual scoring targets for selected images, filtered images, or the complete catalog.
- Added combined **Index & Find Similar** behavior when DataComp CLIP index entries are missing.
- Improved similarity backend status and progress presentation.
- Reserved consistent space for semantic-search activity messages to prevent layout shifts.
- Improved selection coordination and keyboard navigation.
- Refined grid controls, button styling, and rating interactions.
- Routed similarity state and actions through the focused similarity feature instead of the central application view model.
- Added RawCull-owned semantic-search backend presentation values for UI and accessibility without exposing PhotoAIKit types.

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
- Preserved manual winners and review decisions when compatible burst groups are restored or recomputed.
- Isolated burst analysis from catalog navigation and rating actions while retaining the existing user workflow.

## 🗂️ Sidebar and Navigation

- Refined the main catalog sidebar and file presentation.
- Added clearer empty, filtered, and missing-file states.
- Made the empty-state catalog icon an accessible button that opens the catalog folder picker.
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
- Reduced histogram-processing cost by sampling large previews to a maximum dimension of 512 pixels.
- Improved JPEG extraction progress, cancellation handling, selection behavior, and per-file failure reporting.
- Improved AI artifact persistence and cache-boundary handling.
- Improved catalog loading, sorting, saved-file persistence, and culling-state restoration.
- Improved similarity, sharpness, focus, and burst-analysis consistency.
- Preserved generation-token and cancellation behavior so superseded AI work cannot publish stale results.
- Kept DataComp CLIP and SAM 3 artifacts separated by compatible model identity.

## ♿ Accessibility

- Added bounded accessibility labels, values, and hints across major workflows.
- Improved accessibility for ratings, image controls, focus controls, model management, and comparison views.
- Added accessible descriptions for image sorting and metadata controls.
- Added accessible labeling and guidance to the empty-state Add Catalog button.
- Improved keyboard-accessible culling and navigation actions.
- Added accessible progress descriptions to the redesigned Burst Groups workflow.
- Decoupled semantic-search accessibility from PhotoAIKit backend types.

## 🧪 Testing and Quality

- Added tests for semantic search, Deep Review, similarity features, burst coordination, and intelligence-runtime identity and configuration.
- Added model-download and model-validation test coverage, including retry and modular-runtime integration.
- Added thumbnail cache identity and contention tests.
- Added histogram request-ordering and bounded-sampling tests.
- Added typed AI persistence and cache-boundary tests.
- Added accessibility presentation tests.
- Added Loupe and Burst metadata tests.
- Added focused tests for Burst catalog-preparation presentation and JPEG-export selection behavior.
- Updated culling, burst queue, navigation, concurrency, and thumbnail-provider tests for the focused AI feature boundaries.
- Added isolated test factories so AI and view-model tests do not depend on production caches, settings, or user data.
- Added checked-in smoke and performance enumeration verification with duplicate-identifier protection.
- Expanded the smoke manifest to 208 unique test selectors and retained two performance tests.
- Passed the final smoke, full Thread Sanitizer, performance, Debug-build, Release-build, import-boundary, and unused-code gates for Phase 12.
- Left the final manual acceptance matrix explicitly pending because its versioned photo catalog and installed licensed model resources were unavailable in the verification workspace.

## 🧹 Diagnostics and Code Cleanup

- Removed the Diagnostics menu and the memory, RAW-file, similarity, and semantic-search diagnostics screens.
- Removed the supporting diagnostics logs, view models, cache statistics, and obsolete contention instrumentation.
- Removed unused production APIs and their obsolete test-only coverage.
- Retained active operational logging and intentional test-support APIs.
- Removed obsolete AI compatibility shims after all callers moved to focused feature APIs.
- Completed an unused-code and import audit across `RawCull/Intelligence` with no findings in the modular AI boundary.

## 🛠️ Build and Documentation

- Updated the project for RawCull 3.0.0, 3.1.0, 3.1.1, and the 3.2.0 modular-AI development release.
- Advanced RawCull 3.2.0 to Build 350.
- Added a dedicated model-downloader target and supporting entitlements.
- Updated Swift package dependencies and project configuration.
- Added model installation, provenance, licensing, and macOS Background Assets release documentation.
- Added detailed modular-AI architecture, migration, dependency-boundary, rollback, and validation documentation.
- Added a high-level modular-AI guide for contributors who are new to the architecture.
- Updated the README with the version-3.2.0 branch purpose, final application-local ownership decision, AI workflow, requirements, and build instructions.
- Updated export, release, smoke-test, performance-test, and import-boundary verification.

## 📦 Version Progression

- Released the initial RawCull 3.0.0 implementation.
- Stabilized AI persistence, thumbnail loading, accessibility, and release validation.
- Advanced the project through RawCull 3.1.0 and RawCull 3.1.1 Build 345.
- Started RawCull 3.2.0 as the modular-AI development branch from the 3.1.1 behavior and data-format baseline.
- Completed modular-AI Phases 0–8, 10, 11, and 12; intentionally deferred persistence Phase 9.
- Updated the current development release to RawCull 3.2.0, Build 350.
- Updated this changelog through commit `56931c2` on 2026-08-30.
