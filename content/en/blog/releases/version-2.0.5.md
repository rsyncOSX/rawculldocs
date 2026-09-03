+++
author = "Thomas Evensen"
title = "Version 2.0.5"
date = "2026-06-18"
tags = ["changelog","version 2.0.5"]
categories = ["changelog"]
+++

# RawCull Changelog — v2.0.2 → 2.0.5

Submitted for update on [Apple App Store](https://apps.apple.com/no/app/rawcull/id6759362764?mt=12).

 ------------------------------------------------------------------------------------------------------------------------------

 - 🖼️ Fixed orientation handling for embedded JPEG previews, sidecar JPEGs, cached previews, comparison view, and zoom previews.
- 🎯 Improved sharpness scoring and burst analysis so they can run on selected files or active star-rating filters instead of always processing the full catalog.
- ⚡ Refreshed full-size JPEG cache keys to avoid reusing older non-orientation-normalized previews.
- 🧹 Fixed empty catalog handling so loading state and security-scoped access are cleaned up correctly.
- ✨ Added an “Add Catalog” action when a selected folder contains no RAW files.
- 🧪 Added regression tests for orientation decoding, scoped sharpness/burst analysis, cache behavior, and empty catalog cleanup.
