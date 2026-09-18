+++
author = "Thomas Evensen"
title = "Cache"
date = "2026-07-15"
weight = 30
tags = ["memory", "cache", "performance"]
categories = ["user doc"]
+++

RawCull caches previews to make reopening and scrolling through a catalog faster.

It keeps previews and grid thumbnails in memory and stores thumbnails and full-size embedded JPG previews on disk. If an item is missing, RawCull reads it from the original RAW file and rebuilds it.

Select **Cache JPGs** in Loupe view to prepare missing full-size embedded previews for the current catalog. This can make later zooming and comparison more responsive.

Open **RawCull > Settings > Cache** to see current cache use. **Clear Disk Cache** removes thumbnail files, and **Clear JPG Cache** removes full-size preview files. RawCull rebuilds both as needed. Clearing them does not change source photos, ratings, or exported JPGs.

Memory limits adapt to the Mac's available unified memory. Under memory pressure, RawCull reduces or clears memory caches automatically. See [Memory Pressure](/docs/memorypressure/).
