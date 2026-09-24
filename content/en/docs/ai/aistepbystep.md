+++
author = "Thomas Evensen"
title = "AI Step by Step"
linkTitle = "AI Step by Step"
date = "2026-09-21"
lastmod = "2026-09-24"
description = "A practical workflow for using Burst Review, SAM 3 with CLIP, Qwen Vision, and Objects to review selected photographs."
weight = 57
tags = ["ai", "culling", "burst-review", "clip", "sam3", "qwen", "objects"]
categories = ["guides"]
+++

# AI Step by Step

RawCull uses three local AI models. Each has a different job:

| Model | What it is | What it contributes |
| --- | --- | --- |
| **CLIP** | An image-and-text embedding model | Recognizes visual similarity, helps form burst groups, and can identify a broad subject such as a person, bird, or animal. |
| **SAM 3** | A prompt-guided segmentation model | Draws a mask around the subject so RawCull can measure detail on the important part of the photograph instead of the whole frame. |
| **Qwen Vision** | A vision-language model | Reviews the visible photograph and comments on composition, exposure, subject visibility, expression, obstructions, strengths, and problems. |

In **Deep Review**, SAM 3 and CLIP work together. CLIP suggests what the
subject is; SAM 3 uses that information to find the subject; RawCull then
measures detail inside the mask. If CLIP cannot supply a useful label, SAM 3
can still use the general **Subject** prompt.

All three models run on the Mac. They advise you; they do not replace your
decision. Before starting, install the models you want under **Settings › AI**;
SAM 3 also requires acceptance of its licence.

## Why analyze only a small set with AI?

Detailed AI review is much heavier than ordinary thumbnail browsing. SAM 3
must create a subject mask for each photograph, while Qwen examines and
describes one photograph at a time. Running both over every file would spend
time on obvious rejects and repeated frames.

RawCull therefore uses a funnel:

1. **Burst Review** quickly reduces the complete catalog.
2. You keep a small set by selecting images or rating them with two or more stars.
3. **AI Analysis** gives those finalists a closer review.

This is both faster and more useful: AI spends its time on the difficult
choices. For a SAM 3 + CLIP batch larger than 12 images, RawCull reviews at
most eight candidates, using the available burst ranking or the current file
order.

## 1. Begin with Burst Review

Open a catalog, go to **Burst Review**, and choose **Analyze Bursts**.

RawCull performs three steps:

1. **Similarity:** CLIP creates a visual description of each image and groups
   near-duplicates. If CLIP is unavailable, RawCull can use Apple Vision for
   similarity instead.
2. **Sharpness:** RawCull measures focus and useful detail. This is traditional
   image analysis, not a generative AI opinion.
3. **Ranking:** similarity and sharpness evidence are combined to suggest the
   strongest frames in each burst.

Start with **Needs review**. Compare the best-ranked frames, check important
details at a useful zoom level, and make your own decision. Select the
remaining candidates in Grid View, or rate promising images with **two or more
stars** so that they appear as Tagged images later.

## 2. Choose the finalists

Open **AI Analysis** from the toolbar. At the top right, choose one input:

- **Selected** uses the images currently selected in Grid View.
- **Tagged** uses every active-catalog image rated two stars or higher.

Keep this set small. A handful of genuinely close candidates is ideal.

## 3. Run SAM 3 + CLIP

Choose **SAM 3 + CLIP**, then select a review target:

- **Auto** lets the detected subject guide the mask.
- **Full Subject** checks detail across the complete subject.
- **Head / Face** concentrates on the area that often decides portraits and wildlife photographs.

Choose **Run Deep Review**. For each candidate, inspect the subject outline,
the **Deep** score, the normal **Sharp** score, mask status, AF position, and
any warning in **Notes**. A high score is useful only when the mask covers the
subject you intended. If the outline is wrong, trust the photograph—not the
number.

Use this review to answer: **Which frame contains the best detail on the
subject that matters?**

## 4. Run Qwen Vision

Choose **Qwen Vision**. The default prompt asks about composition, exposure,
subject visibility, expression, and obstructions. You may replace it with a
specific question, for example: *Which visible problems would matter in a
final edit?*

Choose **Run Analyze**. Qwen processes the pending images one at a time and
returns scores plus strengths and issues. It may also report whether eyes are
open when that can be judged from the image.

Use this review to answer: **Does the photograph work as a photograph?** Qwen
adds a visual critique; it does not participate in burst similarity or SAM 3
subject-detail scoring.

## 5. Run AI Objects

Choose **Objects** and keep **Selected** or **Tagged** as the input. SAM 3 and Qwen must both show as ready.

1. Leave **Concepts** on **Automatic** for Qwen to suggest concrete subjects in each photograph. If you already know what to look for, choose **Specific Concepts** and enter short comma-separated terms such as `puffin` or `deer, fawn`.
2. Optionally edit the additional photographic criteria, then choose **Analyze [number] Images** to process the pending photos. You can cancel a running batch, retry failed results, or clear results from this view.
3. Select a completed row. Compare the numbered outlines with the original image, then choose an object in the list to inspect its crop and Qwen description. The table also shows discovered concepts, object count, Qwen assessment confidence, and status.
4. Read the whole-photo summary, per-object visibility and focus notes, relationships, strengths, and problems. Treat the SAM 3 mask score and Qwen assessment confidence as different signals. Check the crop, outline, and wording against the source photo; small or overlapping subjects can be missed or mixed up.

Objects is useful when a frame contains several subjects, such as a deer with fawns or a group of musk oxen. Its numbered crops help you inspect each subject, but the analysis remains advisory. It does not rate, reject, or select a photograph for you.

## 6. Make the final choice

Put the evidence in this order:

1. Your intent and visual judgment.
2. Correct subject detail from SAM 3 + CLIP.
3. Object visibility and relationships from Objects, when the frame has multiple subjects.
4. Composition and visible issues from Qwen Vision.
5. The initial Burst Review ranking.

When the signals disagree, inspect the image. AI results are recommendations,
and RawCull does not automatically turn a Qwen result into a rating.

For the implementation details behind this workflow, see
[Burst Groups](../burstgroup/) and [AI Models in RawCull](../aiinrawcull/).
