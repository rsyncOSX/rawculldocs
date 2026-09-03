+++
author = "Thomas Evensen"
title = "Version 1.7.4"
date = "2026-05-10"
tags = ["changelog","version 1.7.4"]
categories = ["changelog"]
+++

# RawCull Changelog — v1.7.0 → 1.7.4

Version 1.7.4 is submitted for update on the Apple App Store. It focuses on faster zoom previews, better cache management, and more reliable culling-data saving.

## ✨ New

- **Full-size JPG disk cache.** The zoom view now caches the full-resolution embedded JPEG to disk (`~/Library/Caches/no.blogspot.RawCull/FullsizeJPGs/`), so re-opening a previously zoomed photo is instant instead of re-extracting from the ARW.
- **"Cache JPGs" button** in the catalog sidebar lets you pre-warm the full-size JPG cache for an entire catalog in one go (uses a new `ScanAndExtractJPGs` actor with progress + ETA).
- **JPG cache controls in Settings → Cache.** A new section shows the on-disk size of the full-size JPG cache and adds a Prune action (with confirmation).

## 🔄 Changed

- **Unified culling grid.** The standard grid and the Similarity/Burst grid now share one `CullingGridView`. Selection, scroll-to-selection, rating filter, the "N selected" status, and the three progress overlays now behave identically in both views.
- **Redesigned Scan Stats sheet** — clearer layout for the per-catalog statistics.
- **Smoother progress ETA.** The "Estimated time to completion" label is now "Estimated time left", and the countdown no longer jitters upward when a slow file briefly inflates the average.
- **Cleaner toolbar mode switching.** Loupe / Grid / Similarity / Rated buttons go through a single `selectMainViewMode(...)` helper for consistent state reset.
- **Saved-files persistence moved out of the main view model** into a dedicated `CullingModel`, with debounced JSON writes (350 ms) to avoid hammering disk on rapid rating changes.

## 📦 Under the hood

- New concurrency tests for `SharedMemoryCache` and `FocusMaskModel`; revisions to thumbnail-cache eviction (`CacheDelegate`, `CachedThumbnail`) for safer cross-actor access.
- Catalog load/cancel path reworked — switching catalogs now reliably cancels in-flight scan, thumbnail, JPG-extract, and sharpness/similarity work.
- Image extractors (`SaveJPGImage`, `JPGSonyARWExtractor`, `JPGNikonNEFExtractor`) tightened.
