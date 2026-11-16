+++
author = "Thomas Evensen"
title = "AI Analysis"
linkTitle = "AI"
description = "Set up local AI models and use them to compare your best photographs."
weight = 50
date = "2026-07-28"
lastmod = "2026-11-16"
aliases = ["/docs/ai/aianalysis/", "/docs/ai/aistepbystep/"]
tags = ["AI", "CLIP", "SAM 3", "Qwen", "Objects"]
categories = ["user doc"]
+++

RawCull uses optional AI models for catalog search and closer review of selected
photos. The models run on your Mac and require macOS 27 (Golden Gate) and
Apple Silicon. You can browse and rate photos without installing models.

## Set up the models

Open **Settings › AI › Download AI Models**. The model manager shows each
model’s purpose, version, licence, and installation status. Review the licence
before downloading. macOS manages the downloads, and your photos stay on your
Mac during download and analysis.

| Model | What it does |
| --- | --- |
| **DataComp CLIP** | Indexes decodable RAW photos for visual similarity, burst grouping, and semantic search. |
| **SAM 3** | Outlines subjects so RawCull can measure their detail or show individual objects. |
| **Qwen3-VL** | Reviews photos against editable criteria and describes objects and their relationships. |

To search the catalog by description, enable DataComp CLIP and choose
**Index Similarity** or **Re-index** in Similarity view. Once indexed, try a
short English term such as `puffin`. RawCull reuses the index for later searches.
See [Similarity, Bursts, and Search](/docs/similarity/) for the full catalog workflow.

## Choose photos to review

1. Use **Similarity › Analyze Bursts** to compare neighboring frames, or pick
   candidates directly in Grid View.
2. Select a few finalists in Grid View or give them two stars or more.
3. Open **AI Analysis** from the toolbar. Choose **Selected** for the current
   Grid View selection or **Tagged** for all photos in the active catalog rated
   two stars or higher.

SAM 3 and Qwen need more processing than CLIP indexing, so start with the
photos you’re still deciding between.

## Pick a review tab

| Tab | How to use it |
| --- | --- |
| **SAM 3 + CLIP** | Choose **Auto**, **Full Subject**, or **Head / Face**, then select **Run Deep Review**. Check that the outline covers the intended subject before comparing detail, sharpness, and autofocus evidence. |
| **Qwen Vision** | Keep or edit the review criteria, then select **Run Analyze**. Read the evidence behind the scores and check any issues with composition, exposure, or subject visibility against the photo. Unsupported criteria may remain unscored. |
| **Objects** | Choose **Automatic** to let Qwen suggest objects or **Specific Concepts** to enter terms such as `deer, fawn`. Run the analysis, then select a result and a numbered object to inspect its outline, crop, and description. Check those against the original, especially for small or overlapping subjects. |

Use the results to guide what you inspect at full size. You make the final
selection and set the ratings.
