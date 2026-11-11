+++
author = "Thomas Evensen"
title = "Similarity, Bursts, and Search"
date = "2026-07-15"
weight = 40
tags = ["similarity", "bursts"]
categories = ["user doc"]
lastmod = "2026-11-11"
+++

Similarity groups visually related frames into bursts and suggests strong candidates. It shortens review while leaving the final choice to you.

RawCull can use DataComp CLIP or Apple Vision for visual grouping. Install and enable DataComp CLIP in **Settings > AI** to index a catalog for semantic search.

## Analyze a Catalog

1. Open **Similarity**.
2. Choose **Analyze Bursts**.
3. Open **Needs Review** when analysis finishes.

RawCull runs missing sharpness scoring and visual indexing automatically. Use **Re-index** after changing the catalog or updating the CLIP model to rebuild compatible visual representations.

The similarity slider controls grouping: lower values make tighter groups; higher values include more related frames.

## Review Bursts

Each group can include a suggested pick and supporting sharpness or subject information. Open a group to inspect its filmstrip, rate frames, defer the group, mark it reviewed, or open **Compare** for a closer view.

Use **Set pick** to override the suggestion. One-click **Keep Best** rates the suggested frame 3 stars and rejects the other frames; **Keep Top Two** rates the first two candidates 3 and 2 stars and rejects the rest. Review the suggestion before applying either action.

The main queues are:

| Queue | Purpose |
|---|---|
| Needs Review | Bursts still awaiting a decision |
| Deferred | Groups saved for later |
| Marked Reviewed | Groups you have checked |
| Single Images | Photos outside multi-frame bursts |

Analysis results are cached for the catalog. Ratings and manual picks are saved with the rest of the culling data.

For deeper model-based review, select photographs in Grid View and open [AI Analysis](/docs/ai/). SAM 3 and Qwen operate on the selected set rather than on every catalog photograph.

### Compare close-ups

Open **Close-ups** in Burst Review to inspect the active frame. Command-click frames or Shift-click a range in the filmstrip to compare multiple frames. Use **Jump to image** to go directly to the detected crops for a selected frame. Opening Burst Review from a specific frame in the culling grid starts on that frame.

Set a reference region manually or use the recorded autofocus point when available. The **JPG** and **RAW** source picker applies to both the main burst preview and the close-ups. The picker reads **RAW 9** when every relevant frame supports it. In a mixed selection, each crop identifies the decoder it used.

The panel can detect human faces and, where supported, show experimental animal head and eye crops. Automatic detection pauses during catalog indexing and resumes afterward.

### Try People Ranking

With **Best Frame** order active, choose **Apply People Ranking** for a burst showing the same person. RawCull checks the entire burst for at least two frames, one measurable face per frame, matching embedded-preview dimensions, and comparable face sizes. If a check fails, the existing order stays in place and RawCull explains why.

Face detail can move either of the first two Best Frame candidates to the top. **Most face detail** marks the highest face-detail measurement across the burst and can appear on another frame. People Ranking changes the displayed order. Choose **Restore Best Frame Order** to return to the baseline.

## Semantic Search

After DataComp CLIP indexing finishes, enter a short English description such as `bird in flight` or `backlit portrait`. RawCull ranks the catalog by relative text-to-image similarity. The ranking is not a confidence score, so inspect the results before making culling decisions.
