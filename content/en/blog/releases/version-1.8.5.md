+++
author = "Thomas Evensen"
title = "Version 1.8.5"
date = "2026-05-20"
tags = ["changelog","version 1.8.5"]
categories = ["changelog"]
+++

# RawCull Changelog — v1.8.1 → 1.8.5

## ✨ Smarter Burst Culling

RawCull now includes an intelligent burst analysis workflow to help identify the best frames in fast sequences.

- Groups visually similar burst shots automatically.
- Recommends the best frame using sharpness, subject/saliency, AF evidence, and metadata stability.
- Shows confidence levels: High, Medium, or Low.
- Adds burst evidence and caution notes so you can understand why a frame was recommended.
- Adds quick actions to Keep Best, Keep Top 2, Compare, or Undo.

## 🔍 Improved Burst Comparison

The comparison view now supports burst-specific review.

- Compare top burst candidates directly.
- See analysis evidence while reviewing.
- Use keyboard shortcuts for faster decisions.
- Escape returns from burst comparison back to the active burst group.
- Added keyboard controls for zoom, ratings, thumbnail source, focus mask, and focus points.

## 🛠 RAW Diagnostics

A new RAW diagnostics view helps troubleshoot camera-file parsing issues.

- Opens diagnostics for the selected RAW file with Command-I.
- Shows file identity, scan metadata, ImageIO details, parser traces, and embedded JPEG offsets.
- Reports Sony and Nikon AF focus parser stages with explicit error messages.
- Helps diagnose unsupported RAW layouts without relying on external tools.

## 📷 Better Support for Newer Sony RAW Files

RAW preview and JPEG extraction are more reliable for newer Sony ARW files.

- Improved support for ARW 6.0 / RA16-backed files such as A7V and A7R VI / ILCE-7RM6.
- Reads embedded JPEGs directly before ImageIO attempts unsupported RAW decoding.
- Detects full-size JpgFromRaw images stored in Sony SubIFD layouts.
- Uses the largest embedded JPEG for zoom/extraction when available.
- Bumped the full-size JPEG cache key to avoid stale cached results from older extraction logic.

## 🔎 Improved Nikon and Sony Parser Tracing

RAW parser internals are now easier to inspect and verify.

- Added diagnostic parser paths for Sony embedded JPEG locations and AF focus location.
- Added diagnostic parser paths for Nikon NEF embedded JPEG locations and AF focus location.
- Nikon AFInfo parsing now reports accepted/unsupported AFInfo versions and failed parsing stages.
- Parser location models are now Sendable-friendly for Swift 6 concurrency.

## 🏷 Better Thumbnail Labels

Sharpness badges are now easier to read.

- Replaced numeric-only sharpness scores with friendly labels: Sharp, Good, Check, Soft.
- Added burst recommendation badges such as BEST, BEST?, 2ND, Keeper, Top 2, and Rejected.
- Added accessibility labels and clearer help text.

## ⚡ Faster Repeat Analysis

Burst analysis results are now cached.

- Reopening the same catalog can reuse valid burst analysis data.
- Cached results include similarity data, sharpness scores, saliency info, burst groups, and recommendations.
- Cache validation checks catalog path, file count, file size, modification date, and analysis settings.

## 🧭 Zoom Overlay Improvements

The zoom overlay now better matches the current browsing mode.

- Shows sharpness and saliency badges inside the zoom overlay.
- Adds an in-overlay rating action bar.
- Adds keyboard shortcuts for ratings, zoom, JPEG/RAW source toggle, focus mask, and focus points.
- Uses horizontal navigation in grid-style views.
- Uses vertical navigation in loupe-style viewing.
- Navigation icons and help text now reflect the active direction.

## 📁 Safer Saved State Location

RawCull now stores saved culling state in Application Support instead of the user’s Documents folder.

- `savedfiles.json` now lives under `~/Library/Application Support/RawCull/`.
- This keeps app state out of the user’s document workspace and avoids unnecessary document/iCloud clutter.

## 📤 More Reliable Copy Operations

Copy/export handling is more robust.

- Security-scoped folder access is now cleaned up reliably.
- Copy cancellation and sheet dismissal now close active rsync work properly.
- Progress callbacks now hop safely back to the main actor.
- Removed an invalid empty environment entry from rsync process setup.

## 🧪 Test Suite Cleanup and Coverage

The test suite was reorganized and expanded around current app behavior.

- Added tests for burst analysis.
- Added tests for culling model behavior.
- Added tests for disk cache and scan admission.
- Added tests for file sorting and EXIF formatting.
- Added more Sony MakerNote parser coverage.
- Added Nikon MakerNote parser diagnostics coverage.
- Added RAW diagnostics tests.
- Added A7R VI-style Sony embedded JPEG extraction tests.
- Added zoom overlay and comparison grid keyboard shortcut tests.
- Added thumbnail cancellation coverage.
- Removed older duplicated or obsolete verification/test documents.