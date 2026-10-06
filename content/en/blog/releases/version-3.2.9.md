+++
author = "Thomas Evensen"
title = "Version 3.2.9 - In Development"
date = "2026-10-06"
tags = ["changelog", "version 3.2.9"]
categories = ["changelog"]
summary = "In development, not yet released: catalog-wide JPEG extraction, Burst Groups preparation status, and catalog and cache reliability improvements."
+++

**RawCull 3.2.9 is in development and has not yet been released.**

Catalog-wide JPEG extraction, Burst Groups preparation status, and catalog and cache reliability improvements.

<!--more-->

**Changes:** v3.2.8 → v3.2.9 (in development)

This changelog continues from the [3.2.8 release notes]({{< relref "version-3.2.8.md" >}}) and covers source changes since the `v3.2.8` tag through October 6, 2026. It is a development snapshot; features and build numbers may change before release.

<div class="alert alert-warning" role="alert">

**In development - not yet released.** RawCull 3.2.9 adds a complete-catalog option for JPEG extraction and shows preparation coverage in Burst Groups. It also strengthens catalog cancellation, asynchronous sorting, background folder access, and thumbnail-cache accounting. This page does not announce App Store, TestFlight, or downloadable release availability.

</div>

RawCull requires macOS 27 (Golden Gate) and an Apple Silicon Mac. Building from source requires Xcode 27.

**Commit range:** `v3.2.8..da251c4`  
**Baseline tag:** `v3.2.8` at `2b04aa6` (2026-09-30), RawCull 3.2.8 Build 398  
**Latest commit:** `da251c4` (`Align release documentation and metadata tests with 3.2.9`, 2026-10-06)  
**Current source version:** RawCull 3.2.9 (Build 403)  
**Release status:** In development; not yet released

## JPEG Extraction

- Add an **Images** selector to the JPEG extraction sheet, with **Selected images** and **Complete catalog** options.
- Default to selected images when a selection exists; otherwise use the complete active catalog.
- Use the chosen scope for extraction and the sheet's source summary.
- Retain source-folder access while background JPEG extraction and embedded-preview cache warming finish.

## Burst Groups Preparation Status

- Add a **Review data** panel to the Burst Groups home view.
- Show coverage counts and preparation state for sharpness, the CLIP index, Vision similarity data, optional subject evidence, and burst grouping.
- Distinguish ready, partial, preparing, needed, unavailable, and empty-catalog states.
- Mark the inactive similarity backend as **Not used**.
- Hide the idle semantic-search coverage message when all catalog photographs have compatible CLIP artifacts; keep incomplete-coverage guidance visible.

## Catalog Cancellation and Sorting

- Make **Abort** cancel pending catalog transitions as well as active catalog loading.
- Reject cancelled or superseded transitions after persistence finishes, preventing a delayed transition from starting a catalog load or restoring an outdated selection.
- Publish asynchronous sort results only when the request, catalog, selected source, sort order, and search text still match.
- Invalidate pending sorts when catalog loading is cancelled and keep sorting activity tied to the current request.

## Background Folder Access

- Retain catalog access for the lifetime of background file operations, including catalog loading, thumbnail creation, JPEG extraction, full-size previews, sharpness scoring, similarity indexing, and AI analysis.
- Keep existing work's folder access alive when the selected catalog changes, then release it when that work finishes.
- Strengthen shared catalog-access bookkeeping for overlapping operations.

## Thumbnail Cache Reliability

- Synchronize thumbnail storage changes with cached-image count and memory-cost accounting.
- Handle replacement and eviction callbacks using entry identities so an older entry's eviction cannot remove a newer entry's accounting.
- Give each tracked cache ownership of its eviction delegate and detach the delegate during teardown.
- Clear storage and accounting together when resetting the cache.

## Release Tooling and Dependencies

- Add `Scripts/release.sh` and Makefile commands for internal TestFlight and App Store Connect uploads, with dry-run support and optional explicit build numbers.
- Preserve archives and export diagnostics under `build/releases/`. These upload commands do not submit for App Review or publish the app.
- Advance app and model-downloader metadata to version 3.2.9, Build 403, and align release-metadata tests and documentation.
- Update PhotoAIKit to revision `648ea75a1c6bf511e03e879100dc149e4cccd022` and Apple Core AI models to revision `52c84ba874b2c57adcede08a671ce96ed1b3f433`.
- Update Swift Hugging Face from 0.11.0 to 0.12.0.

## Regression Coverage

- Add tests for aborted and superseded catalog transitions and stale asynchronous sort results.
- Add tests for retained catalog access across overlapping operations and Qwen analysis cancellation.
- Add concurrent cache-accounting tests and complete-catalog JPEG extraction selection coverage.

These entries describe checked-in test coverage, not test results from preparing this changelog. No application tests, release uploads, or Hugo build were run while preparing this page.
