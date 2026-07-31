+++
author = "Thomas Evensen"
title = "Version 2.3.2"
date = "2026-07-23"
tags = ["changelog","version 2.3.2"]
categories = ["changelog"]
+++
 
# RawCull Changelog: v2.3.2 → Latest Commits 

 ------------------------------------------------------------------------------------------------------------------------------
  
# 📸 RawCull 2.3.2

Updated  on [Apple App Store](https://apps.apple.com/no/app/rawcull/id6759362764?mt=12).

Changes since version 2.3.0.

## 🎞️ Improved Burst Review

- Burst frames are now consistently ordered by analysis rank, placing the strongest candidates first.
- Added detailed candidate information directly to the burst-review inspector.
- Sharpness, saliency, autofocus, rating, and recommendation evidence are easier to compare.
- Simplified the burst-review sidebar by removing redundant actions.
- Improved navigation between burst frames and back to the burst group.
- Added clearer review and rating controls.

## ⚡ Faster Frame Navigation

- The selected burst frame and its neighboring frames are now preloaded.
- Decoded images and focus-analysis results are cached separately for each preview source.
- Switching between thumbnail, embedded JPEG, and developed RAW sources is more efficient.
- Focus masks are regenerated only when the focus configuration changes.
- Improved cancellation handling when rapidly navigating between images.

## 🔍 Find Similar Images

- Added a new “Find Similar” control to the grid toolbar.
- Select an image to rank the catalog by visual similarity.
- Similarity sorting becomes available after burst analysis is complete.
- Sharpness and similarity sorting are now mutually exclusive for clearer results.

## 🕓 Capture-Time Metadata

- Added support for the original RAW capture date and timezone offset.
- Burst analysis now orders images by their actual capture time instead of filename.
- File modification time is used automatically when capture metadata is unavailable.
- Added the capture date to the File Inspector.
- Added exposure-compensation information to the File Inspector.
- Burst grouping now uses richer exposure and focal-length metadata when evaluating group boundaries.

## 🛠️ Reliability and Maintenance

- Updated RawParserKit to version 1.2.8.
- Updated RawCullCore to version 1.1.2.
- Improved RAW metadata diagnostics.
- Expanded tests for capture metadata, burst ordering, frame caching, and navigation.
- Updated the application version to 2.3.1.
