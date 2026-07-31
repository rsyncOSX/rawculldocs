+++
author = "Thomas Evensen"
title = "Cache"
date = "2026-07-15"
weight = 30
tags = ["memory", "cache", "performance"]
categories = ["user doc"]
+++

RawCull caches previews so reopening and scrolling through a catalog is faster.

It uses separate memory caches for previews and grid thumbnails, plus disk caches for thumbnails and full-size embedded JPG previews. If an item is not cached, RawCull reads it from the original RAW file and caches the result.

Select **Cache JPGs** in Loupe view to prepare missing full-size embedded previews for the current catalog. This can make later zooming and comparison more responsive.

Open **RawCull -> Settings -> Cache** to see current cache use. **Clear Disk Cache** removes thumbnail files, and **Clear JPG Cache** removes full-size preview files. Both caches are rebuilt as needed; clearing them does not change source photos, ratings, or exported JPGs.

Memory limits adapt to the Mac's available unified memory. Under memory pressure, RawCull reduces or clears memory caches automatically. See [Memory Pressure](/docs/memorypressure/).
