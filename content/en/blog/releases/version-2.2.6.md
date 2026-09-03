+++
author = "Thomas Evensen"
title = "Version 2.2.6"
date = "2026-07-13"
tags = ["changelog","version 2.2.6"]
categories = ["changelog"]
+++

# RawCull Changelog: v2.2.4 → v2.2.6

 ------------------------------------------------------------------------------------------------------------------------------

## 🚀 Version 2.2.6

Updated on the [Apple App Store](https://apps.apple.com/no/app/rawcull/id6759362764?mt=12).

### 🗂️ Redesigned Burst Groups

- Added a dedicated Burst Groups home screen.
- Organized burst tools and review categories into a clearer workflow.
- Added compact burst cards with improved visual separation.
- Added Clean View, showing only the highest-ranked frames from each burst.
- Added inline expand and collapse controls with hidden-frame counts.
- Improved Reviewed and Deferred state handling.
- Reviewed bursts now collapse automatically while deferred bursts remain available.

### 🔍 Dedicated Burst Review Workspace

- Added a focused workspace for reviewing one burst at a time.
- Added a large image stage, filmstrip navigation, file information, ratings, tags, and burst actions.
- Added controls for marking bursts reviewed, deferring decisions, comparing candidates, and setting a manual pick.
- Added quick navigation between frames and burst groups.
- Improved JPEG, RAW, focus-mask, and AF-point inspection during burst review.

### ⌨️ Expanded Keyboard Controls

- Added `P` and `N` for previous and next burst frames.
- Added `G` to advance to the next burst group.
- Added arrow-key navigation throughout burst review.
- Added keyboard access to zoom, image sources, focus overlays, ratings, picks, and rejection.
- Added `B` for keeping the best candidate on the burst comparison screen.
- Clarified that `P` means previous frame during burst review; use `0` to keep a frame neutral.

### 🖼️ Improved Culling Grid

- Improved burst-group presentation inside the main culling grid.
- Added clearer burst headers and direct group actions.
- Improved selection behavior while working with grouped images.
- Improved transitions between the burst home, grouped grid, review workspace, and comparison screen.
- Simplified toolbar controls for burst-oriented workflows.

### ℹ️ New About and Shortcut Guide

- Redesigned the About window with adaptive, color-coded cards.
- Grouped shortcuts by Browse, Burst Groups, Image Preview, Zoom Preview, Manual Comparison, Burst Review, and App Commands.
- Added visual keycaps and contextual shortcut descriptions.
- Added accessibility labels for shortcut sections and commands.
- Made the About window scrollable and resizable.

### 🧪 Reliability and Testing

- Added tests for burst keyboard navigation.
- Added tests for grouped-grid selection and burst actions.
- Added coverage for rating behavior and burst-review state transitions.

---

## 🛠️ Version 2.2.5

### 🛡️ Safer File Copying

- Prevented failed folder selections from silently reusing an older security-scoped bookmark.
- Added stronger source and destination validation before starting a copy.
- Replaced the shared include-list file with a unique file for each copy operation.
- Prevented rsync from starting when the selected-file list cannot be created.
- Improved handling of filenames containing rsync filter characters.
- Added cleanup for temporary copy-operation files.
- Improved error reporting when copy preparation fails.

### 🖼️ Culling and Burst Workflow Improvements

- Improved grouped-image presentation in the culling grid.
- Added clearer separation between individual burst groups.
- Added collapsible burst groups and compact ranked-frame presentation.
- Improved Reviewed, Deferred, and manual burst-decision state handling.
- Improved multi-selection and primary-selection consistency.
- Improved rating-filter consistency between the toolbar, visible grid, and keyboard navigation.

### 🔬 Comparison and Focus Analysis

- Added per-photo zoom, pan, focus-mask, and AF-point state in comparison views.
- Prevented interaction state from leaking between comparison candidates.
- Improved comparison scrolling and selection synchronization.
- Debounced comparison selection updates during horizontal scrolling.
- Improved protection against stale comparison image refreshes.
- Corrected sharpness sampling around expanded Gaussian blur boundaries.
- Updated RawParserKit integration and RAW diagnostic handling.

### ⚡ Performance and Concurrency

- Added culling-grid render caching to reduce repeated grouping and lookup work.
- Improved cancellation and stale-result protection for asynchronous operations.
- Improved thumbnail and embedded-JPEG extraction task handling.
- Improved similarity-ranking and catalog-state synchronization.
- Added additional cancellation checks around focus-mask and sharpness processing.
- Improved progress-state ownership for extraction and cache-warming operations.

### 💾 Ratings and Persistence

- Improved saved-rating lookup and update performance.
- Corrected rating persistence during multi-photo operations.
- Improved extraction selection based on tagged and rated photos.
- Improved state restoration when reopening catalogs.
- Improved JSON persistence behavior and reduced unnecessary work on the main actor.

### 🧰 Settings and Diagnostics

- Improved settings reset and save behavior.
- Improved cache-setting validation.
- Moved additional RAW diagnostic work away from the main UI path.
- Improved memory and progress diagnostics.

### 🧪 Expanded Test Coverage

- Added copy-startup and file-list safety tests.
- Added comparison display-state tests.
- Added culling-grid selection and coordination tests.
- Added extraction-selection tests.
- Expanded rating, cache, RAW loading, and sharpness-scoring tests.
