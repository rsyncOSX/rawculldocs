+++
author = "Thomas Evensen"
title = "Similarity and Bursts"
date = "2026-07-15"
weight = 15
tags = ["similarity", "bursts"]
categories = ["user doc"]
+++

Similarity groups visually related frames into bursts and suggests the strongest candidates. It is intended to shorten review, while leaving the final choice to you.

RawCull 3 uses DataComp CLIP for visual grouping and semantic search. Install
and enable it in **Settings > AI** before indexing a catalog.

## Analyze a Catalog

1. Open **Similarity**.
2. Choose **Analyze Bursts**.
3. Open **Needs Review** when analysis finishes.

RawCull runs any missing sharpness scoring and similarity indexing automatically. Use **Re-index** after the catalog changes or when you want to rebuild the analysis.

Re-index after updating DataComp CLIP or changing the catalog so RawCull can
create compatible embeddings for every photo.

The similarity slider controls grouping: lower values make tighter groups; higher values include more related frames.

## Review Bursts

Each group can include a suggested pick and supporting sharpness or subject information. Open a group to inspect its filmstrip, rate frames, defer the group, mark it reviewed, or open **Compare** for a closer view. In RawCull 3, **Deep Review** adds subject-mask, focus, and sharpness evidence to the comparison.

Use **Set pick** to override the suggestion. One-click **Keep Best** rates the suggested frame 3 stars and rejects the other frames; **Keep Top Two** rates the first two candidates 3 and 2 stars and rejects the rest. Review the suggestion before applying either action.

The main queues are:

| Queue | Purpose |
|---|---|
| Needs Review | Bursts still awaiting a decision |
| Deferred | Groups saved for later |
| Marked Reviewed | Groups you have checked |
| Single Images | Photos outside multi-frame bursts |

Analysis results are cached for the catalog. Ratings and manual picks are saved with the rest of the culling data.

## Semantic Search in RawCull 3

After DataComp CLIP indexing finishes, enter a short English description such as `bird
in flight` or `backlit portrait`. RawCull ranks the catalog by relative
text-to-image similarity. The ranking is not a confidence score, so inspect the
results before making culling decisions.
