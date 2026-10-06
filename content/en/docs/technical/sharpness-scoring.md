+++
title = "Sharpness Scoring — Technical Detail"
linkTitle = "Sharpness scoring"
weight = 10
categories = ["tech doc"]
tags = ["technical"]
description = "Technical documentation of RawCull analysis implementation and result computation."
+++

Sharpness is a deterministic measurement of spatial detail in the decoded analysis image. Vision supplies attention regions and optional labels; the numeric detail score comes from image processing in `PhotoAnalysisKit`.

## Image and edge-energy pipeline

The scorer normalizes input to 8-bit sRGB RGBA. It applies Gaussian pre-blur, then the Metal `focusLaplacian` kernel and a fixed scoring gain. Pre-blur suppresses noise before second-derivative energy is measured. Its effective radius is:

```text
radius = min(preBlurRadius × isoFactor × resolutionFactor × apertureBlurDamp, 100)
resolutionFactor = clamp(sqrt(max(longestSide, 512) / 512), 1, 3)
```

The ISO multiplier is 1 below ISO 800, rises linearly to 1.6 at ISO 3200, then rises to a cap of 2.2 at ISO 9600. The landscape aperture hint uses a blur damping factor of 0.8. Quality settings can blend a second, finer Laplacian pass: its pre-blur radius is `max(0.35, primaryRadiusSetting × 0.58)` and the blend weight is clamped to 0–0.65. This preserves small textured detail while retaining the normal noise suppression pass.

The energy image is rendered as floating-point RGBA. The scorer samples its red channel, excluding a configurable outer border to avoid edge artifacts. Preview resolution, decoding mode, ISO, aperture hints, and quality configuration affect the measurement; the score is not a camera-independent optical resolution metric.

## Robust-tail statistic

For a sample set of size `N`, percentile indices use `floor((N−1) × p)`. Let `p20`, `p90`, and `p97` be the corresponding energies. The score is:

```text
band = samples whose energy is between p90 and p97, inclusive
bandMean = mean(max(0, energy − p20)) over band
density = min(1, (band.count / N) / 0.06)
robustTail = bandMean × density
```

If `p97 <= p90`, or the band is empty, the result is `max(0, p90−p20)`. An empty sample set has no score. Subtracting p20 provides a background energy reference; excluding the highest tail reduces the influence of isolated extreme edges. The density factor attenuates sparse evidence.

Micro-contrast is the standard deviation of finite energy samples, computed as `sqrt(max(0, mean(x²)−mean(x)²))`. It measures variation in the processed detail signal, not exposure contrast in the original photograph.

## Regional measurements and final scalar

The breakdown retains whole-frame, selected saliency, AF-region, AF-center, and AF-neighborhood scores. Broad saliency and AF regions require at least 64 samples; the tighter AF center requires 16. Saliency selection favors overlap with the recorded AF location, then proximity, confidence, interior detail, and area.

When both broad AF and saliency measurements exist, the subject score begins as `0.60 × AF + 0.40 × saliency`. A local-detail estimate similarly combines the selected AF-neighborhood and subject-interior patches. When broad and local evidence both exist, the conservative subject score is `0.75 × broad + 0.25 × local`. Available evidence is used alone when the other component is absent.

The frame/subject blend is `(1−w) × global + w × subject`. Weight precedence is explicit override, aperture-hint override, then configured salient weight. If global evidence exists without any subject measurement, the fallback is `global × (1−w)³`.

Two further adjustments apply to the blend. A silhouette penalty starts when the outer 12% rim of the selected subject region dominates the interior: the implementation uses the ratio of rim mean to the sum of rim and interior means, with a threshold of 0.62. The penalty increases with excess dominance and the configured strength. A saliency-only subject-size bonus multiplies by `1 + normalizedArea × subjectSizeFactor`; an available AF region disables that bonus.

Finally, subject micro-contrast drives an aperture-dependent blur gate. Between the low and high configured thresholds the multiplier rises linearly from 0.20 to 1.0. With insufficient subject samples it is 1.0. This is a soft attenuation, so low-contrast subject detail does not encounter a hard rejection boundary.

## Calibration and interpretation

See [Focus mask](/docs/technical/focus-mask/) for the full overlay rendering pipeline, including local adaptive thresholds and morphology.

`FocusMaskCalibration` samples positive finite overlay-detail energies and selects a percentile threshold, default p90, clamped to 0.01–0.95. It requires at least five successful images by default. **This calibrates the visual focus-mask threshold only.** Scalar scoring uses `stableScoringEnergyMultiplier` and is independent of the catalog's calibration threshold.

The final scalar, AF-point measurement, focus-mask overlay, and [masked Deep Review score](/docs/technical/subject-evidence/) remain distinct results. A high whole-frame measurement can come from a detailed background; an AF coordinate records where focus was attempted, not proof that focus succeeded.

## Source map

- `PhotoAnalysisKit/Sources/PhotoAnalysisKit/FocusMaskEngine+Scoring.swift`: energy processing, statistics, regional selection, blending, and attenuation.
- `PhotoAnalysisKit/Sources/PhotoAnalysisKit/FocusMaskCalibration.swift`: overlay calibration.
- `PhotoAnalysisKit/Sources/PhotoAnalysisKit/SharpnessConfiguration.swift` and `SharpnessPresets.swift`: configuration and presets.
- `RawCull/RawCull/Model/ViewModels/FocusandSharpness/SharpnessScoringModel.swift`: application scoring workflow.
