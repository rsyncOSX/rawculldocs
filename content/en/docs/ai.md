+++
author = "Thomas Evensen"
title = "AI Analysis"
description = "Set up RawCull’s local AI models, choose a Review preset, and enable fast CLIP similarity."
date = "2026-11-24"
lastmod = "2026-11-24"
weight = 50
tags = ["ai", "review", "clip"]
categories = ["user doc"]
+++

RawCull uses AI to group similar shots, find photos from a text description,
and help you review a shortlist. The models process photos locally on your Mac.
You decide which frames to keep, rate, or mark as reviewed.

## Set up the three models

On an Apple Silicon Mac running macOS 27 (Golden Gate), open **Settings → AI**.
Choose **Download AI Models** to install the models you want to use. The AI
settings show which models are available. Use **Check Again** to refresh their
status.

| Model | What it does |
| --- | --- |
| **DataComp CLIP** | Matches photos by visual content so RawCull can group similar frames. It also connects photos with words, so you can search for a description like “bird in flight” or check how closely a subject crop matches a description during review. |
| **SAM 3** | Finds the outline of a subject and creates a mask. RawCull uses that outline to measure sharpness on the subject itself, even when the background contains sharp detail. |
| **Qwen3-VL-2B-Instruct** | Looks at a photo preview and puts observations into words. It can comment on composition, exposure, and how clearly the subject is visible. Use its observations alongside your own inspection of the frame. |

## Choose a Review button

Select or tag the photos you want to inspect, then open **Review** in the
toolbar. Choose one of the three presets.

| Button | When to use it |
| --- | --- |
| **Focus check** | Use this when you want to know whether the subject looks sharp. RawCull uses SAM 3 to isolate the subject and measure focus, with subject relevance from CLIP when available. This preset can run on Macs with less than 16 GB of unified memory. |
| **Describe** | Use this for a written overview of each photo. Qwen looks at the whole preview once per photo. |
| **Full** | Use this for a closer look at a few hard-to-choose frames. It combines Qwen’s overview with subject masks, focus measurements, and CLIP relevance when the models are available. Deeper settings can inspect more crops and run additional Qwen checks. |

From the results, you can tag, rate, or mark photos as reviewed.

**Full review takes time and uses more memory.** Full and Describe can run
with **16 GB of unified memory**. RawCull recommends **32 GB** for these
presets. On a 16 GB Mac, start Full with a few shortlisted photos at the
standard depth and allow extra time for the models to load and process them.

## Enable CLIP for fast similarity

For fast similarity indexing and burst grouping, install **DataComp CLIP**
and turn on **Use selected CLIP model for similarity** in **Settings → AI**.
This is the recommended setting for catalog similarity. It also enables
search by description after indexing. Open **Similarity** and choose
**Re-index** if you already indexed the catalog with another model or with
Apple Vision. See [Similarity, Bursts, and Search](/docs/similarity/).

For Qwen photo assessments requested from Codex, see
[Model Context Protocol (MCP)](/docs/mcp/#assess-photos-from-a-completed-job).
