+++
author = "Thomas Evensen"
title = "Cache"
date = "2026-07-15"
weight = 90
tags = ["memory", "cache", "performance"]
categories = ["user doc"]
lastmod = "2026-10-25"
+++

RawCull caches previews to make reopening and scrolling through a catalog faster.

It keeps previews and grid thumbnails in memory and stores thumbnails and full-size JPG previews, including developed RAW variants, on disk. If an item is missing, RawCull reads it from the original RAW file and rebuilds it.

Select **Cache JPGs** in Loupe view to prepare missing full-size embedded previews for the current catalog. This can make later zooming and comparison more responsive.

Open **RawCull > Settings > Cache** to see memory previews, grid thumbnails, disk thumbnails, full-size JPGs, similarity artifacts, burst groups, and saved Combined Reviews.

**Delete Cache** asks for confirmation, then removes memory and disk caches, similarity artifacts, burst analysis results, and saved Combined Reviews, including masks, retained inputs, and review history. Preview caches can be rebuilt and analysis can be rerun. Original photos, settings, and downloaded AI models are kept.

**Delete Saved Files** is a separate action that deletes saved catalog records, ratings, and analysis results after confirmation. Original photos are kept. Do not use it merely to free preview-cache space.

Memory limits adapt to the Mac's available unified memory. Under memory pressure, RawCull reduces or clears memory caches automatically. See [Memory Pressure](/docs/memorypressure/).
