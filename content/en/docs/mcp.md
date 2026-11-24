+++
author = "Thomas Evensen"
title = "Model Context Protocol (MCP)"
description = "Connect Codex to RawCull’s local MCP helper, authorize catalog access, analyze bursts, and assess photos."
date = "2026-11-13"
lastmod = "2026-11-24"
weight = 45
tags = ["mcp", "bursts"]
categories = ["user doc"]
+++

RawCull bundles a local Model Context Protocol (MCP) helper. A client such as
Codex can read authorized catalog metadata, run burst analysis, retrieve ranked
frames, and request follow-up photo assessments.

**Codex** is only used as an example here, the following applies to any local AI-agent.

[Watch the RawCull MCP video on YouTube](https://youtu.be/ahWjBtW8iN0).

## How MCP works in RawCull

MCP gives Codex a standard way to find available tools, send them
requests, and read their results. Codex is the client, and `rawcull-mcp` is the
server. The helper and the RawCull app run as separate processes.

1. Codex starts the helper and communicates over standard input and output
   (`stdio`). There is no local web address or port to configure.
2. Codex asks the helper for its tools and the arguments each accepts.
3. When you ask for the helper version, Codex calls `rawcull_status` with no
   arguments. The helper receives the tool name and any arguments. Codex keeps
   the conversation.
4. The helper returns the version, connection type, and available tools. Codex
   can report the result or use it to choose the next tool.

The helper runs on your Mac. If Codex uses a cloud model, data returned by the
tools, such as filenames and capture metadata, may be sent to that model.

## Connect Codex to RawCull

Register the signed helper bundled with RawCull. Adjust the path if you installed
RawCull elsewhere. If `rawcull` is already registered to a different executable,
run `codex mcp remove rawcull` first.

```bash
helper=/Applications/RawCull.app/Contents/MacOS/rawcull-mcp
codex mcp add rawcull -- "$helper"
```

Start a fresh Codex session and use the [connection prompt](#check-the-connection-and-catalog-access) to verify what your installed helper supports.

## Control catalog access

After the catalog finishes loading, open **Settings → MCP** and choose
**Authorize Selected Catalog** to share a snapshot of its photo metadata. Before
authorizing, enable **Allow MCP burst analysis and photo assessments** to let the
client start jobs. Enable **Allow analysis of future photos in this folder** if
you reuse the same folder for new shoots.

A prompt can open RawCull and ask you to select or confirm a folder.
**Approve This Job** queues one analysis and permits follow-up assessments on its
shared result. **Approve and Remember** also saves permission for eligible future
runs. RawCull keeps one saved catalog authorization and one current shared burst
result.

The helper can read saved metadata and shared rankings while RawCull is closed.
Use **Clear Last Shared Result** to remove the rankings and their dependent
assessments. Use **Revoke Access** to remove the saved authorization and current
job.

## Run a burst analysis

Codex submits `request_catalog_setup` with your folder path and checks
`get_catalog_setup_status` if approval is pending. Setup queues the analysis and
returns the catalog and job IDs. Codex uses those IDs with `get_analysis_status`
to track progress and `list_bursts` to read the completed rankings. For a catalog
with saved analysis permission, `start_catalog_analysis` can queue the job
directly.

Use a fresh request UUID for new work or changed arguments. Reuse the UUID and
arguments when retrying the same request. For a complete review, read each
ranking page by passing `nextCursor` as `cursor` until it is null.

## Assess photos from a completed job

Install **Qwen3-VL-2B** in **Settings → AI** and turn on **Enable MCP photo
assessments** in **Settings → MCP**. Complete an authorized burst analysis
first. Assessments work with Vision or CLIP rankings.

A request can target a burst's saved recommendation and alternate via
`burstId`, or one to four photos from the source job via `photoIds`. Choose one
or more rubric fields from `composition`, `exposure`, and `subjectVisibility`.
Run one assessment at a time and retrieve its result before submitting another.

RawCull processes previews locally. The assessment response shares photo IDs,
preview dimensions, scores, text findings, and per-photo errors. The client can
match IDs to filenames from the rankings and may send findings to its model
provider.

Keep the source catalog and job IDs and each assessment request ID for
follow-ups. Results can be read for up to 24 hours while the source job and
approval are current. Finish follow-ups before starting another analysis that
replaces the shared result. Turning off **Enable MCP photo assessments** cancels
active assessments, clears shared findings, and blocks assessment calls. After
re-enabling it, run a fresh burst analysis before requesting an assessment.

## Available MCP tools

| Tool | Input and behavior |
| --- | --- |
| `rawcull_status` | Takes no arguments. Reports the helper version, app build when available, transport, supported capabilities, and assessment setting. |
| `list_catalogs` | Takes no arguments. Lists the saved catalog ID, display name, and permissions when an authorization exists. |
| `catalog_summary` | Takes `catalogId`. Returns scan status, photo count, capture date range, and available camera breakdown. |
| `list_photos` | Takes `catalogId` and optional `limit` and `cursor`. Returns photo IDs, filenames, capture dates, and camera models. Defaults to 50 per page, with a range of 1–100. |
| `request_catalog_setup` | Takes `catalogHint` (exact catalog name or absolute folder path), `jobType: "burstAnalysis"`, and `requestId`. Reuses eligible authorization or opens RawCull for folder selection and approval. |
| `get_catalog_setup_status` | Takes `requestId`. Reports setup state and returns `catalogId` and `jobId` when analysis is queued. |
| `start_catalog_analysis` | Takes `catalogId` and `requestId`. Queues analysis for a catalog with saved analysis permission and returns the request UUID as `jobId`. |
| `get_analysis_status` | Takes `catalogId` and `jobId`. Reports job state, stage, progress, and coverage at completion. |
| `cancel_analysis` | Takes `catalogId` and `jobId`. Requests cancellation of the current analysis. |
| `list_bursts` | Takes `catalogId`, `jobId`, and optional `limit` and `cursor`. Returns completed rankings, including recommended frames, alternates, confidence, and available evidence. Defaults to 10 bursts per page, with a range of 1–25. |
| `start_photo_assessment` | Takes `catalogId`, `sourceJobId` (the completed analysis job ID), `requestId`, `rubric`, and exactly one of `burstId` or `photoIds`. Queues a local Qwen assessment. |
| `get_photo_assessment_status` | Takes `requestId`. Reports state, cancellation, whether the result was cleared, and any failure code. |
| `get_photo_assessment_result` | Takes `requestId`. Returns saved findings and per-photo errors. Burst assessments also include original ranks for the selected photos and the full burst frame count. |
| `cancel_photo_assessment` | Takes `requestId`. Cancels queued assessment work or requests cancellation of a running assessment. |

After requesting cancellation, poll the corresponding status tool until the job
or assessment reaches a final state.

## Sample prompts

These examples are adapted from RawCull's repository prompt guide. Replace the
placeholders before sending them. For follow-ups, stay in the same chat or
provide the exact source IDs.

### Check the connection and catalog access

> Use RawCull MCP to call `rawcull_status` and `list_catalogs`. Report the helper
> and app versions, transport, whether photo assessments and `burstId` are
> supported, the assessment setting if reported, saved catalog permissions, and
> any tool errors.

### Analyze a new catalog

> Use RawCull MCP to analyze `<absolute folder path>` for bursts. Show the setup
> request ID and keep checking while I approve in RawCull if prompted. Follow the
> queued job to completion. Report scanned, analyzed, and skipped counts, the
> similarity backend, and up to five recommendation and alternate pairs with
> confidence. Include the catalog and job IDs.

### Analyze a catalog and assess the closest calls

> Use RawCull MCP to analyze `<absolute folder path>` for bursts and complete
> the AI photo assessments in the same run. If RawCull asks for approval, show
> the setup request ID and keep checking until a job is queued or setup ends.
> Wait for the analysis to complete, then read every `list_bursts` page. Select
> up to five bursts with distinct recommendations and alternates, starting with
> the lowest confidence and keeping the returned order for ties.
>
> For each selected burst, call `start_photo_assessment` with the `catalogId`,
> the analysis job ID as `sourceJobId`, the `burstId`, a fresh request UUID, and
> the rubric `["composition", "exposure", "subjectVisibility"]`. Reuse the UUID
> if retrying the same request. Poll `get_photo_assessment_status` every two
> seconds until the assessment reaches a final state. On completion, retrieve
> `get_photo_assessment_result` before starting the next assessment.
>
> Report the total burst count, each assessed pair's filenames, original ranks,
> scores out of 5, and findings. Suggest a keeper where the findings support
> one. Include the catalog, source job, and assessment request IDs, and report
> any failures or if there were no eligible pairs.

### Review the closest calls

> Use the completed burst job from this chat. Read all ranking pages and show up
> to five low-confidence recommendation and alternate pairs, prioritizing ties
> and the closest recorded scores. Include burst IDs, filenames, confidence,
> reported reasons or cautions, and what to inspect. Report how many you found.
> Use read-only tools.

### Compare the recommendation and alternate

> Read every ranking page from the completed burst job in this chat. Pick the
> lowest-confidence burst with two distinct saved picks, preserving ranking
> order for ties. Assess it using `burstId` and the rubric
> `["composition", "exposure", "subjectVisibility"]`. Wait for the result and
> report the burst ID, frame count, filenames, original ranks, scores out of 5,
> findings, and errors. Recommend a keeper if the findings support one. Include
> the source job and assessment request IDs.

### Assess exposure for specific photos

> Assess exposure for `<up to four filenames>` from the completed burst job.
> Resolve their photo IDs from the rankings and ask me about any missing or
> ambiguous matches. Use `photoIds` and the rubric `["exposure"]`. Wait for the
> result and report each filename's score out of 5, supporting findings, and any
> errors. Include the source job and assessment request IDs.

For help reviewing rankings in RawCull, see
[Similarity, Bursts, and Search](/docs/similarity/).
