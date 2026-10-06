+++
title = "Subject Evidence — Technical Detail"
linkTitle = "Subject evidence"
weight = 30
categories = ["tech doc"]
tags = ["technical"]
description = "Technical documentation of RawCull analysis implementation and result computation."
+++

Subject evidence tells RawCull where a measurement came from and how trustworthy that region is. It combines camera metadata, Vision attention regions, optional classification, segmentation geometry, and measured interior detail. These signals answer different questions and retain separate provenance.

## Camera AF and Vision saliency

A normalized camera AF coordinate records where autofocus was attempted. For Vision saliency comparisons the scorer flips the vertical coordinate with `yVision = 1−yAF`. Incorrect coordinate conventions would select a different region of the photograph.

`VNGenerateAttentionBasedSaliencyImageRequest` supplies candidate bounding boxes. A candidate survives when its normalized area exceeds 0.03 or its confidence is at least 0.9. Selection prioritizes AF overlap, distance to the AF point, saliency confidence, measured detail, area, and a deterministic geometric tie break. This is attention-based region selection, not pixel segmentation.

Optional `VNClassifyImageRequest` supplies a whole-image label. The label-selection code first looks for subject-related keywords at confidence 0.06 or higher, then accepts non-environment labels at 0.15 or higher. The stored saliency summary's `subjectConfidence` comes from the maximum salient-object confidence; it should not be read as the classification label's posterior probability.

The sharpness breakdown also retains AF-center and neighborhood evidence, local scoring patches, the selected region, and selection reason. See [sharpness scoring](/docs/technical/sharpness-scoring/) for the numeric blends.

## Segmentation acquisition and quality

`SubjectMaskSelector` checks a repository for a cached mask, then optionally generates one. Its package defaults try subject, person, bird, and animal prompts, stop at the first acceptable result, and require at least warning-level geometry. RawCull's Deep Review chooses its own prompt sequence according to Auto, Full Subject, or Head/Face presets and the subject label.

Mask geometry describes normalized coverage and bounding box. Quality is poor for an empty box, coverage at or below 0.005, or coverage at or above 0.90. A good mask has fresh geometry, coverage in 0.02–0.70, and no box edge within 0.02 of an image edge; other usable masks receive a warning. These checks measure geometric plausibility, not semantic correctness. Selection records attempts, confidence, cache origin, and whether minimum quality was met.

## Masked detail measurement

`SubjectMaskFocusScorer` resizes the mask to the analysis image and converts image RGB into luminance:

```text
Y = (0.2126R + 0.7152G + 0.0722B) / 255
energy = abs(4Y − Yleft − Yright − Yup − Ydown)
```

It excludes a border of `max(2, min(width,height)/250)` pixels. A pixel belongs to the subject when mask alpha exceeds 16 on the 0–255 scale. Coverage is the counted subject pixels divided by the full image pixel count.

Broad detail is the robust-tail statistic over subject energies; fine detail is their standard deviation. For local evidence, the image is divided into a 6×6 grid. A patch needs at least `max(64, 0.08 × nominalPatchArea)` masked samples. Each valid patch scores `robustTail + 0.35 × microContrast`; the best patch supplies local detail.

```text
maskedScore = 0.40 × broad + 0.40 × (local, or broad if local is absent)
              + 0.20 × fine
```

If whole-image robust-tail energy exceeds `max(maskedScore × 1.45, maskedScore + 0.04)`, the scorer applies a background-dominance multiplier of 0.82. It records whether a local patch was usable, whether AF lies inside the mask, and whether that penalty applied. AF membership is evidence; it is not an extra numeric bonus in this formula.

This scorer uses a luminance Laplacian directly. It is a separate computation from the pre-blurred Metal pipeline used by ordinary sharpness, so their raw scalars should not be compared as though they share a calibration.

## Deep Review result and confidence

The application stores the masked final score as `deepScore`, alongside ordinary sharpness, prompt, mask confidence, coverage, local detail, fallback status, and issues. Groups of more than 12 input candidates are narrowed to eight for detailed review; otherwise all candidates are considered.

Deep Review confidence uses the relative lead `(firstScore−secondScore)/max(firstScore,1e−6)`. High confidence requires a lead of at least 0.12, a mask prompt, local detail, no issues, and no fallback mask. A lead of at least 0.05, or strong evidence with a fallback mask, gives medium confidence. Other cases give low confidence. This rule differs from the absolute score gaps in ordinary burst ranking.

## Source map

- `PhotoAnalysisKit/Sources/PhotoAnalysisKit/FocusMaskEngine+Scoring.swift`: attention, classification, and AF region handling.
- `PhotoAIKit/Sources/PhotoAIWorkflows/SubjectMaskSelection.swift`, `SubjectMaskGeometry.swift`, and `SubjectMaskQuality.swift`: acquisition and geometry checks.
- `RawCull/RawCull/Intelligence/DeepReview/SubjectMaskFocusScorer.swift`: masked numeric detail.
- `RawCull/RawCull/Intelligence/DeepReview/DeepAIReviewFeature.swift`: candidate selection, evidence, and confidence.
