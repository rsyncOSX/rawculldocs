+++
author = "Thomas Evensen"
title = "AI Support in RawCull"
date = "2026-07-28"
weight = 1
aliases = ["/docs/ai/"]
tags = ["AI", "CLIP", "SAM 3", "semantic search", "similarity", "bursts"]
categories = ["user doc"]
+++

RawCull AI is planned for release when macOS 27 becomes publicly available. It adds local AI-assisted search and review while retaining the same culling workflow and non-AI functions as the current RawCull version.

The AI features run locally on Apple Silicon. RawCull does not upload photos to an external inference service.

## AI-Assisted Culling

RawCull uses two types of vision model:

- **CLIP** converts images and text into comparable vectors. RawCull uses those vectors for semantic search, visual similarity, and burst grouping.
- **SAM 3** locates the subject in an image. Deep Review uses the resulting subject mask together with sharpness and camera autofocus evidence to help compare frames.

AI results are review aids, not automatic decisions. Semantic-search scores describe relative similarity rather than confidence, and the photographer always makes the final selection.

![RawCull Burst Groups after AI-assisted similarity analysis, with semantic search available above the review workspace.](/images/ai/burstgroup.png)

## Similarity and CLIP

CLIP does not create captions, keywords, or fixed labels while indexing. It converts each image into a normalized numeric embedding that summarizes its overall visual content. RawCull stores that embedding locally and can reuse it for both similarity analysis and semantic search.

### Build the Similarity Index

Choose a CLIP model in **Settings > AI**, enable it for similarity, and select **Index Similarity** or **Re-index** in the burst workspace. Indexing runs the image encoder once for each photograph that does not already have a compatible cached embedding.

CLIP embeddings are specific to the selected model and its preprocessing configuration. Changing the CLIP model or installing an incompatible model version requires a new index. If CLIP is unavailable, RawCull can use the macOS Vision feature-print fallback for visual grouping, but text-based semantic search requires compatible CLIP embeddings.

### Group Similar Frames

Select **Analyze Bursts** after indexing. RawCull compares the cached image embeddings and groups visually related frames for review. The **Similarity** control adjusts how tightly frames are grouped: a lower value creates tighter groups, while a higher value admits more visually related frames.

The burst list shows each group and highlights its suggested pick. From here you can open the burst, run **Deep Review**, mark it reviewed, or defer it until later.

![CLIP-based Burst Groups showing related puffin and bird sequences, suggested picks, and review actions.](/images/ai/burstgroups.png)

### Review a Burst

Open a burst to inspect its frames in the filmstrip and compare them at a larger size. The review workspace combines the candidate rank with sharpness, focus-point, saliency, metadata, and subject evidence. You can navigate between frames, assign ratings, pick or reject a photograph, and return to the burst list when the review is complete.

The suggested pick and component scores are starting points, not final judgments. Check expression, pose, timing, framing, and critical focus before deciding which frame to keep.

![Review Burst workspace showing a puffin candidate, the burst filmstrip, ratings, and detailed scoring evidence.](/images/ai/reviewburst.png)

## Semantic Search

After a catalog has been indexed with a compatible CLIP model, enter a short description such as `puffin`, `raven`, or `squirrel`. RawCull ranks the catalog using the cached image embeddings, so later searches do not have to reprocess every image.

Search terms work best in English because the supported CLIP models were primarily trained and evaluated with English text.

### Puffin

![Semantic-search results for puffin, showing perched and flying puffins ranked together.](/images/ai/puffin.png)

### Raven

![Semantic-search results for raven, showing a burst of dark birds in flight.](/images/ai/raven.png)

### Squirrel

![Semantic-search results for squirrel, showing related frames ranked across the catalog.](/images/ai/squirrel.png)

## Supported Models

RawCull supports one selected CLIP model for similarity and semantic search, plus SAM 3 for subject-aware Deep Review.

| RawCull model | Purpose | Upstream model |
|---|---|---|
| OpenAI CLIP ViT-B/32 | Semantic search, similarity, and burst grouping | [OpenAI CLIP ViT-B/32 on Hugging Face](https://huggingface.co/openai/clip-vit-base-patch32) |
| OpenCLIP ViT-B/32 DataComp | Semantic search, similarity, and burst grouping | [DataComp `s34B-b86K` on Hugging Face](https://huggingface.co/laion/CLIP-ViT-B-32-256x256-DataComp-s34B-b86K) |
| Meta SAM 3 | Subject masks and Deep Review | [Meta SAM 3 on Hugging Face](https://huggingface.co/facebook/sam3) |

The upstream files on Hugging Face are the source models. RawCull requires model bundles converted and validated for Apple Core AI on macOS 27.

## Model Downloads

The AI models are **not included in the RawCull application or its release download**. Compatible model bundles will be available separately. Each RawCull AI release note will identify the supported model versions, download locations, installation steps, and expected checksums.

Follow the [RawCull release notes](/blog/releases/) rather than installing an arbitrary conversion. RawCull validates an installed bundle before enabling it and falls back safely when a required model is missing or invalid.

Keeping the models separate makes the application download smaller. It does
not remove the upstream model's licence conditions. Every model archive
distributed by RawCull must contain the complete applicable licence, model
provenance, conversion information, and checksums.

Before using **Download** or **Accept and Download** in RawCull, read
[AI Model Licences and Downloads](/docs/ai-in-rawcull/ailicences/).

## Model Licences

The RawCull application licence does not replace or extend the licences for the separately downloaded models.

- OpenAI's CLIP source repository is published under the [MIT License](https://github.com/openai/CLIP/blob/main/LICENSE).
- The DataComp model page identifies its licence as MIT. Its model card also documents the training data, intended uses, and limitations.
- SAM 3 is distributed under Meta's separate [SAM License](https://huggingface.co/facebook/sam3/blob/main/LICENSE), not the MIT License. Access to the official Hugging Face files may require signing in, sharing the requested contact information, and accepting Meta's terms.

Review the complete licence and model card shown by RawCull before downloading
or using a model. RawCull records the exact model revision and licence version
applicable to each converted bundle. If a model update changes its licence,
RawCull must present the new terms before downloading that update.

## macOS Support

RawCull AI requires macOS 27 and Apple Silicon and will be released when the public version of macOS 27 is available.

At that point, active RawCull development and support move to macOS 27. The existing macOS 26 version will no longer receive support or new features, but installed copies will continue to work with their current functionality.

The macOS 27 AI version retains the current RawCull feature set and workflows. AI support is additive: catalogs, ratings, culling, comparison, sharpness tools, previews, and copying continue to work as they do in the current version.
