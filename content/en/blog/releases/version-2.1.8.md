+++
author = "Thomas Evensen"
title = "Version 2.1.8"
date = "2026-06-22"
tags = ["changelog","version 2.1.8"]
categories = ["changelog"]
+++
 
# RawCull Changelog — v2.0.5 → 2.1.8

Updated on [Apple App Store](https://apps.apple.com/no/app/rawcull/id6759362764?mt=12).
 
 ------------------------------------------------------------------------------------------------------------------------------
 
### 🔍 Actual-Pixels Inspection

- Added `Z` as a fast inspection shortcut from grid, loupe, and comparison views.
- Opens the zoom overlay directly on the embedded JPEG at an actual-pixels zoom level.
- Centers the view around the AF focus point when one is available.
- Shows the focus-point marker on open for faster sharpness checking.

### ⭐ Faster Culling Flow

- Rating, reject, keep, and tag actions now advance automatically to the next image.
- The same advance behavior is available in thumbnail navigation, zoom overlay, and comparison grid workflows.
- Catalog loading now preselects the first visible file by name, so keyboard culling can begin immediately.

### 🧭 Orientation-Correct Previews

- Normalized source orientation for thumbnails, embedded previews, sharpened previews, comparison images, and zoom previews.
- Improved handling for Sony embedded previews that do not carry their own orientation metadata.
- Added corrected transform handling for EXIF orientation values, including rotated and mirrored images.

### 🧠 Adaptive Memory Handling

- Added adaptive preview and grid cache recommendations based on installed memory, current system memory use, and macOS memory pressure.
- Reduced cache budgets automatically when memory pressure is warning or critical.
- Updated default cache settings for different memory tiers.
- Simplified the memory warning banner and added a close button.

### 🪟 About Window

- Added a native About RawCull window.
- Replaced the standard app info command with RawCull's custom About window.
- Included app version/build information and a compact keystroke reference.

### 🗂️ Saved Files View Cleanup

- Split saved-files UI into smaller focused components for catalog rows, file rows, detail rows, section headers, and star ratings.
- Kept the saved-file detail presentation while making the view easier to maintain.
- Added a `FileRecord` convenience initializer for previews and tests.

### 🧪 Tests

- Added and expanded Swift Testing coverage for culling auto-advance, thumbnail/cache admission, zoom overlay key actions, comparison navigation, and thumbnail provider behavior.
- Added coverage for `Z` actual-pixels inspection in comparison navigation.

### 🛠️ Maintenance

- Aligned project versioning across build settings and release branches.
- Added a `test-performance` Makefile target.
- Performed cleanup in scoring parameter UI and cache-related code.
 