+++
title = "Technical Documentation"
linkTitle = "Technical"
weight = 80
categories = ["tech doc"]
tags = ["technical"]
description = "Technical documentation of RawCull analysis implementation and result computation."
+++

These **tech docs** describe how RawCull computes analysis results, stores evidence, and turns measurements into review recommendations. They are intended for readers who want implementation detail beyond the user guides.

The reference is the local RawCull source inspected on **6 October 2026**, primarily the `RawCull/RawCull` application, `RawCullCore`, `PhotoAnalysisKit`, and `PhotoAIKit`. Defaults and algorithms describe that source snapshot; they do not establish which features are available in an App Store release. RawCullBrowse and RawCullFB have separate integrations and should not be assumed to behave identically.

## Technical index

| Type | Technical theme | Contents |
|---|---|---|
| Tech doc | [Sharpness scoring](/docs/technical/sharpness-scoring/) | Laplacian energy, robust statistics, regional blending, and calibration |
| Tech doc | [Focus mask](/docs/technical/focus-mask/) | Native-pixel detail, region selection, adaptive thresholds, and overlay rendering |
| Tech doc | [Vision and CLIP indexes](/docs/technical/vision-clip-index/) | Representations, distance calculations, indexing, and compatibility |
| Tech doc | [Subject evidence](/docs/technical/subject-evidence/) | Saliency, autofocus, masks, local detail, and Deep Review confidence |
| Tech doc | [Burst groups](/docs/technical/burst-groups/) | Boundaries, metadata checks, ranking weights, and recommendation confidence |
| Tech doc | [CLIP model](/docs/technical/clip-model/) | Image/text inference, preprocessing, semantic search, and model identities |
| Tech doc | [SAM 3 model](/docs/technical/sam3-model/) | Prompted segmentation, union masks, object instances, and downstream scoring |
| Tech doc | [Qwen Vision model](/docs/technical/qwen-model/) | Vision-language generation, structured scores, and object assessment |

The three principal AI model families are **CLIP, SAM 3, and Qwen Vision**. The AI tabs combine them: SAM 3 + CLIP uses segmentation and semantic evidence, Qwen Vision performs image assessment, and Objects combines SAM 3 instances with Qwen. Apple Vision also supplies feature prints, attention saliency, and classification. EfficientSAM has a separate provider in the source tree; it is not one of the three families documented here.

## How the stages connect

1. Decode a photograph into an analysis image and read available camera metadata.
2. Measure sharpness and retain regional focus evidence.
3. Index visual representations and compare adjacent photographs.
4. Create burst boundaries and rank candidates using deterministic rules.
5. Run optional subject-mask or vision-language review on a smaller candidate set.

A similarity distance, a sharpness scalar, a segmentation confidence, and a generated assessment score have different meanings. Their scales cannot be interchanged. The pages below identify the computation and its limits at each stage.

For operating instructions, start with [Sharpness Scoring](/docs/sharpness/), [Similarity, Bursts, and Search](/docs/similarity/), and [AI Step by Step](/docs/ai/aistepbystep/).
