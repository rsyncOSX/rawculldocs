+++
title = "Focus Mask — Technical Detail"
linkTitle = "Focus mask"
weight = 15
categories = ["tech doc"]
tags = ["technical", "focus mask"]
description = "Technical documentation of focus-mask edge detection, region selection, adaptive thresholds, and rendering."
+++

The focus mask renders spatial detail as a colored overlay. It answers where the decoded image contains strong edge energy. It shares low-level processing with [sharpness scoring](/docs/technical/sharpness-scoring/), but its thresholds and visual styling serve a different purpose from the scalar ranking score.

This page describes the RawCull source inspected on 6 October 2026. For controls and keyboard shortcuts, see the [Focus Mask user guide](/docs/focuspeaking/).

## Detail signal and image scale

`FocusMaskEngine` transforms the input image by the requested scale before computing detail. “Native pixels” here means pixels of that scaled analysis image, which may be an embedded preview rather than the camera's full RAW sensor data.

`buildFocusMaskDetail` sets the pre-blur parameter to `max(0.35, configuredPreBlurRadius × 0.52)` and calls the shared amplified Laplacian pipeline in native-mask mode. The effective Gaussian radius is:

```text
maskPreBlur = max(0.35, configuredPreBlurRadius × 0.52)
radius = min(maskPreBlur × isoFactor × apertureBlurDamp, 100)
```

Native-mask mode uses a resolution multiplier of 1, clamps input to its extent before blurring, and evaluates the kernel over the original extent. Clamping prevents the image boundary from introducing artificial detail. ISO and aperture damping follow the shared pipeline described in [sharpness scoring](/docs/technical/sharpness-scoring/).

The Metal kernel computes a 3×3 discrete Laplacian independently for each RGB channel, then combines absolute responses with Rec. 601 weights:

```text
Lchannel = 8 × centerChannel − sum(channel values of eight neighbors)
energy = 0.299 × abs(Lred) + 0.587 × abs(Lgreen) + 0.114 × abs(Lblue)
```

The scalar is packed into RGB, amplified by the configured energy multiplier, and retained as floating-point image data. Unlike scalar scoring's fixed gain, the mask uses the configured amplification setting. Texture, noise, JPEG sharpening, preview resolution, and blur settings all influence this signal.

## Choosing the visual region

When subject isolation is enabled, rendering can reuse the winning saliency rectangle and region from existing focus evidence. If a saliency rectangle is unavailable, the mask-only entry point can request attention saliency without classification and select a candidate using AF evidence.

Normalized AF coordinates are converted to Core Image coordinates using `y = 1−yAF`. AF squares and saliency rectangles are intersected with unit bounds, mapped into pixel coordinates, rounded outward to integral rectangles, and clipped to the image extent.

The visual region can be AF center, AF neighborhood, broad AF region, saliency, mixed AF and saliency, or global. The renderer honors a requested evidence region when its geometry exists; otherwise it falls back to broad AF, then saliency, then global. Disabling subject isolation explicitly chooses global edges.

Mixed mode searches both AF and saliency rectangles. These are rectangular regions, not SAM 3 pixel masks. A subject-isolated focus overlay therefore may contain background detail within the selected rectangle. [Subject evidence](/docs/technical/subject-evidence/) explains the separate segmented-subject scorer.

## Local patches and visible extent

The renderer ranks overlapping patches within the search regions. Patch width and height start at 34% of the corresponding region dimension, bounded between 3.5% and 14% of the full image dimension. Sampling steps are half the patch dimensions, with a minimum of one pixel. An AF-centered patch is added when sufficiently contained, and rendering also evaluates a patch spanning 6% of the image width and height around AF.

Patch evidence includes robust-tail detail, micro-contrast, coverage, AF distance, silhouette fraction, and shape heuristics. Selection retains up to three positive finite composite scores while excluding patches with overlap ratio >= 0.55 relative to the smaller patch. For AF-anchored evidence, the nearest patch can precede the strongest when the strongest is less than 1.15 times its composite score.

These patches summarize evidence. **They do not truncate the visible overlay:** threshold sampling and clipping use the complete search rectangles. Shape heuristics for compact or ring-like detail are not an eye detector or proof that the subject's eye is in focus.

## Adaptive threshold

The renderer sorts positive finite energies from the visual search regions. Its percentile uses `floor((N−1) × percentile)`. Let `T` be the configured threshold, which may have come from catalog calibration:

| Visual region | Percentile | Minimum floor | Cap percentile value at T |
|---|---|---|---|
| AF center, neighborhood, or broad AF | p82 | `max(0.32 × T, 0.01)` | Yes |
| Saliency, mixed, or global | p90 | `max(0.55 × T, 0.01)` | No |

The effective threshold is the selected percentile value, optionally capped at T, clamped between the floor and 0.95. If no positive finite samples exist, the result is the floor capped at 0.95.

The rendering path does not lower this threshold to satisfy a minimum visible coverage. Weak detail may produce an empty mask, and the recorded `relaxedForVisibility` flag is false. Although the source contains a visibility-relaxation helper, this rendering path does not call it.

Catalog calibration is another stage: it samples positive detail energies across images and supplies an overlay threshold, default p90. It does not recalibrate the fixed scalar sharpness gain. Local adaptive rendering then derives the effective threshold from this configured fallback and the visual region.

## Thresholding, morphology, and color

The renderer copies the energy's red channel into grayscale and applies `CIColorThreshold`. It optionally erodes the binary image with `CIMorphologyMinimum`, then dilates it with `CIMorphologyMaximum`. Erosion removes small or narrow features; dilation expands surviving highlights. Their order matters because dilation operates on the eroded result.

A color matrix maps the binary signal to orange-red RGB coefficients `(1.0, 0.22, 0.02)` and alpha coefficient `0.92`. The image is clipped to the union of search rectangles over a transparent background. Optional Gaussian feathering follows clipping, and the result is cropped to the analysis-image extent before becoming a `CGImage`. Feathering can soften highlights beyond a region boundary before the final image crop.

The raw-Laplacian diagnostic option bypasses thresholding, morphology, colorization, and subject-region clipping and returns the cropped amplified detail image. Its recorded region source still describes available selection geometry; it does not imply that the raw image was region-isolated.

## Coverage and diagnostics

Rendered coverage is measured after morphology and feathering. Alpha above 0.05 counts as visible. Core Image area-average reductions measure visible pixels within the union of search regions and divide by that union's area fraction. This is a fraction of the rendered region, not SAM 3 subject coverage and not a fraction of pixels proven optically sharp.

The breakdown records region source and effective threshold. Focus evidence also retains visualized region, overlay style, patch rankings, rendered coverage, and alignment/confidence diagnostics. The region source describes available saliency/AF geometry, while the visualized region identifies the actual evidence region used for rendering.

Core Image and Vision work runs in a cancellable background worker using an immutable configuration snapshot. Cancellation checks stop obsolete analysis and rendering. An empty overlay may indicate insufficient edge energy; failure or cancellation can instead return no image, so these cases should not be interpreted identically.

## Source map

- `PhotoAnalysisKit/Sources/PhotoAnalysisKit/FocusMaskEngine+MaskGeneration.swift`: region selection, patches, thresholds, morphology, colorization, and coverage.
- `PhotoAnalysisKit/Sources/PhotoAnalysisKit/Resources/Kernels.ci.metal`: 3×3 Laplacian and channel weighting.
- `PhotoAnalysisKit/Sources/PhotoAnalysisKit/FocusMaskEngine+Scoring.swift`: shared amplified detail pipeline and regional scoring.
- `PhotoAnalysisKit/Sources/PhotoAnalysisKit/FocusMaskCalibration.swift`: catalog overlay calibration.
- `PhotoAnalysisKit/Sources/PhotoAnalysisKit/FocusMaskTypes.swift`: region, patch, evidence, and diagnostic types.
- `RawCull/RawCull/Model/ViewModels/FocusandSharpness/FocusMaskModel.swift`: application-facing generation and calibration adapter.
