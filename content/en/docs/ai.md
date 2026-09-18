+++
author = "Thomas Evensen"
title = "AI Support in RawCull"
date = "2026-07-28"
weight = 1
aliases = ["/docs/ai/"]
tags = ["AI", "CLIP", "SAM 3", "Qwen", "semantic search", "similarity", "bursts"]
categories = ["user doc"]
+++

RawCull requires **macOS 27 Golden Gate** and **Apple Silicon** mac.

RawCull supports local AI-assisted search and review while retaining the same culling workflow and non-AI functions as the current RawCull version. The AI features run locally on Apple Silicon. RawCull does not upload photos to an external inference service.

## AI-Assisted Culling

RawCull uses three vision models:

- **DataComp CLIP** converts images and text into comparable vectors. RawCull uses those vectors to index every ARW file in a catalog for semantic search, visual similarity, and burst grouping.
- **SAM 3** locates the subject in selected images. Deep Review uses the resulting subject mask together with sharpness, CLIP, and camera autofocus evidence to help compare frames.
- **Qwen3-VL** performs a deeper vision-language assessment of selected images against editable criteria, returning structured scores, strengths, possible problems, confidence, and subject details.

CLIP is compact and fast enough to index all ARW files in a catalog. SAM 3 and Qwen are substantially larger and require more computation, so RawCull reserves them for deeper analysis of photographs you select rather than running them across the complete catalog.

AI results are review aids, not automatic decisions. Semantic-search scores describe relative similarity rather than confidence, and the photographer always makes the final selection.

## Similarity and DataComp CLIP

DataComp CLIP does not create captions, keywords, or fixed labels while indexing. It converts each image into a normalized numeric embedding that summarizes its overall visual content. RawCull stores that embedding locally and can reuse it for both similarity analysis and semantic search.

### Build the Similarity Index

Enable DataComp CLIP in **Settings > AI**, then select **Index Similarity** or **Re-index** in the burst workspace. Indexing runs the image encoder once for each ARW photograph that does not already have a compatible cached embedding. This catalog-wide index supports both similarity and semantic search without invoking the larger SAM 3 or Qwen models.

CLIP embeddings are specific to the model and its preprocessing configuration. Installing an incompatible model version requires a new index. Similarity and semantic search require compatible DataComp CLIP embeddings.

### Group Similar Frames

Select **Analyze Bursts** after indexing. RawCull compares the cached image embeddings and groups visually related frames for review. The **Similarity** control adjusts how tightly frames are grouped: a lower value creates tighter groups, while a higher value admits more visually related frames.

The burst list shows each group and highlights its suggested pick. From here you can open the burst, mark it reviewed, or defer it until later. For deeper model-based analysis, select photographs in Grid View and open **AI Analysis**.

### Review a Burst

Open a burst to inspect its frames in the filmstrip and compare them at a larger size. The review workspace combines the candidate rank with sharpness, focus-point, saliency, metadata, and subject evidence. You can navigate between frames, assign ratings, pick or reject a photograph, and return to the burst list when the review is complete.

The suggested pick and component scores are starting points, not final judgments. Check expression, pose, timing, framing, and critical focus before deciding which frame to keep.

## Deeper Analysis of Selected Images

Select photographs in Grid View, or choose photographs rated two stars and higher, then open **AI Analysis**. This focused workflow avoids the time and computational cost of running the larger models on every ARW file in the catalog.

- **SAM 3 + CLIP** isolates the subject and combines subject-aware detail, sharpness, autofocus, and coverage evidence to rank the selected photographs and recommend a frame.
- **Qwen** evaluates each selected photograph against editable criteria such as composition, exposure, subject visibility, expression, and obstructions. It returns an advisory assessment with scores, strengths, possible problems, confidence, and subject details.

Both analysis modes run locally on the Mac. Their results are review aids; confirm the findings against the photographs before making culling decisions.

## Semantic Search

After a catalog has been indexed with DataComp CLIP, enter a short description such as `puffin`, `raven`, or `squirrel`. RawCull ranks the catalog using the cached image embeddings, so later searches do not have to reprocess every image.

Search terms work best in English because DataComp CLIP was primarily trained and evaluated with English text.

## Supported Models

RawCull supports DataComp CLIP for catalog-wide similarity and semantic-search indexing, plus SAM 3 and Qwen3-VL for deeper analysis of selected images.

| RawCull model | Purpose | Upstream model |
|---|---|---|
| OpenCLIP ViT-B/32 DataComp | Semantic search, similarity, and burst grouping | [DataComp `s34B-b86K` on Hugging Face](https://huggingface.co/laion/CLIP-ViT-B-32-256x256-DataComp-s34B-b86K) |
| Meta SAM 3 | Subject masks and Deep Review | [Meta SAM 3 on Hugging Face](https://huggingface.co/facebook/sam3) |
| Qwen3-VL-2B-Instruct | Criteria-based vision-language assessment | [Qwen3-VL-2B-Instruct on Hugging Face](https://huggingface.co/Qwen/Qwen3-VL-2B-Instruct) |

The upstream files on Hugging Face are the source models. RawCull requires model bundles converted and validated for Apple Core AI on macOS 27.

## Model Downloads

The AI models are **not included in the RawCull application or its release
download**. Open **Settings > AI** and select **Download AI Models** to view the
available models, their purpose, publisher, version, licence, and installation
status.

Downloads use macOS Managed Background Assets. macOS stores and manages each
asset pack, and its location can change between app launches. RawCull validates
an installed model before enabling it and falls back safely when a required
model is missing or invalid.

Review the licence in the model manager before selecting **Download**. Progress
and cancellation are shown in the same window. After installation, the models
run locally; photographs are not uploaded as part of downloading or using a
model.

Keeping the models separate makes the application download smaller. It does
not remove the upstream model's licence conditions. Follow the [RawCull release
notes](/blog/releases/) for the model versions supported by each beta or
release rather than installing an arbitrary conversion.

## Model Licences

The RawCull application licence does not replace or extend the licences for the separately downloaded models.

- The DataComp model page identifies its licence as MIT. Its model card also documents the training data, intended uses, and limitations.
- SAM 3 is distributed under Meta's separate [SAM License](https://huggingface.co/facebook/sam3/blob/main/LICENSE), not the MIT License. Access to the official Hugging Face files may require signing in, sharing the requested contact information, and accepting Meta's terms.
- Qwen3-VL-2B-Instruct is distributed under the [Apache License 2.0](https://huggingface.co/Qwen/Qwen3-VL-2B-Instruct/blob/main/LICENSE).

Review the complete licence and model card shown by RawCull before downloading
or using a model. RawCull records the exact model revision and licence version
applicable to each converted bundle. If a model update changes its licence,
RawCull must present the new terms before downloading that update.
