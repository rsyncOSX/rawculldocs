+++
author = "Thomas Evensen"
title = "Version 1.8.0"
date = "2026-05-14"
tags = ["changelog","version 1.8.0"]
categories = ["changelog"]
+++

# RawCull Changelog — v1.7.4 → 1.8.0 

RawCull has received improvements to image comparison, zoom review, rating workflow, thumbnail responsiveness, cache reliability, sharpness/similarity behavior, and release/test tooling. 
Version 1.8.0 is submitted for update on Apple App Store.

## 🚀 v1.8.0

### 🖼️ New Comparison View

- Added a dedicated comparison view for reviewing selected images side by side.
- Supports comparing up to four selected thumbnails.
- Added keyboard navigation inside the comparison view.
- Added zoom and pan controls for closer inspection.
- Added focus mask and focus point overlays while comparing images.
- Added support for switching between thumbnail preview and extracted JPEG source.
- Added rating controls directly inside comparison mode.

### 🔍 Improved Zoom Review

- Improved the full-window zoom overlay experience.
- Added previous/next image navigation in zoom view.
- Added visible rating badge for the current image.
- Added keyboard shortcuts for zooming and switching image source.
- Added tests for zoom overlay navigation and behavior.

### ⚡ Faster and More Responsive Thumbnail Handling

- Improved cancellation of thumbnail and JPEG extraction work.
- Switching or cancelling catalogs should now feel more responsive because old ImageIO work is cancelled earlier.
- Sony and Nikon thumbnail/JPEG extraction now check for cancellation before expensive decode work.
- Improved thumbnail loader slot accounting so cancelled requests do not consume concurrency capacity.
- Added regression tests for thumbnail loader cancellation and concurrency behavior.

### 🎯 Sharpness and Similarity Reliability

- Similarity and burst actions now wait for in-progress sharpness scoring when needed.
- Prevents similarity/burst workflows from running before required sharpness scores are ready.
- Added test coverage for concurrent sharpness scoring behavior.

### 🧠 Cache and Memory Improvements

- Improved cache replacement accounting for memory and grid thumbnail caches.
- Improved test isolation around shared cache state.
- Added memory diagnostics checklist for validating large-catalog behavior.
- Improved cache-related concurrency tests.

### ⭐ Rating Workflow Improvements

- Added reusable rating controls and rating badge UI.
- Rating actions are now available in more review contexts, including comparison and zoom workflows.
- Ratings support reject, keeper, and 2-5 star values.

### 🛠️ Build and Test Tooling

- Added shared Xcode scheme.
- Added dedicated test plans:
  - Smoke
  - Full test suite
  - Performance
- Updated Makefile test commands.
- Improved documentation for test architecture and release validation.
- Pinned Swift Package dependencies to exact versions for more predictable builds.

### 📷 Camera and Image Extraction Notes

- Continued improvements to Sony ARW thumbnail extraction.
- Added cancellation-aware extraction paths for Sony and Nikon raw/JPEG preview handling.
- Nikon support remains experimental.

## 📝 Latest Commit After v1.8.0

### 📚 Documentation

- Updated README release information and project description.