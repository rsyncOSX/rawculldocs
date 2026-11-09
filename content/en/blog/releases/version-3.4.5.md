+++
author = "Thomas Evensen"
title = "Version 3.4.5"
date = "2026-11-09"
tags = ["changelog", "version 3.4.5"]
categories = ["changelog"]
+++

**In development.** RawCull 3.4.5 is planned for release in one to two weeks, around November 16–23, 2026.

The move from [3.3.6]({{< relref "version-3.3.6.md" >}}) to 3.4.5 reflects a substantial expansion of burst review. The new Close-ups panel and automatic face detection give you a dedicated way to inspect expressions, eye detail, and focus across near-identical frames before choosing which shots to keep.

## Compare close-ups across a burst

- Open **Close-ups** in the burst workspace to inspect the active frame, or select several frames in the filmstrip to compare their crops together.
- Set a reference region manually or use a recorded autofocus point when available. Use embedded previews or developed RAW for the crop comparison, and open a frame in Zoom View for a closer inspection.
- Open burst review from a specific frame in the culling grid and start on that frame.

## Automatically find faces and animal landmarks

- The Close-ups panel detects human faces and supported animal landmarks in embedded previews. It shows individual crops with a detail score when available, so you can inspect subjects separately when a frame contains more than one.
- Automatic detection pauses while the catalog is indexing and resumes afterward. RawCull caches face-detection results for reuse when you revisit frames.

## Preview and review refinements

- Zoom View shows RAW preparation progress in the source control and displays the current RAW detail level and render-request time in the overlay.
- Burst review leaves keyboard input with the focused control when you interact with it, avoiding unintended review shortcuts.
- Updated package dependencies and added regression coverage for close-up selection, face-evidence caching, and RAW previews.

These notes cover the changes from the `v3.3.6` tag through commit `56d3f6b5` (RawCull 3.4.5, build 436).
