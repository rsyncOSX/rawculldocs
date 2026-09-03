+++
author = "Thomas Evensen"
title = "Memory Pressure"
date = "2026-07-15"
weight = 31
tags = ["memory", "pressure", "cache"]
categories = ["user doc"]
+++

Large catalogs and high-resolution previews can use substantial unified memory. RawCull monitors the macOS memory-pressure level and adjusts its caches automatically.

| Level | RawCull response |
|---|---|
| Normal | Uses adaptive cache limits based on available memory |
| Warning | Reduces preview and grid cache limits |
| Critical | Clears memory caches and keeps a small working limit |

When pressure returns to normal, RawCull recalculates its normal cache limits. Source photos and saved ratings are not affected.

The **Memory** settings tab shows total and used memory, RawCull's memory use, and the current system pressure.

If warnings continue, close other memory-heavy apps, stop the current task with **Actions -> Abort task** (`Command-K`), or work with a smaller catalog.
