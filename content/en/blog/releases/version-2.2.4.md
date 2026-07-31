+++
author = "Thomas Evensen"
title = "Version 2.2.4"
date = "2026-07-03"
tags = ["changelog","version 2.2.4"]
categories = ["changelog"]
+++
 
# RawCull Changelog: v2.2.1 → Latest Commits 

Updated  on [Apple App Store](https://apps.apple.com/no/app/rawcull/id6759362764?mt=12).
 ------------------------------------------------------------------------------------------------------------------------------
  
## 🖼️ JPG Export Improvements

- Added a new **Extract JPGs** sheet.
- JPG extraction now works from the current selection:
  - multiple selected thumbnails
  - or the single active image when no multi-selection exists
- Added destination folder selection for JPG export.
- Added export mode picker:
  - **Embedded JPG**
  - **Demosaiced RAW**
- Demosaiced exports are saved with a `_demosaic.jpg` suffix.
- Added extraction progress overlay in the main view.

## 📷 RAW Loading Refactor

- Introduced a shared `RawImageLoading` abstraction.
- Centralized metadata, thumbnail, and preview loading through `RawParserKit`.
- Added `FullSizePreviewLoader` for reusable full-size embedded preview loading.
- Sidecar `.jpg` files are preferred before extracting from RAW.
- Full-size preview extraction now reuses disk cache when possible.
- Removed older local thumbnail and preview loading paths.

## ⚡ Performance and Concurrency

- Added a shared batch extraction concurrency limit based on CPU count.
- Thumbnail creation, JPG extraction, and full-size preview loading now share more consistent loading paths.
- Improved cancellation checks during preview and extraction work.
- Reduced duplicated image-loading logic across zoom, comparison, thumbnail, and scan flows.

## 🧩 RawParserKit Update

- Updated `RawParserKit` from **1.1.0** to **1.2.5**.
- Migrated RawCull code to use newer RawParserKit APIs for:
  - EXIF metadata
  - focus point metadata
  - thumbnail generation
  - embedded preview extraction
  - full-size JPEG and demosaic export support

## 🔍 Scanning and Metadata

- `ScanFiles` now uses the shared raw loader for EXIF and Sony focus-point metadata.
- Removed local EXIF parsing helpers from RawCull.
- Focus point normalization now comes from RawParserKit metadata.
- Security-scoped folder access handling was made more tolerant by only stopping access when it was actually started.

## 🧪 Tests

- Added tests for JPG extraction selection behavior.
- Added tests for JPG output naming.
- Added integration-style tests for:
  - thumbnail request cache promotion
  - full-size preview disk caching
  - sidecar JPG preference
  - cancellation behavior
  - scan metadata loading through `RawImageLoading`
- Updated existing tests for current MainActor isolation needs.

## 🛠️ Tooling and Maintenance

- Added a dedicated `RawCullPeriphery` Xcode scheme.
- Updated `.periphery.yml` to use the new scheme and exclude tests.
- Removed obsolete local helper files:
  - `SupportedFileType.swift`
  - `ThumbnailError.swift`
  - `OrientationNormalizedImageLoader.swift`

## 📝 Documentation and Review Notes

- Moved issue tracking into `doc/issues.md`.
- Added a deep code review report and later re-verification notes.
- Updated README latest-release text to show `v2.2.1` as the App Store release.

## 📦 Commit Range Summary

- From: `v2.2.1`
- To: latest commit `e46f76d` on `version-2.2.5`
- Total: **17 commits**
- Diff size: **47 files changed**, **1352 insertions**, **1122 deletions**