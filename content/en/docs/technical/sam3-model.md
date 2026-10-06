+++
title = "SAM 3 Model — Technical Detail"
linkTitle = "SAM 3 model"
weight = 60
categories = ["tech doc"]
tags = ["technical"]
description = "Technical documentation of RawCull analysis implementation and result computation."
+++

SAM 3 supplies prompted subject masks and object instances. RawCull's `CoreAISAM3Provider` adapts the Core AI segmentation runtime into PhotoAIKit contracts. The downstream sharpness calculation remains application-owned.

## Segmentation input and output

The provider receives an image and a subject or object-concept prompt. The runtime predicts segments with masks, scores, and spatial information. The provider uses a mask threshold of 0.5 and converts runtime results into image masks and normalized geometry.

For the subject contract, compatible segments are combined into an exhaustive union mask: a pixel is included when any returned segment includes it. The adapter also contains probability-mask decoding and feathering around the threshold. Unioning multiple instances means a subject mask can include several animals or people; it should not be assumed to identify one individual.

The returned confidence is derived from runtime output, with decoding fallbacks where applicable. It represents segmentation evidence. It is separate from geometric mask quality, pixel detail, and recommendation confidence.

## Object instances

The object contract retains individual masks instead of flattening them into a union. It validates mask dimensions and element counts, obtains a normalized box, and orders instances by descending score, then geometry, then original runtime order. Runtime masks use top-origin row-major coordinates; macOS box coordinates require vertical conversion.

In the Objects workflow, concepts come from manual queries or Qwen discovery. SAM 3 segments each concept. `ObjectInstanceDeduplicator` removes overlapping duplicate candidates before the application renders a review board with retained object IDs. Qwen then evaluates that board. Instance detection, deduplication, and language assessment are distinct stages, with separate timings and failure reporting.

## From mask to result

PhotoAIKit measures mask coverage and bounding box and assigns geometric quality. A repository can reuse a cached mask for the same source and prompt; model and source compatibility determine whether evidence remains useful. Prompt fallback can provide a usable full-subject mask when a more specific head/face request is unavailable, and the review result records that fallback.

RawCull computes detail inside the chosen mask using broad robust-tail energy, best local-patch evidence, and micro-contrast. See [Subject evidence](/docs/technical/subject-evidence/) for the exact `0.40 broad + 0.40 local + 0.20 fine` formula and background penalty. **SAM 3 does not return that sharpness score.** It determines the pixels on which the application measures detail.

A geometrically plausible mask can still select the wrong subject. A union mask can also include a sharp secondary subject while the intended subject is soft. The stored prompt, geometry, AF membership, and local-detail evidence help explain the result.

## Runtime boundary

The repository adapter exposes model loading, prompt submission, thresholding, decoding, and evidence contracts. The converted model's learned segmentation internals execute inside Core AI. These docs describe the observable implementation rather than claiming an application-specific equation for the neural network's confidence output.

## Source map

- `PhotoAIKit/Sources/CoreAISAM3Backend/CoreAISAM3Provider.swift`: subject union and object-instance adaptation.
- `PhotoAIKit/Sources/PhotoAIWorkflows/SubjectMaskSelection.swift` and `SubjectMaskQuality.swift`: acquisition and validation.
- `RawCull/RawCull/Intelligence/ObjectAnalysis/RawCullObjectAnalysisFeature.swift` and `ObjectInstanceDeduplicator.swift`: object workflow.
- `RawCull/RawCull/Intelligence/DeepReview/SubjectMaskFocusScorer.swift`: downstream detail measurement.
- `RawCull/ModelAssets/Notices/SAM3/PROVENANCE.json`: converted asset provenance.
