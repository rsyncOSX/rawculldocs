+++
author = "Thomas Evensen"
title = "Version 3.2.6"
date = "2026-09-28"
tags = ["changelog", "version 3.2.6"]
categories = ["changelog"]
+++

To be released on Apple App Store.

# RawCull Changelog: v3.2.4 → v3.2.6

This changelog covers development since the released RawCull 3.2.4 through the latest source commit on September 28, 2026. It includes the intervening 3.2.5 work and the current 3.2.6 implementation; it does not establish App Store availability for 3.2.6.

<div class="alert alert-secondary" role="alert">

RawCull 3.2.6 adds an Objects mode to AI Analysis, combining SAM 3 instance segmentation with Qwen vision-language assessment of numbered subjects. This update also improves AI selection and rerun behavior, moves image-review state into focused sessions, keeps every frame visible in burst grids, strengthens copy-folder access handling, and adds a separate Release-mode integration test runner for real RAW photographs. AI inference continues to run locally on the Mac.

</div>

RawCull requires macOS 27 (Golden Gate), an Apple Silicon Mac, and Xcode 27 when building from source.

**Commit range:** `v3.2.4..89cb666`  
**Baseline tag:** `v3.2.4` at `6612d1a` (2026-09-21), RawCull 3.2.4 Build 383  
**Latest commit:** `89cb666` (`release test`, 2026-09-28)  
**Current version:** RawCull 3.2.6 (Build 392)  
**Scope:** 64 commits after the 3.2.4 tag

The tagged baseline is used for this comparison. The earlier 3.2.4 blog post describes a pre-tag snapshot and lists Build 380; the released tag itself declares Build 383.

## 🚀 Major Features

- Added **Objects** beside **SAM 3 + CLIP** and **Qwen Vision** in the AI Analysis workspace.
- Added automatic discovery of visible object categories and a manual **Specific Concepts** mode.
- Added separate SAM 3 object instances, numbered object crops, mask outlines, and Qwen assessments for each retained subject.
- Added retryable object-assessment failures with reuse of compatible cached segmentation masks.
- Fixed AI result views overriding a photograph selected in Grid View.
- Allowed SAM 3 + CLIP Deep Review to run again when the analysis target changes.
- Extracted focused comparison, Loupe, Zoom, Burst, and histogram presentation models.
- Removed burst-grid Clean View and collapse controls so every frame remains visible.
- Added an opt-in, hostless Release integration test target and Markdown analysis reports.

## 🧠 Objects: SAM 3 and Qwen Analysis

- Added analysis of the selected or rated photographs already available to the AI Analysis workspace.
- Added **Automatic** mode, where Qwen proposes concrete, visible object categories before SAM 3 locates their instances.
- Added **Specific Concepts** mode with comma-separated categories such as `bird, person, car`.
- Limited discovery and manual input to six concepts, normalized duplicate concepts, and rejected invalid or empty manual entries.
- Added editable photographic criteria covering visibility, focus, expression, obstructions, and strengths.
- Loaded analysis images at a maximum side of 4,320 pixels and requested up to eight SAM 3 instances per concept.
- Filtered weak, empty, tiny, and near-full-image masks before assessment.
- Deduplicated nearly identical masks across concepts while retaining alternative concept names as aliases.
- Retained up to eight objects for the final review board, with numbered IDs and normalized bounding boxes.
- Rendered one board containing the photograph overview and numbered object crops for Qwen to assess together.
- Refined board numbering and aspect-fit rendering to keep crops and outlines readable.
- Added prompts that distinguish repeated overview/crop views from separate photographs and require descriptions to match the numbered crops.
- Added per-object descriptions, visibility, focus quality, expression, obstructions, strengths, problems, and confidence.
- Added photograph-level summaries, relationships, strengths, problems, preferred object IDs, and confidence.
- Treated segmentation with no retained objects as a valid result, without asking Qwen to assess an empty board.

## 🖼️ Objects Results and Progress

- Added a results table showing filename, retained-object count, concepts, Qwen confidence, and completion or failure status.
- Added a detail panel with the photograph overview, numbered subject boxes, cached-mask outlines, and a crop of the selected object.
- Displayed alternative concept aliases alongside the retained object category.
- Distinguished **SAM 3 mask scores** from **Qwen assessment confidence** in the interface.
- Added visible progress stages for image loading, concept discovery, segmentation, board preparation, and object assessment.
- Added sequential batch processing, cancellation, **Retry Failed**, and **Clear Results** controls.
- Preserved completed results and skipped successful photographs when continuing analysis.
- Kept invalid structured assessments retryable and preserved the original response for inspection.
- Reused cached segmentation for assessment retries when source metadata, concept mode, Qwen model name, and mask-cache entries remain compatible; otherwise performed segmentation again.
- Added guidance for missing SAM 3, missing Qwen, both missing, no results, no matching objects, and no selected result.
- Added accessibility labels for analysis modes, input sources, object concepts, results, mask scores, and selected-object crops.

## 🔧 Qwen Response Reliability and Model Lifecycle

- Added a general vision-request interface with an instruction, image, and response-token limit, shared by Qwen Vision and Objects.
- Serialized Qwen generations across features sharing the same inference runtime.
- Added cancellable waiting for queued generation requests.
- Cancelled active inference when the model is cleared or revalidated, and rejected results from superseded model generations.
- Validated token limits and rejected empty model responses.
- Used 384 response tokens for concept discovery and 1,024 for object assessment; retained the existing 512-token whole-photo assessment limit.
- Added JSON-envelope extraction followed by strict discovery and assessment validation.
- Added specific errors for incomplete JSON, missing or mistyped fields, invalid values, excessive list lengths, and duplicate, unknown, or missing object IDs.
- Required the structured assessment to cover exactly the object IDs sent on the board, with finite confidence values between zero and one.
- Accepted observed response variations including numeric board IDs, an omitted optional image summary, and singular text values in list fields, while retaining ID and value validation.
- Added optional private diagnostic capture of raw discovery and assessment responses with model, token-limit, and response-length metadata.
- Added separate in-memory and optional disk stores for object masks, alongside the existing subject-mask stores.
- Connected Objects availability to the application-owned SAM 3 service and shared Qwen runtime.
- Cancelled Objects work when its model services change and disabled it while managed model locations are being refreshed.

## 🎯 AI Workspace, Selection, and Deep Review

- Grouped image-source controls beside the analysis-tool choices and expanded the mode picker for the third tool.
- Allowed switching tools or image sources during analysis, cancelling the affected in-flight work.
- Preserved an existing Grid selection when returning to stored Qwen or Deep Review results; chose a result automatically only when no photograph was selected.
- Tracked completed Deep Review photographs separately for each target preset.
- Allowed photographs analyzed under one target to be analyzed under another, such as **Full Subject** or **Head / Face**.
- Removed unused Deep Review summary presentation and controller forwarding code.

## 🔍 Comparison, Loupe, Zoom, and Burst Review

- Moved comparison selection, display scope, image state, source flags, viewport state, loading, reloads, and focus-mask work into `ComparisonSessionModel`.
- Added a comparison presentation snapshot for visible files, rows, selection, recommendation, focus status, and analysis status.
- Extracted shared image-review policies for source selection, rating display/actions, keyboard actions, zoom limits, and request identity.
- Added `LoupeSessionModel`, `ZoomSessionModel`, and `BurstWorkspaceSessionModel` to own workspace-specific review state and lifecycle.
- Routed review actions through focused rating, navigation, focus-analysis, and subject-outline dependencies.
- Added request tracking and cancellation checks to prevent stale image or focus results from replacing current review state.
- Hardened comparison completion handling so older reloads and bulk loads superseded by newer work are rejected.
- Preserved the existing review controls and source choices during the session extraction.
- Moved histogram bins and asynchronous presentation into `HistogramPresentationModel`.
- Renamed the Loupe image view to `LoupeMainImageView` and standardized several model and view property names.

## 🎞️ Burst Grid and Catalog Sorting

- Removed **Clean View**, the top-three-frame limit, and burst expand/collapse controls.
- Kept every burst frame available in ranked order, appending frames absent from the ranking.
- Kept review/defer actions without hiding frames after a review-state change.
- Replaced the nested lazy horizontal stack with an eager stack so burst rows retain a stable height inside the outer lazy list.
- Replaced generic catalog sort comparators with a typed descriptor for filename, modification date, and size, in either direction.
- Used natural filename comparison and explicitly preserved input order for equal sort values.
- Retained case-insensitive filename filtering after sorting.

## 📂 Copy-Folder Access and Reliability

- Added `CopyBookmarkStore` as the shared owner of destination bookmark creation, resolution, refresh, and access acquisition.
- Preserved the existing `destBookmark` preference key so saved destinations remain usable.
- Added scoped-access objects that release successful folder-access acquisitions once, including during cleanup or deinitialization.
- Kept source and destination access active for the copy operation and released both during cleanup.
- Refreshed stale destination bookmarks while access was active; a refresh failure did not discard an otherwise usable grant.
- Preserved the previous saved destination if selecting or saving a replacement failed.
- Added explicit missing, invalid, denied-access, and save-failure handling with folder-reselection guidance.
- Updated the folder picker and copy executor to use the same bookmark store.
- Standardized copy, source/destination, and output-presentation naming.

## 🧪 Tests and Release Integration

- Added Objects tests for concept normalization, invalid discovery, manual-input validation, response envelopes, exact board-ID matching, confidence validation, and observed schema variations.
- Added tests for mask deduplication, concept aliases, numbered board crops, transparent contours, automatic analysis, no-match results, model-removal cancellation, and retries using cached masks.
- Added runtime and model-download tests for Objects service composition and managed-model availability.
- Added Qwen generation-gate coverage and opt-in real-model probes.
- Added comparison-session, shared review-policy, focused-dependency, Loupe, Zoom, and Burst session tests.
- Added regression coverage for comparison cancellation, out-of-order reloads, superseded bulk results, and selected image sources.
- Added copy-bookmark tests for saved-destination compatibility, refresh behavior, denied access, failed replacement, and balanced access release.
- Added natural-sort, stable-tie, and revised burst-frame-order coverage.
- Raised the expected smoke enumeration from 222 to 227 tests and retained two performance tests.
- Updated release-metadata expectations for version 3.2.6 and matching app/downloader build numbers.
- Added `RawCullReleaseTests`, its shared scheme, and the `ReleaseObjects` test plan, separate from ordinary app, smoke, and performance tests.
- Added `make releastest` to run the production Objects, Qwen, and RAW-loader sources in a hostless Release target on arm64 macOS 27.
- Added configurable catalog and model paths, with a default nonrecursive scan of ARW files in Downloads.
- Added a unique Markdown report written before model initialization, after each photograph, and at completion, containing identities, concepts, masks, assessments, timings, raw fallback responses, and failures.
- Continued processing remaining photographs after a per-photo failure and returned failure for incomplete analysis.
- Added isolated release-runner contract tests and required explicit environment opt-in for real-model inference.

## 📋 Recorded Validation and Current Limits

- The checked-in Objects workplan records a September 24 Xcode run with **466 tests passed and one skipped out of 467**. This is recorded historical evidence, not a test run performed while preparing this changelog.
- The documented eight-photo packaged-model probe produced **16 structured results from 16 analyses** across Automatic and manual `bird` modes, with the expected board-ID sets.
- The workplan also records completed in-app Automatic batches for eight puffin photographs and eight mixed-subject photographs.
- Visual grounding remains under validation: recorded examples include swapped crop descriptions, repeated descriptions, and an incorrect additional-bird claim despite valid structured output and high reported confidence.
- Objects assessments remain advisory. Structured decoding, successful model downloads, and Qwen confidence do not establish factual accuracy.
- Successful photographs are skipped on subsequent analysis. Use **Clear Results** before comparing discovery modes or changing criteria for a photograph already marked Complete.
- Broader subject/overlap/no-match testing, clean-install TestFlight lifecycle checks, and release-build resource measurements remain documented follow-up work.

## 🛠️ Dependencies, Build, and Documentation

- Advanced the tagged RawCull 3.2.4 Build 383 baseline through 3.2.5 and into RawCull 3.2.6 Build 392.
- Updated PhotoAIKit to revision `77cc1d84a5d98a485caa15be102c8a55eb3d7698`, including the additive SAM 3 instance API alongside existing union-mask support.
- Updated Apple Core AI models to revision `475c585fdb0fe82a83c8f777f259e9414bd44c98`.
- Updated resolved Swift ASN.1 to 1.7.3, Swift Collections to 1.7.0, and Swift Hugging Face to 0.11.0.
- Extended AI import-boundary verification for the object-analysis workflow.
- Updated the README for the current AI workflows, model requirements, and application structure.
- Expanded the SAM 3 → Qwen workplan with implementation checkpoints, response-reliability findings, real-model results, known issues, and remaining release evidence.
- Added a detailed Apple-hosted model-pack runbook covering packaging, provenance, uploads, processing, and exact-build TestFlight download checks.
- Revised Apple assets documentation to distinguish the released 3.2.4 distribution baseline from ongoing 3.2.6 Objects validation.
- Documented the separate Release integration runner and its report format.

## 📦 Development Progression

- **September 21:** began post-3.2.4 correctness hardening and catalog/review refinements.
- **September 22:** extracted comparison and shared review policies, adopted focused Loupe/Zoom/Burst sessions, and refined burst presentation.
- **September 23:** completed the 3.2.5 work, strengthened copy bookmarks, fixed AI result selection and target reruns, and advanced to Build 388.
- **September 24:** implemented the 3.2.6 Objects pipeline, UI, caching, response validation, and real-model validation refinements.
- **September 26:** updated release and model-pack guidance with Objects test evidence and known visual-grounding issues.
- **September 28:** added the Release Objects integration test target and report runner; current source is Build 392 at `89cb666`.
