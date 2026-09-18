+++
author = "Thomas Evensen"
title = "RawCull Screenshots"
date = "2026-08-20"
weight = 1
aliases = ["/docs/screenshots/"]
tags = ["screenshots"]
categories = ["user doc"]
+++

This visual tour shows how RawCull helps you inspect, group, search, and review
photos before making the final selections.

## Loupe View

The Loupe view keeps the selected photo large while nearby frames remain within
reach. The information panel brings together the histogram, sharpness and focus
results, file details, and camera settings needed for a careful decision.

{{< figure src="/images/loupeview.png" alt="Loupe view showing a puffin photo, nearby frames, and the photo information panel" position="center" style="border-radius: 8px;" >}}

## Similar Photos in Grid View

Choose a reference photo and RawCull uses DataComp CLIP to bring visually related frames together. The grid makes it easy to compare poses and timing across the catalog.

{{< figure src="/images/gridviewbysimilarity.png" alt="Grid view ordered by visual similarity to a selected puffin portrait" position="center" style="border-radius: 8px;" >}}

The same grid can also be sorted by estimated sharpness.

{{< figure src="/images/gridbysharpness.png" alt="Grid view ordered by estimated sharpness" position="center" style="border-radius: 8px;" >}}

## Burst List

RawCull groups related frames into bursts and highlights a suggested pick. You
can open a burst, run a deeper review, mark it complete, or defer the decision.

{{< figure src="/images/burslists.png" alt="Burst list with grouped puffin sequences and a suggested pick" position="center" style="border-radius: 8px;" >}}

## Focus Point and Focus Mask

In the full-window viewer, the Focus Point marks the autofocus location recorded
by the camera, while the Focus Mask highlights areas with strong edge detail.
Use the overlays together to check whether focus landed on the intended part of
the subject; they are guides rather than a guarantee of sharpness.

{{< figure src="/images/focusandmask.png" alt="Full-window puffin preview with the camera autofocus point and focus-mask detail overlays visible" position="center" style="border-radius: 8px;" >}}

## Burst Review

The burst reviewer places every frame in a filmstrip beneath a large preview.
Scores and camera details provide evidence, while you choose the strongest
moment, rate it, or reject it.

{{< figure src="/images/burstreview.png" alt="Burst reviewer comparing a puffin sequence with a large preview, filmstrip, and scoring details" position="center" style="border-radius: 8px;" >}}

## AI Analysis

AI Analysis works with photographs selected in Grid View or tagged for review.
Choose **SAM 3 + CLIP** for a subject-aware Deep Review or **Qwen** for a
criteria-based assessment. Both modes run locally on the Mac.

### SAM 3 + CLIP

Deep Review isolates the subject and combines subject-aware detail, sharpness,
autofocus, and coverage evidence to recommend a frame. Review the ranked results
and subject outline before marking the winner.

{{< figure src="/images/deepreview.png" alt="AI Analysis using SAM 3 and CLIP to rank selected photographs and outline the subject in the preview" position="center" style="border-radius: 8px;" >}}

### Qwen

Qwen evaluates the photographs against editable criteria and returns structured
scores for composition, exposure, and subject visibility. Select a result to
inspect its subject description, confidence, strengths, and possible problems.
The assessment is advisory, so confirm its findings against the photograph.

{{< figure src="/images/qwen.png" alt="AI Analysis using Qwen to score selected photographs and show strengths, problems, and confidence for one result" position="center" style="border-radius: 8px;" >}}

## Semantic Search

Describe what you want to find in everyday language. RawCull ranks the locally
indexed catalog by meaning, as shown here for the search `puffins in flight`.

{{< figure src="/images/puffinsinflight.png" alt="Semantic search results ranked for the phrase puffins in flight" position="center" style="border-radius: 8px;" >}}

## AI Settings

The AI settings show whether the local DataComp CLIP, SAM 3 and Qwen models are ready.
DataComp CLIP powers similarity indexing and semantic search.

{{< figure src="/images/aisettings.png" alt="AI settings showing DataComp CLIP and SAM 3 readiness" position="center" style="border-radius: 8px;" >}}

## Model Downloads

RawCull lists each optional model with its purpose, source, licence, and current
status. Models run locally after installation, and photographs are not uploaded
as part of the download.

{{< figure src="/images/modeldownload.png" alt="AI model download window showing model installation status" position="center" style="border-radius: 8px;" >}}

Download progress is shown in the same window, where an active transfer can also be cancelled.
