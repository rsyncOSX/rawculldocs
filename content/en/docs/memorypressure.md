+++
author = "Thomas Evensen"
title = "Memory Pressure"
date = "2026-07-15"
weight = 91
tags = ["memory", "pressure", "cache"]
categories = ["user doc"]
+++

Large catalogs and full-size previews can use a lot of your Mac’s memory. macOS reports how much strain the system is under as memory pressure. RawCull uses that reading to adjust how many previews and thumbnails it keeps in memory.

| Level | RawCull response |
|---|---|
| Normal | Sizes the cache to fit the available memory |
| Warning | Reduces preview and grid cache limits |
| Critical | Clears its memory caches and uses smaller limits |

When pressure returns to normal, RawCull recalculates its normal cache limits. Source photos and saved ratings are not affected.

The **Memory** settings tab shows total and used memory, RawCull's memory use, and the current system pressure.

If warnings continue, close other memory-heavy apps, stop the current task with **Actions > Abort task** (`Command-K`), or work with a smaller catalog.
