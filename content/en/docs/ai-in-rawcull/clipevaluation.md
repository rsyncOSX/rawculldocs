+++
author = "Thomas Evensen"
title = "CLIP Models and Beta Testing"
linkTitle = "CLIP Models and Beta Testing"
date = "2026-08-13"
description = "How RawCull uses and evaluates its two CLIP models, why beta testers should try both, and how to compare their results."
weight = 2
tags = ["AI", "CLIP", "semantic search", "similarity", "beta testing"]
categories = ["user doc"]
+++

RawCull supports two optional CLIP models for semantic search, visual
similarity, and burst grouping. This page explains what CLIP does, how the
models were evaluated, and how to compare them during the RawCull beta.

Both models run locally on Apple Silicon. RawCull does not upload photographs,
search text, or model results to an external inference service.

## What CLIP Does

CLIP connects text and images by converting both into numeric embeddings. An
embedding represents visual and semantic characteristics in a form that can be
compared efficiently.

RawCull uses CLIP in two ways:

- **Semantic search:** a description such as `a dog on a beach`, `city street
  at night`, or `sharp portrait` is compared with the indexed photographs.
- **Visual similarity and burst grouping:** image embeddings are compared with
  each other to find related frames.

CLIP does not generate captions, modify photographs, rate artistic quality, or
make final culling decisions. Its search scores are relative similarity values,
not confidence percentages. The photographer remains responsible for every
selection and rejection.

## The Two Models

| Model | Image input | Background | Practical role |
|---|---:|---|---|
| OpenAI CLIP ViT-B/32 | 224 × 224 | The established OpenAI CLIP model | A conservative and well-understood reference model |
| OpenCLIP DataComp ViT-B/32-256 | 256 × 256 | OpenCLIP model trained with DataComp weights | An alternative with different retrieval behavior that may suit some catalogs and searches better |

The models use different weights, image sizes, tokenization details, and
converted Core AI graphs. They therefore do not rank every photograph in the
same way. A higher raw score from one model cannot be compared directly with a
score from the other.

## How the Models Were Evaluated

RawCull uses two independent forms of evaluation.

### Conversion parity

The first test compares embeddings produced by each converted Apple Core AI
bundle with embeddings from its original source framework. This detects errors
in model conversion, tokenization, preprocessing, output selection, and
normalization.

Text and image parity are tested separately. A cosine value of `1.0` means that
the normalized source and Core AI embeddings are identical. Very small
differences are expected from Float16 conversion and differences in image
decoding or resizing.

### Product behavior

The second test builds complete, model-specific indexes for the same photo
catalog and runs the same 77 text queries. It checks result diversity, repeated
"hub" images, query time, paraphrase behavior, and image-similarity
neighborhoods.

Conversion parity answers whether Core AI reproduces the source model. Product
testing answers whether the model is useful for photographers. One test cannot
replace the other.

## Current Results

The current Core AI bundles produced these results on the fixed conversion
fixtures and the 77-query RawCull test:

| Result | OpenAI CLIP | DataComp CLIP |
|---|---:|---:|
| Minimum text parity | approximately `1.0000` | approximately `1.0000` |
| Minimum end-to-end image parity | `0.9988` | `0.9908` |
| Distinct first-ranked images across 77 queries | 53 | 58 |
| Largest repeated first-result hub | 4 of 77 | 4 of 77 |
| Warm mean query time | 67.25 ms | 65.67 ms |

The two models selected the same first-ranked photograph for only 19 of the 77
queries, or approximately 25 percent. This is the most important beta-testing
finding: the models offer meaningfully different search behavior.

OpenAI currently passes RawCull's chosen `0.998` end-to-end conversion-parity
gate. DataComp's text path is extremely close to its source implementation,
but its image path has a larger difference that remains under investigation.
That difference does not by itself mean that DataComp produces worse semantic
search. DataComp can still retrieve better results for a particular query or
catalog, and the initial product test produced slightly more diverse first
results.

The test set is not yet a large human-labeled accuracy benchmark. Beta feedback
is therefore valuable, especially when it describes which model returned a
more useful photograph and why.

## Why Beta Testers Should Download Both

Downloading both models lets you compare two genuinely different views of the
same catalog. OpenAI may work better for one subject or photographic style,
while DataComp may work better for another. Testing both also helps RawCull
identify weak query categories and choose sensible model-specific defaults.

Each model is optional. Downloading both uses more storage and creating an
index for each takes additional time. A user who does not want to participate
in comparison testing can install only one model. For this beta, however,
installing both is the most useful configuration.

The models are downloaded through macOS Managed Background Assets. macOS stores
and manages the asset packs, and RawCull validates a model before enabling it.

## Download Both Models

1. Open **RawCull > Settings > AI**.
2. Select **Download AI Models**.
3. Review the licence and model information for each CLIP model.
4. Download **OpenAI CLIP** and **DataComp CLIP**.
5. Wait until both downloads report **Installed**.
6. Close the download window and select **Check Again** if either model has not
   yet appeared as available.

Downloaded models run locally. Removing a managed model from the same window
removes its RawCull asset pack but does not delete manually installed models.

## Compare the Models

Use the same photographs and search phrases for both tests.

### Test OpenAI CLIP

1. In **Settings > AI**, choose **OpenAI** under **Selected CLIP model**.
2. Enable **Use selected CLIP model for similarity**.
3. Open the catalog you want to test.
4. Select **Index Similarity** or **Re-index** and wait for indexing to finish.
5. Try a set of short English descriptions and record which results are useful.
6. If you use burst analysis, inspect the groups without changing the source
   catalog before testing the second model.

### Test DataComp CLIP

1. Return to **Settings > AI** and choose **DataComp**.
2. Keep **Use selected CLIP model for similarity** enabled.
3. Re-index the same catalog. Embeddings from OpenAI and DataComp are not
   interchangeable, so the DataComp test requires its own index.
4. Repeat exactly the same searches and burst-analysis workflow.
5. Compare the photographs, rankings, and groups rather than comparing the raw
   numeric scores between models.

Useful test phrases include:

- objects and scenes: `a dog`, `mountains surrounding a lake`;
- colors and attributes: `yellow flower`, `red car`;
- actions and relationships: `person riding a bicycle`, `bird in flight`;
- photographic properties: `sharp portrait`, `motion blur`, `backlit subject`;
- paraphrases: try several different descriptions of the same idea.

Search primarily in English during this beta because the supported models were
trained and evaluated mainly with English text.

## What to Report

Useful feedback includes:

- RawCull beta version, macOS build, and Mac model;
- selected CLIP model;
- approximate number and type of photographs in the catalog;
- the exact search phrase;
- which model returned the more useful first five results;
- examples of clearly relevant or irrelevant results;
- unexpected repeated results across unrelated searches;
- indexing failures, model validation errors, or unusually slow operation; and
- whether burst groups became more or less useful after switching models.

Screenshots are helpful, but do not share private photographs unless you are
comfortable doing so. A text description of the expected and actual result is
enough.

## Choosing a Model After Testing

There is no universal winner yet. Choose the model that gives the most useful
results for your photographs and vocabulary. OpenAI is the established
reference, while DataComp is a promising alternative with noticeably different
retrieval behavior.

You can switch later, but changing models requires a compatible index for the
new selection. RawCull keeps model identities separate so that embeddings from
one model are never silently reused with the other.

For the broader feature overview, see [AI Support in RawCull](/docs/ai-in-rawcull/ai/).
