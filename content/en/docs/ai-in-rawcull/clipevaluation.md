+++
author = "Thomas Evensen"
title = "DataComp CLIP and Beta Testing"
linkTitle = "DataComp CLIP and Beta Testing"
date = "2026-08-13"
description = "How RawCull uses and evaluates DataComp CLIP, and how beta testers can report useful results."
weight = 2
tags = ["AI", "CLIP", "DataComp", "semantic search", "similarity", "beta testing"]
categories = ["user doc"]
+++

RawCull uses DataComp CLIP for semantic search, visual similarity, and burst
grouping. This page explains what the model does, how RawCull validates it, and
how to provide useful feedback during the RawCull beta.

DataComp CLIP runs locally on Apple Silicon. RawCull does not upload
photographs, search text, embeddings, or model results to an external inference
service.

## What DataComp CLIP Does

CLIP connects text and images by converting both into numeric embeddings. An
embedding represents visual and semantic characteristics in a form that can be
compared efficiently.

RawCull uses DataComp CLIP in two ways:

- **Semantic search:** a description such as `a dog on a beach`, `city street
  at night`, or `sharp portrait` is compared with the indexed photographs.
- **Visual similarity and burst grouping:** image embeddings are compared with
  each other to find related frames.

CLIP does not generate captions, modify photographs, rate artistic quality, or
make final culling decisions. Its search scores are relative similarity values,
not confidence percentages. The photographer remains responsible for every
selection and rejection.

## Supported CLIP Model

| Model | Image input | Background | Purpose |
|---|---:|---|---|
| OpenCLIP ViT-B/32 DataComp | 256 × 256 | OpenCLIP model trained with DataComp `s34B-b86K` weights | Semantic search, visual similarity, and burst grouping |

RawCull uses a converted Apple Core AI bundle based on the [DataComp model on
Hugging Face](https://huggingface.co/laion/CLIP-ViT-B-32-256x256-DataComp-s34B-b86K).
The bundle includes the preprocessing, tokenization, and normalization required
by RawCull.

## How the Model Is Evaluated

RawCull uses two complementary forms of evaluation.

### Conversion parity

Conversion tests compare embeddings from the converted Apple Core AI bundle
with embeddings from the original source framework. Text and image paths are
tested separately to detect errors in conversion, tokenization, preprocessing,
output selection, or normalization.

A cosine similarity of `1.0` means the normalized source and Core AI embeddings
are identical. Small differences can result from Float16 conversion and from
differences in image decoding or resizing.

### Product behavior

Product tests build a complete index for a fixed photo catalog and run a stable
set of text queries. They check result relevance and diversity, repeated
"hub" images, query time, paraphrase behavior, and image-similarity
neighborhoods.

Conversion parity shows whether Core AI reproduces the source model. Product
testing shows whether the model is useful to photographers. Neither test
replaces the other, and the current test set is not a large human-labeled
accuracy benchmark.

## Install DataComp CLIP

1. Open **RawCull > Settings > AI**.
2. Select **Download AI Models**.
3. Review the DataComp CLIP model information and licence.
4. Download **DataComp CLIP** and wait until its status is **Installed**.
5. Close the download window and select **Check Again** if the model does not
   yet appear as available.
6. Enable DataComp CLIP for similarity.

The model is downloaded through macOS Managed Background Assets. macOS stores
and manages the asset pack, and RawCull validates it before enabling AI
features. Once installed, the model runs locally.

## Test Search and Similarity

Use a representative catalog and a repeatable set of search phrases:

1. Open the catalog you want to test.
2. Select **Index Similarity** or **Re-index** and wait for indexing to finish.
3. Try a set of short English descriptions and note which results are useful.
4. Choose reference photos and inspect the visually similar results.
5. Run burst analysis and check whether the resulting groups are coherent.

Useful test phrases include:

- objects and scenes: `a dog`, `mountains surrounding a lake`;
- colors and attributes: `yellow flower`, `red car`;
- actions and relationships: `person riding a bicycle`, `bird in flight`;
- photographic properties: `sharp portrait`, `motion blur`, `backlit subject`;
- paraphrases: several different descriptions of the same idea.

Search primarily in English during the beta because DataComp CLIP was trained
and evaluated mainly with English text.

## What to Report

Useful feedback includes:

- RawCull beta version, macOS build, and Mac model;
- approximate number and type of photographs in the catalog;
- the exact search phrase;
- examples of relevant and irrelevant results in the first five matches;
- unexpected repeated results across unrelated searches;
- examples of useful or incorrect visual-similarity groups;
- indexing failures, model-validation errors, or unusually slow operation; and
- burst groups that combine unrelated sequences or split a single sequence.

Screenshots are helpful, but do not share private photographs unless you are
comfortable doing so. A text description of the expected and actual result is
enough.

For subject-aware burst comparison using SAM 3, and for the broader AI feature
overview, see [AI Support in RawCull](/docs/ai-in-rawcull/ai/).
