+++
author = "Thomas Evensen"
title = "Cache"
date = "2026-07-15"
weight = 90
tags = ["memory", "cache", "performance"]
categories = ["user doc"]
lastmod = "2026-10-25"
+++

RawCull saves copies of previews in a cache so catalogs reopen and scroll faster.

RawCull keeps previews and thumbnails in memory while you work. It also saves thumbnails and full-size JPG previews on disk, including previews made from RAW data. If a cached image is missing, RawCull makes it again from the original file.

Select **Cache JPGs** in Loupe view to prepare missing full-size embedded previews for the current catalog. This can make later zooming and comparison more responsive.

Open **RawCull > Settings > Cache** to see memory previews, grid thumbnails, disk thumbnails, full-size JPGs, saved similarity data, burst groups, and saved Combined Reviews.

**Delete Cache** asks for confirmation, then removes memory and disk caches, saved similarity data, burst analysis results, and saved Combined Reviews, including masks, saved images and crops used by the models, and review history. Preview caches can be rebuilt and analysis can be rerun. Original photos, settings, and downloaded AI models are kept.

**Delete Saved Files** is a separate action that deletes saved catalog records, ratings, and analysis results after confirmation. Original photos are kept. Do not use it merely to free preview-cache space.

When your Mac runs low on memory, RawCull automatically shrinks or clears its memory caches. See [Memory Pressure](/docs/memorypressure/).
