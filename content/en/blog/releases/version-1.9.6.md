+++
author = "Thomas Evensen"
title = "Version 1.9.6"
date = "2026-06-04"
tags = ["changelog","version 1.9.6"]
categories = ["changelog"]
+++

# RawCull Changelog — v1.9.4 → 1.9.6

Released on [Apple App Store](https://apps.apple.com/no/app/rawcull/id6759362764?mt=12).

### 🔄 Comparison view is now a horizontal filmstrip

The burst comparison layout was completely redesigned.

- Candidates are now presented in a side-by-side horizontal scroll, one full-width pane per candidate, instead of a vertical grid stack.
- Navigation is left/right only — up/down arrow keys are removed. The view snaps one candidate at a time using SwiftUI scrollTargetBehavior(.viewAligned).
- Selecting a file in the filmstrip updates the main selection and vice-versa, keeping both views in sync automatically.
- The burst header bar is more compact: decision text and action buttons share a single line with tooltips replacing verbose inline descriptions.

----------------------------------------------------------------------------------------------------------------------------------------------------

### 🔍 Loupe view can now toggle the extracted JPG

The loupe (single-image zoom) view gains a built-in extracted-JPG mode so you can compare the embedded thumbnail against the full-size sidecar or extracted JPEG without opening a separate window.

- Press J (or j) to toggle between the embedded thumbnail and the extracted JPG. Zoom (+/-) continues to work in both modes.
- A LoupeImageKeyAction enum centralises key resolution (+, -, j/J), replacing the inline switch on key characters and making key handling unit-testable.
- When the extracted JPG is active, a ProgressView is shown while loading and a "No extracted JPG" placeholder appears if no sidecar or embedded preview is available. The task is cancelled and cleaned up when the image changes or the view disappears.
- ZoomPreviewHandler.loadExtractedJPGPreview is extracted as a reusable static method (sidecar JPG → RAM cache → raw extraction pipeline), shared between the loupe toggle and the existing zoom-window open path.
- The focus overlay and focus mask both track currentImageSize / currentDisplayedImage, so they remain correctly anchored whether the thumbnail or the extracted JPG is displayed.
- The image-source toggle in ImageOverlayControlsView is now wired to showExtractedJPG, replacing the previously constant .constant(false) binding.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### 🏷 Rating badge now visible on every thumbnail view

CurrentRatingBadgeView is now shown as an overlay on ImageItemView, RatedImageItemView, and the loupe MainThumbnailImageView.

- A new density parameter (.regular / .compact) shrinks the badge's font, icon, and padding for use inside small thumbnails where screen space is limited.
- In ImageItemView the rating badge and the burst-recommendation badge are stacked vertically in the top-left corner.
- RatedImageItemView extracts its rating lookup into a shared ratingValue computed property, eliminating duplicated savedFiles traversal.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### 🎯 Scoring signatures now version-stamp every result

Sharpness scores are now stamped with a SharpnessScoringSignature that captures the exact algorithm version, ISO scaling policy, aperture hint policy, and all scoring parameters used to produce the score.

- Saved file records (savedfiles.json) store the scoring signature, file size, and modification date alongside the score, so RawCull can detect when a cached score is stale.
- A SharpnessScoringSignature carries explicit algorithm version, ISO policy version, and aperture hint policy version fields, making it straightforward to invalidate old scores when the pipeline changes.
- The burst cache schema advances to version 2 to match the new signature layout.
- BurstSharpnessSignature is now a typealias for SharpnessScoringSignature (no migration needed for existing burst caches).

----------------------------------------------------------------------------------------------------------------------------------------------------

### 🩺 Candidate inspector shows richer scoring evidence

The inspector panel shown when reviewing burst candidates now surfaces much more of the internal scoring data.

- New fields: Source, Blur Gate sigma, Subject Label, Subject Confidence, Focus Flag, Mask Region, and Mask Threshold in the Scoring section.
- New evidence fields: Scoring Local Detail, AF Center Detail, AF Neighborhood Detail, Scoring AF Patch, Scoring Subject Patch, Saliency Candidates, Saliency Selection reason, Evidence Threshold, Evidence Coverage, and Visibility Relaxed.

----------------------------------------------------------------------------------------------------------------------------------------------------

### 🧩 Saliency selection is now multi-candidate with AF-point guidance

Vision saliency results are no longer collapsed into a single union rectangle.

- Each detected object is now a SaliencyCandidate with its own rect and confidence.
- A dedicated selectSaliencyCandidate function picks the winning region by weighing AF-point overlap, distance to AF point, local detail score, and area — preferring the subject the camera actually focused on.
- The selection reason and candidate count are recorded in the breakdown and shown in the inspector.

----------------------------------------------------------------------------------------------------------------------------------------------------

### 🖼 Focus mask gains visibility relaxation

The focus mask overlay now actively checks whether the selected threshold would produce a nearly invisible mask.

- When coverage falls below a configurable minimum, the threshold is automatically relaxed until the overlay is visible, and relaxedForVisibility = true is recorded in the breakdown.
- Overlay styles are renamed from subjectHeat/globalDetail to subjectEdges/globalEdges to better reflect what they show.

----------------------------------------------------------------------------------------------------------------------------------------------------

### ⚡ Cancellation guards throughout the scoring pipeline

All heavy per-frame work checks Task.isCancelled at every major stage, so switching images quickly no longer queues up stale work.

- A shared runCancellableWorker helper on FocusMaskEngine wraps detached tasks with a proper withTaskCancellationHandler so cancellation propagates immediately.
- Calibration, saliency, Laplacian sampling, mask generation, and scoring all return early when cancelled.

----------------------------------------------------------------------------------------------------------------------------------------------------

### 📐 Calibration now samples pixel energies, not scores

The auto-calibration pass that sets the visual edge threshold was redesigned.

- Instead of scoring every file end-to-end, calibration now decodes a small 512-px thumbnail, runs only the Laplacian, and collects raw pixel energy samples.
- This makes calibration faster and ensures the core sharpness score no longer depends on catalog contents — only the visual threshold changes per catalog.

----------------------------------------------------------------------------------------------------------------------------------------------------

### 🎛 Compact overlay controls in comparison panes

All image overlay control capsules (focus mask, focus points, ratings, image source) now accept a density parameter.

- .compact density shrinks button sizes, padding, and font sizes for use inside the comparison pane where screen space is limited.
- Focus peaking (FocusPeakingControlsView and the G keyboard shortcut) has been removed from the zoom overlay.

----------------------------------------------------------------------------------------------------------------------------------------------------

### 🧪 New tests

- resolution scaling uses longest side regardless of orientation
- scoring size normalization clamps legacy and oversized values
- conservative subject score keeps broad score dominant
- conservative subject score preserves broad fallback without patches
- saliency selection prefers AF overlap over confidence
- saliency selection is deterministic without AF
- saliency selection reports empty fallback
- cancellable worker forwards cancellation (thread-safety, 1 min limit)
- visibility relaxation lowers threshold and reports coverage
- generic decode normalization produces sRGB eight bit RGBA
- scoring signature ignores visual controls and tracks score controls
- scoring signature invalidates for every score-affecting field and policy version
- applying visual calibration does not change scoring signature
- zoom shortcuts resolve from characters
- jpg source shortcuts resolve from lowercase and uppercase j
- unmapped keys are ignored

