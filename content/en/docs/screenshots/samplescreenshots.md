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

Choose a reference photo and RawCull uses CLIP to bring visually related frames together. The grid makes it easy to compare poses and timing across the catalog.

{{< figure src="/images/gridviewbysimilarity.png" alt="Grid view ordered by visual similarity to a selected puffin portrait" position="center" style="border-radius: 8px;" >}}

By estimated sharpness.

{{< figure src="/images/gridbysharpness.png" alt="Grid view ordered by sharpnesst" position="center" style="border-radius: 8px;" >}}

## Burst List

RawCull groups related frames into bursts and highlights a suggested pick. You
can open a burst, run a deeper review, mark it complete, or defer the decision.

{{< figure src="/images/burslists.png" alt="Burst list with grouped puffin sequences and a suggested pick" position="center" style="border-radius: 8px;" >}}

## Burst Review

The burst reviewer places every frame in a filmstrip beneath a large preview.
Scores and camera details provide evidence, while you choose the strongest
moment, rate it, or reject it.

{{< figure src="/images/burstreview.png" alt="Burst reviewer comparing a puffin sequence with a large preview, filmstrip, and scoring details" position="center" style="border-radius: 8px;" >}}

## Semantic Search

Describe what you want to find in everyday language. RawCull ranks the locally
indexed catalog by meaning, as shown here for the search `puffins in flight`.

{{< figure src="/images/puffinsinflight.png" alt="Semantic search results ranked for the phrase puffins in flight" position="center" style="border-radius: 8px;" >}}

## Deep Review 

Deep Review by SAM 3

{{< figure src="/images/deepreview.png" alt="Semantic search results ranked for the phrase puffins in flight" position="center" style="border-radius: 8px;" >}}

## AI Settings

The AI settings show which local models and supporting features are ready. Here
you can select the CLIP model used for similarity indexing and semantic search.

{{< figure src="/images/aisettings.png" alt="AI settings showing available CLIP models and integration readiness" position="center" style="border-radius: 8px;" >}}

## Model Downloads

RawCull lists each optional model with its purpose, source, licence, and current
status. Models run locally after installation, and photographs are not uploaded
as part of the download.

{{< figure src="/images/modeldownload.png" alt="AI model download window showing one installed CLIP model and another ready to download" position="center" style="border-radius: 8px;" >}}

Download progress is shown in the same window, where an active transfer can also be cancelled.

