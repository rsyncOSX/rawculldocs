+++
author = "Thomas Evensen"
title = "Version 3.3.3 - In Development"
date = "2026-10-16"
tags = ["changelog", "version 3.3.3"]
categories = ["changelog"]
summary = "In development, not yet released: Combined Review, evidence inspection, AI memory guidance, and keyboard, export, persistence, accessibility, and catalog reliability improvements since 3.2.8."
+++

**RawCull 3.3.3 is in development and has not yet been released.**

Combined Review brings local AI findings and their supporting evidence into a dedicated review workspace. This cumulative changelog also includes JPEG extraction, Burst Groups preparation status, and catalog and cache improvements since 3.2.8.

<!--more-->

**Changes:** v3.2.8 → v3.3.3 (in development)

This changelog continues from the [3.2.8 release notes]({{< relref "version-3.2.8.md" >}}) and covers source changes through October 16, 2026. It replaces the earlier 3.2.9 development changelog and includes the intervening development toward 3.3.3.

<div class="alert alert-warning" role="alert">

**In development — not yet released.** RawCull 3.3.3 is a development snapshot. Features and build numbers may change before release. This page does not announce App Store, TestFlight, or downloadable release availability.

</div>

RawCull requires macOS 27 (Golden Gate) and an Apple Silicon Mac. Building from source requires Xcode 27.

**Commit range:** `v3.2.8..97c68c49`  
**Baseline tag:** `v3.2.8` at `2b04aa6e` (2026-09-30), RawCull 3.2.8 Build 398  
**Latest source commit:** `97c68c49` (2026-10-16)  
**Current source version:** RawCull 3.3.3 (Build 421)  
**Release status:** In development; not yet released

## Major Enhancements

- Add **Combined Review**, a dedicated workspace combining Qwen, SAM 3, CLIP, and measured image evidence.
- Present concise findings beside the photograph, with an evidence workspace for inspecting supporting crops, measurements, conflicts, and limitations.
- Add memory guidance when enabling Combined Review in **Settings > AI**; the feature is off by default.
- Improve Objects response handling when the model cannot support a focus judgment.
- Support any number of ARW inputs in the local analysis diagnostic scripts, with bounded batches for Combined Review.
- Add complete-catalog JPEG extraction and clearer Burst Groups preparation status.
- Strengthen catalog cancellation, background folder access, asynchronous sorting, and cache accounting.
- Improve keyboard routing, embedded JPEG export, saved-review recovery, thumbnail readiness, Settings error reporting, and accessibility.

## Latest Updates — October 15–16

- Correct burst keyboard routing: **2** keeps the top two candidates in burst mode and remains rating 2 in ordinary grids. Unavailable burst commands no longer fall through to rating changes.
- Scope photo shortcuts to the active review window. Settings, About, sheets, text fields, and native controls retain their keyboard input. Guard against duplicate thumbnail keyboard monitors.
- Export embedded JPEGs directly from ARW, NEF, and DNG files, independently of sidecar JPEGs and display-preview caches. Preserve original JPEG bytes where possible and normalize orientation when required.
- Reload thumbnails when size, style, or source metadata changes, and prevent cancelled or superseded requests from publishing stale images.
- Initialize saved review history before accepting edits, including when no catalog is open. Provide explicit retry after a failed load and refresh ratings, filters, scoring, and selections after recovery.
- Correct tagging dates when rating existing photo records, while preserving them during scoring-only updates.
- Show Settings load/save errors with retry controls while retaining edits after a failed save.
- Add accessible slider names, values, and hints. Respect **Reduce Motion** throughout review interactions, including scrolling, transitions, thumbnail hover, and memory warnings.
- Bound concurrent metadata reads and coalesce scan progress updates. Release ready thumbnails individually before the entire catalog preload finishes.
- Cache filtered and ranked grid projections by mutation revisions, allowing selection-only changes to reuse existing projections. Restore saved sharpness scores in a single batch.

Focused Xcode regression suites passed for these changes. The October 15 full smoke run reported **388 passed, 11 failed, and 1 skipped**: nine failures required missing recorded AI fixtures, and two concerned release metadata/documentation checks. That run predates the latest version bump and October 16 keyboard changes; it is not a passing full-suite result for this snapshot. Manual VoiceOver and Reduce Motion checks, review-window keyboard checks, and Instruments measurements remain outstanding. The previously reported runtime crash has not been reproduced or diagnosed.

## Combined Review

- Open Combined Review from its own toolbar button, separate from the existing **SAM 3 + CLIP**, **Qwen Vision**, and **Objects** analysis modes.
- Review selected or tagged images in selections of **1–8 images per app review run**.
- Configure review criteria, source preference, and **Standard**, **Deep**, or **Exhaustive** review depth.
- Combine scene discovery, subject segmentation, identity handling, measurements, CLIP evidence, crop observations, and report synthesis in a bounded workflow.
- Keep findings tied to their source image, subject, region, and supporting evidence. Record conflicts, missing coverage, and unavailable evidence explicitly.
- Compare the reviewed selection while allowing abstention when the evidence cannot support a preference.
- Retain saved review results, run history, diagnostics, and optional input images and rejected responses for inspection. Provide cleanup controls for retained review data.

All model inference runs locally on the Mac. Results remain advisory: a completed review can still contain limitations or incomplete criterion coverage, and a model-generated statement is not an independently verified photographic fact.

## Photo-first and Evidence Workspaces

- Make **Photo-first** the default presentation, with the photograph, concise findings, coverage limits, and existing rating controls.
- Add **Inspect evidence** to open the finding's supporting region or crop in the **Evidence workspace**.
- Separate **Findings**, **Coverage & limits**, and the **Technical record**, including inspectable and copyable structured JSON.
- Preserve the selected photograph, region, and rating when switching between presentation modes.
- Add draggable column dividers and layouts that stack in narrower windows. Preserve image aspect ratios and keep the filmstrip compact.
- Keep saved reviews accessible outside the current input selection; enable rating controls only for files in that selection.
- Show an empty state for images without a review rather than displaying another image's findings. Keep report evidence available when retained images are missing.
- Distinguish the toolbar actions with `rectangle.badge.sparkles` for AI Analysis and `sparkles.rectangle.stack` for Combined Review, without the former plus badge.

## Memory Guidance in AI Settings

- Default Combined Review to **off** when no preference has been saved.
- Show this Mac's unified memory and a recommendation table before enabling the feature.
- Recommend **32 GB** for regular use, with additional headroom at 48 GB or more when other demanding apps are open.
- On Macs below **32 GB**, explain that Combined Review requires substantial memory and can take several minutes; recommend closing demanding apps and using high-quality previews.
- Require at least **16 GB** to enable Combined Review. On smaller Macs, show the guidance but leave the feature off after the message closes.
- Keep the feature off when the enablement sheet is cancelled. Turning it off cancels an active review while keeping saved results.

These memory recommendations are estimates, not measured peak-memory requirements for every image and model combination.

## AI Response Reliability and Diagnostics

- Tighten structured-response validation, crop and subject ownership, claim evidence, and focus consistency.
- Retry invalid Objects assessments with specific feedback. After the retry, abstain from unsupported focus judgments, including classifications returned with explicitly null focus evidence.
- Mark those focus judgments **uncertain**, remove dependent unsupported focus claims and preferences, and preserve supported information. Continue rejecting unrelated schema, ownership, and claim-evidence errors.
- Preserve bounded rejection diagnostics and available observations when report synthesis cannot complete; label observations separately from synthesized findings.
- Update `RunAIAnalysis.py` and `RunCombinedReviewSix.py` to require an explicit input directory and select all ARW files there by default. AI Analysis retains an optional positive file limit.
- Split larger Combined Review diagnostic selections into batches of up to eight images, with per-batch diagnostics and a top-level summary. Comparisons remain within each batch.
- Improve progress reporting and retain logs and evidence when a run fails.

## JPEG Extraction and Burst Groups

- Add an **Images** selector to the JPEG extraction sheet, offering **Selected images** and **Complete catalog**.
- Default to selected images when a selection exists; otherwise use the complete active catalog.
- Retain source-folder access until background extraction and embedded-preview cache warming finish.
- Add a **Review data** panel in Burst Groups showing preparation coverage for sharpness, CLIP indexing, Vision similarity, optional subject evidence, and burst grouping.
- Distinguish ready, partial, preparing, needed, unavailable, and empty-catalog states, and mark the inactive similarity backend **Not used**.
- Hide the idle semantic-search coverage message when all catalog photographs have compatible CLIP artifacts while keeping incomplete-coverage guidance visible.

## Catalog, Cache, and Internal Reliability

- Make **Abort** cancel pending catalog transitions as well as active loading, preventing cancelled or superseded transitions from restoring outdated selections.
- Publish asynchronous sort results only while the request, catalog, source, sort order, and search text still match.
- Retain catalog-folder access for background loading, thumbnails, JPEG extraction, previews, sharpness scoring, similarity indexing, and AI analysis, including work continuing after a catalog switch.
- Synchronize thumbnail storage, image counts, and memory-cost accounting. Prevent an older eviction callback from removing a replacement entry's accounting.
- Move catalog lifecycle coordination, saved-file presentation, and rsync output parsing into more focused components.

## Release Tooling and Verification

- Add `Scripts/release.sh` and Makefile commands for internal TestFlight and App Store Connect uploads, including dry-run support and explicit build numbers. Uploading does not submit the app for review or publish it.
- Update the Core AI and PhotoAIKit integrations, including PhotoAIKit 1.0.0 integration work, and advance source metadata to 3.3.3 Build 421.
- Expand regression coverage for review evidence ownership, response validation, presentation, memory thresholds, catalog cancellation, retained folder access, and cache accounting.
- On October 15, complete a local 12-ARW AI Analysis evaluation: **36 analysis steps, zero failures** after the focus-evidence fix.
- Complete a local 12-ARW Combined Review evaluation in batches of eight and four: **12 reports, all scheduled work items completed**, with engineering acceptance passing.

These evaluations establish the checks performed on that input selection. They do not establish general photographic accuracy, complete criterion coverage, or memory qualification across all Macs. The source verification notes retain those remaining limits.
