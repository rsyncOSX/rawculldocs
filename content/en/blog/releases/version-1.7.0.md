+++
author = "Thomas Evensen"
title = "Version 1.7.0"
date = "2026-05-06"
tags = ["changelog","version 1.7.0"]
categories = ["changelog"]
+++

# RawCull Changelog — v1.6.9 → 1.7.0

Version 1.7.0 is submitted for update on Apple App Store. RawCull 1.7.0 focuses on faster zoom previews, better cache management, and more reliable culling-data saving.
 
## ✨ Highlights
 
 - **Faster zoom preview loading** with a new full-size JPG disk cache.
 - **Better cache controls** in Settings, including cache size visibility and manual JPG cache pruning.
 - **More reliable culling persistence** when rating, batch-rating, and saving sharpness results.
 - **Improved diagnostics** for understanding memory pressure and cache evictions.
 
## ✨ What’s new
 
### Full-size JPG disk cache for zoom previews
 
 RawCull now caches extracted full-size JPEG previews on disk. Reopening zoomed images is faster, and the app avoids repeating the same  extraction work for files you have already inspected.
 
### JPG cache controls in Settings

The Cache settings screen now shows the size of the full-size JPG cache and adds a dedicated **Prune JPG Cache** action, making it easier to  reclaim disk space when needed.
 
## 🔄 Improvements
 
### More accurate cache estimates

Cache estimates in Settings are now based on live cache usage when available, giving a more realistic idea of how many images fit in memory  and in the grid cache.
 
### More consistent culling grid behavior

The standard grid and similarity grid now share the same core grid implementation. This improves consistency across:
 - multi-selection
 - keyboard-driven rating
 - burst-group display
 - progress overlays
 - scroll-to-selection behavior
 
### More reliable rating and scoring saves

Rating updates, batch rating changes, threshold-based culling, and persisted sharpness/saliency results now go through a centralized culling  model with coalesced saves. This reduces the risk of stale or out-of-order writes during fast culling sessions.
 
### Safer persistence pipeline

Saved-file models are now concurrency-safe, and JSON writes are serialized through a shared writer to improve stability.
 
## ⚡ Diagnostics and Cache Monitoring
 
### Richer memory diagnostics

Memory diagnostics now break out evictions by cache type, making it easier to see whether pressure is coming from the main memory cache or  the grid cache.
 
### Better cache visibility

RawCull now exposes more current cache usage details in Settings, including memory cache and grid cache counts and sizes.
 
## 📦 Under the Hood
 
 - Internal cache-management cleanup to support the new zoom-preview cache.
 - Concurrency and isolation fixes in supporting code paths.
 - Documentation and internal review notes updated during the 1.7.0 cycle.
