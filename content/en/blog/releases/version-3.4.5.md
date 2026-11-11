+++
author = "Thomas Evensen"
title = "Version 3.4.5"
date = "2026-11-09"
tags = ["changelog", "version 3.4.5"]
categories = ["changelog"]
+++

**In development.** RawCull 3.4.5 is planned for release around November 16–23, 2026.

RawCull 3.4.5 expands burst review with a **Close-ups** panel and an experimental **People Ranking** option. Compared with [3.3.6]({{< relref "version-3.3.6.md" >}}), you can inspect expressions, eye detail, and focus across near-identical frames and try a face-detail-based ordering for bursts of one person.

## Compare close-ups across a burst

- Open **Close-ups** in the burst workspace to inspect the active frame. Command-click individual frames or Shift-click a range in the filmstrip to compare them. Use **Jump to image** to go directly to a selected frame’s detected crops.
- Set a reference region manually or use a recorded autofocus point when available. Use embedded previews or developed RAW for the crop comparison, and open a frame in Zoom View for a closer inspection.
- Open burst review from a specific frame in the culling grid to start on that frame.

## Inspect detected faces and animal landmarks

- Close-ups uses embedded previews to detect human faces and, where supported, animal heads and eyes. Animal crops are experimental and available for visual inspection.
- Each frame reports whether its face is ready for comparison, no face or multiple faces were detected, or face detail is unavailable. You can retry a failed check.
- Automatic detection pauses while the catalog is indexing and resumes afterward. RawCull reuses cached face measurements and keeps detected animal landmarks in memory for the current app session.

## Try People Ranking

- Use **Apply People Ranking** for a burst with at least two frames showing the same person. RawCull checks the whole burst for one measurable face per frame, matching embedded preview dimensions, and comparable face sizes. If a check fails, RawCull explains why and keeps the existing order.
- People Ranking adjusts **Best Frame** order using measured face detail. Either of the first two frames in the original order can move to the top. The filmstrip separately marks the frame with the highest measured face detail across the burst as **Most face detail**.
- Use **Restore Best Frame Order** to return to the original order. People Ranking changes the displayed order only.

## Preview and catalog refinements

- Zoom View shows RAW preparation progress in the source control and displays the current RAW detail level and render-request time in the overlay.
- Burst review leaves keyboard input with the focused control, avoiding unintended review shortcuts.
- The catalog sidebar offers an **Add Catalog** button when no catalog is selected or the selected folder contains no RAW images.
- Updated package dependencies and added regression coverage for close-up selection, face-evidence caching, and RAW previews.

These notes cover the changes from the `v3.3.6` tag through commit `8bd3e840` (RawCull 3.4.5, build 440).
