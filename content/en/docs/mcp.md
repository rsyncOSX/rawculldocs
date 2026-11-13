+++
author = "Thomas Evensen"
title = "Model Context Protocol (MCP)"
description = "See how Codex uses RawCull’s experimental MCP service to run a burst analysis and retrieve recommended frames."
date = "2026-11-13"
weight = 45
tags = ["mcp", "bursts"]
categories = ["user doc"]
+++

The Model Context Protocol (MCP) is an open standard that lets AI assistants connect to tools and data exposed by other applications. An assistant can call a tool with structured inputs and use the result to answer a question or carry out the next step. [Model Context Protocol project](https://blog.modelcontextprotocol.io/posts/welcome-to-mcp-blog/)

RawCull has added an MCP service in its development build. Through a local helper, an assistant such as Codex can inspect catalogs, request analysis, check progress, and retrieve recommended frames from ranked bursts.

## Burst analysis from Codex

In the development run below, Codex first checked the RawCull connection.

> Use only the rawcull MCP server. Call rawcull_status and report the helper version, transport, and available tools.

The `rawcull_status` call reported helper version **0.6.0** over standard input and output (`stdio`). Its tools included catalog and photo listing, analysis setup and status, cancellation, and burst results.

Codex then received this prompt:

> Use only rawcull. I’ve replaced the photos in /Volumes/MacMini4/Orrfugl_mai_2026/DCIM. Start a new burstAnalysis using that exact path and a fresh request ID. Rescan the folder and run the full pipeline, including similarity indexing and sharpness scoring before burst grouping and ranking. Wait for completion, then report how many photos were analyzed and show the recommended frame in each of the first five bursts.

### Progress in the console

The first request reached `pendingApproval` and then failed because RawCull was busy. Codex submitted a second request with a fresh ID and polled it through completion. These are the main tool responses from the console, with repeated polls and JSON payloads omitted.

| Tool | Progress reported |
|---|---|
| `request_catalog_setup` | The second request entered `pendingApproval`. |
| `get_catalog_setup_status` | The request moved to `queued`. |
| `get_analysis_status` | `openingCatalog`. The folder rescan found 1,846 photos. |
| `get_analysis_status` | `scoringSharpness`. Codex reported 1,272 of 1,846 photos scored while the job was still running. |
| `get_analysis_status` | `indexingSimilarity`. RawCull used CLIP for this run. |
| `get_analysis_status` | `completed`, with 1,846 photos analyzed. |
| `list_bursts` | Returned the requested 5 of 156 ranked bursts. |

{{< figure src="/images/mcp/burststatus.png" alt="RawCull Burst Groups overview showing sharpness and CLIP indexing ready for all 1,846 photos and 156 bursts awaiting review" caption="RawCull’s Burst Groups overview after analysis." position="center" style="border-radius: 8px;" >}}

### Recommended frames

Codex reported these recommendations for the first five bursts.

| Burst | Frames | Recommended frame |
|---|---:|---|
| 1 | 13 | `_DSC6821.ARW` |
| 2 | 13 | `_DSC6834.ARW` |
| 3 | 6 | `_DSC6841.ARW` |
| 4 | 4 | `_DSC6845.ARW` |
| 5 | 5 | `_DSC6850.ARW` |

All five recommendations had low confidence because the top two candidates were close. The results also flagged high ISO noise. The console returned filenames. RawCull’s burst list shows the frames side by side for visual review.

{{< figure src="/images/mcp/burstcompleted.png" alt="RawCull showing rows of grouped bird photos in the burst list, with suggested frames marked for review" caption="Grouped frames in RawCull after the burst analysis completed." position="center" style="border-radius: 8px;" >}}

## Notes

RawCull’s MCP service is still early in development. This run verified the workflow end to end. Codex requested the analysis, tracked its progress, and retrieved recommendations after RawCull analyzed all 1,846 photos and ranked 156 bursts.

For more on reviewing the resulting groups in RawCull, see [Similarity, Bursts, and Search](/docs/similarity/).
