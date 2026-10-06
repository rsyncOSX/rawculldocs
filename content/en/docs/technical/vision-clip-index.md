+++
title = "Vision and CLIP Indexes — Technical Detail"
linkTitle = "Vision and CLIP indexes"
weight = 20
categories = ["tech doc"]
tags = ["technical"]
description = "Technical documentation of RawCull analysis implementation and result computation."
+++

RawCull supports two different visual representations: Apple Vision feature prints and CLIP embeddings. An index stores reusable representations keyed to photographs, avoiding repeated inference for every comparison. The representations are not interchangeable.

## Vision feature prints

`VisionFeaturePrintBackend` creates a `VNGenerateImageFeaturePrintRequest`, using revision 2 by default. Vision produces a `VNFeaturePrintObservation`; the provider securely archives that observation as the artifact payload. Callers receive a typed artifact rather than the Vision object itself.

Comparison securely decodes both observations and calls `computeDistance`. A lower distance indicates greater visual similarity. The provider verifies compatible descriptors and rejects non-finite distances. Apple's feature representation and distance internals are framework-owned: RawCull does not implement an explicit cosine formula for these observations.

The descriptor records `vision-feature-print`, the request revision, archive representation version, and framework-managed preprocessing/normalization identifiers. There is no public embedding dimension in this artifact. Vision feature prints do not provide a text encoder, so a Vision index alone cannot implement CLIP semantic text search.

## CLIP vector indexes

CLIP produces numeric image embeddings and matching text embeddings. Image vectors are L2-normalized where configured. For compatible image vectors `a` and `b`, the contract computes:

```text
cosineDistance = clamp(1 − dot(a,b) / (norm(a) × norm(b)), 0, 2)
```

Comparison requires matching backend, model identity, nonempty vectors of equal length, and positive magnitudes. Invalid or incompatible comparisons return no distance. Identical directions give distance 0; orthogonal directions give 1. This scale must not be interpreted as a calibrated probability or reused as a Vision distance scale.

Image-to-image comparisons support visual similarity. Text-to-image comparisons support semantic search using the selected model's shared representation. See [CLIP model](/docs/technical/clip-model/) for tensor preprocessing and inference.

## Compatibility and freshness

`SimilarityArtifactDescriptor` separates the source fingerprint from backend identity. Distance compatibility checks the representation and backend configuration fields, including model fingerprint, dimensions, preprocessing version, normalization version, and configuration version. Two different photos may be compared when those fields agree; their source fingerprints are expected to differ.

The source fingerprint describes the input used to create an artifact. Index reuse must also establish that the artifact still belongs to the current source. A model or preprocessing change can require rebuilding representations even if the photograph is unchanged. Mixing CLIP model variants produces incompatible vectors even when their dimensions happen to match.

## Index execution and fallback

`SimilarityArtifactIndexer` decodes images and asks a provider to generate artifacts using bounded concurrency, default two tasks. It returns successful artifacts, per-source failures, and whether whole-batch fallback occurred. It checks cancellation and reports completed item counts.

Fallback is an explicit policy: none, per-item, or whole-batch. Whole-batch fallback reruns all sources with the fallback provider if the initial pass has failures. Per-item fallback can yield representations from different backends, but compatibility checks still prevent cross-backend distance calculation. The existence of a package fallback policy does not imply that every application workflow enables it.

Burst grouping consumes adjacent-file distances rather than comparing every possible pair. Missing similarity evidence creates a boundary in the grouping engine. This prevents an unmeasured pair from being treated as a confident match.

## Source map

- `PhotoAIKit/Sources/VisionFeaturePrintBackend/VisionFeaturePrintBackend.swift`: generation, secure archive, and native distance.
- `PhotoAIKit/Sources/PhotoAIContracts/SimilarityArtifact.swift` and `ImageEmbedding.swift`: representation compatibility and cosine distance.
- `PhotoAIKit/Sources/PhotoAIWorkflows/SimilarityArtifactIndexer.swift`: indexing and fallback policies.
- `RawCull/RawCull/Intelligence/Similarity/RawCullSimilarityFeature.swift` and `RawCullVisionSimilarityService.swift`: application integrations.
