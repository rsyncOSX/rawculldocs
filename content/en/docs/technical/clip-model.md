+++
title = "CLIP Model — Technical Detail"
linkTitle = "CLIP model"
weight = 50
categories = ["tech doc"]
tags = ["technical"]
description = "Technical documentation of RawCull analysis implementation and result computation."
+++

CLIP supplies image and text representations for visual similarity and semantic search. RawCull's Core AI provider runs separate image and text inference functions from a validated model bundle. It does not generate prose or measure optical focus.

## Inference inputs

`CoreAICLIPProvider` reads runtime configuration that specifies function names, tensor input/output names, image preprocessing, and tokenizer behavior. RawCull includes model assets for OpenAI CLIP and DataComp CLIP; selected bundle identity and configuration determine which representation is used.

Image preprocessing resamples the source and prepares RGB values in channel-first order. Channels are converted from bytes to 0–1 floats, then normalized:

```text
channelValue = (byte/255 − configuredMean[channel]) / configuredStdDev[channel]
```

The implementation supports preprocessing paths selected by configuration, including Pillow-compatible bicubic resizing followed by integer center crop. When the resized excess is odd, the crop origin uses integer division rather than rounding a half-pixel offset. Crop and interpolation details can change embeddings, which is why preprocessing version participates in artifact compatibility. The tensor size comes from the loaded model configuration and descriptor; it should not be assumed universal across bundles.

Text preprocessing uses the configured CLIP tokenizer or Hugging Face tokenizer JSON. Tokens are padded to the model context length; truncation preserves an end token. Models with an attention-mask input receive that mask. Some converted function signatures also require dummy inputs for the unused modality, which the provider constructs to satisfy the exported model contract.

## Outputs and calculations

The image function returns an embedding vector. Text inference validates a two-dimensional `[batch, dimension]` embedding output, supported float scalar type, and consistent element count. Model identity and backend accompany the representation.

CLIP's learned encoders map image and text inputs into a shared feature space. RawCull computes similarity from the returned vectors rather than reconstructing the encoders' internal layers. L2 normalization divides by vector magnitude, and image cosine distance is `1−cosineSimilarity`, clamped to 0–2. [Vision and CLIP indexes](/docs/technical/vision-clip-index/) describes compatibility and storage.

Semantic search encodes a description and compares it with indexed image representations from the same model. Ranking indicates relative correspondence to the description. A similarity score is not a calibrated probability that a label is true, and it does not establish sharpness, eye focus, or aesthetic quality.

## Use with subject review

CLIP supplies semantic evidence while SAM 3 supplies subject geometry. A semantic match and a mask can support the same workflow, but the numeric masked focus score is computed by RawCull's image-processing scorer. It is not the CLIP embedding distance. The current Deep Review pipeline retains prompt verification and fallback evidence alongside that masked score.

Apple Vision feature prints provide another visual-similarity backend; they do not substitute for CLIP's text encoder. Changing between OpenAI and DataComp bundles requires compatible indexing for the chosen model rather than combining their vectors.

## Source map

- `PhotoAIKit/Sources/CoreAICLIPBackend/CoreAICLIPProvider.swift`: model loading, tokenization, preprocessing, and tensor inference.
- `PhotoAIKit/Sources/CoreAICLIPBackend/CLIPRuntimeConfiguration.swift`: exported runtime configuration.
- `PhotoAIKit/Sources/PhotoAIContracts/ImageEmbedding.swift`: normalization and image cosine distance.
- `RawCull/ModelAssets/Notices/CLIP-OpenAI/` and `CLIP-DataComp/`: bundle provenance.
