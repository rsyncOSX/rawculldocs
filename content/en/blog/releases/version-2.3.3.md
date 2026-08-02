+++
author = "Thomas Evensen"
title = "Version 2.3.3"
date = "2026-08-01"
tags = ["changelog","version 2.3.3"]
categories = ["changelog"]
+++
 
# RawCull Changelog: v2.3.3 → Latest Commits 

 ------------------------------------------------------------------------------------------------------------------------------
  
# 📸 RawCull 2.3.3

Updated  on [Apple App Store](https://apps.apple.com/no/app/rawcull/id6759362764?mt=12).

Changes since version 2.3.2 through commit `e576ca0`.

## 🚀 Highlights

- Added durable per-file similarity artifacts, allowing Vision feature prints to be reused between sessions.
- Improved the Burst Groups workflow and similarity-threshold controls.
- Made the About window context-sensitive across special views.

## 🧠 Similarity Analysis

- Persist Vision feature prints individually instead of regenerating them for every burst analysis.
- Reuse compatible artifacts when reopening or rescanning a catalog.
- Generate only missing, outdated, or incompatible artifacts.
- Validate artifacts against the source path, file size, modification date, Vision revision, representation version, preview size, and pipeline version.
- Automatically migrate compatible artifacts from the previous catalog-wide cache.
- Store artifacts atomically and prune obsolete entries.
- Track generation and persistence failures separately.
- Distinguish between similarity generation and artifact-saving progress.

## 💥 Burst Groups

- Added an interactive similarity-threshold slider to the Burst Groups home view.
- Automatically regroup bursts after the threshold changes.
- Improved burst-analysis progress messages.
- Added guidance explaining that extracted thumbnails are shown by default and `J` switches to the embedded JPEG.
- Simplified the Burst Groups home interface and status presentation.
- Improved validation of cached burst results using the current similarity-artifact set.

## ⌨️ Context-Sensitive Shortcuts

- Made the About window observe the active RawCull workspace.
- Show only the relevant shortcut card when About is opened from:
  - Zoom Preview
  - Burst Groups
  - Burst Review
  - Manual Comparison
- Continue showing the complete shortcut reference in normal views.
- Added explanatory text describing the context-sensitive behavior.

## ⚙️ Cache Management

- Added a dedicated Similarity Artifact Cache entry to Settings.
- Display the cache size, artifact count, and storage location.
- Added an option to purge saved similarity artifacts independently.
- Kept derived burst-analysis results in their own cache.

## 🧪 Reliability and Documentation

- Added extensive tests for similarity-artifact persistence, invalidation, migration, cancellation, corruption handling, pruning, and concurrent access.
- Updated test-isolation support for disk-backed artifacts.
- Updated the README with the new similarity-analysis and caching architecture.
- Removed an obsolete merge document.

## 🔖 Version

- Updated the marketing version from `2.3.2` to `2.3.3`.
- Updated the build number from `229` to `230`.