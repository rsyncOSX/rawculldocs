+++
author = "Thomas Evensen"
title = "Version 3.4.9"
date = "2026-11-23"
tags = ["changelog", "version 3.4.9"]
categories = ["changelog"]
+++

Download RawCull from the [Apple App Store](https://apps.apple.com/no/app/rawcull/id6759362764?mt=12).

RawCull 3.4.9 makes **Review** the single in-app AI review, adds a bundled **MCP server** for desktop AI clients, and lets you run an **AI assessment** of the top two frames of every burst. Compared with [3.4.5]({{< relref "version-3.4.5.md" >}}), every review result now ends in a decision, the legacy AI Analysis tabs are gone, and burst ranking, focus scoring, and Vision work are more reliable.

## One AI review: Review

- **Review** replaces the AI Analysis view and its SAM 3 + CLIP, Qwen Vision, and Objects tabs. It is always in the toolbar; the earlier opt-in toggle is removed.
- Choose selected or tagged photos and one of three presets:
  - **Focus check**: SAM 3 subject masks, focus measurement, and CLIP. It never loads Qwen and works without the Qwen model installed.
  - **Describe**: one Qwen assessment of the whole photo.
  - **Full**: every stage, including per-object crops and a written report.
- Describe and Full require at least 16 GB of memory; on smaller Macs they are disabled and Review starts on Focus check. Below 32 GB they show a memory note. Saved runs without a preset load as Full.
- Focus check and Full also supply the subject outline (the **S** key) and the mask badge on thumbnails in the loupe, zoom, and burst workspace.

## Decide from the result

- Every result has a decision bar: **Tag/Untag**, the **X**, **P**, and **2–5** ratings, and **Mark Reviewed** when the photo is in a burst. The grid's rating keys work in Review.
- An action runs only while the photo is in the open catalog, has a unique filename, and is unchanged on disk since the review. The same actions are used in the AI Assessments window.
- **Untag** no longer erases a star rating. It clears only a two-star tag; for 3–5 stars the button reads **Tagged** and is disabled.
- Running, restoring, or reopening a review never changes ratings or review state.

## A clearer Review workspace

- The photo stays in place while the panel beside it drills down from the summary to a finding and then to its region. The selected finding's region is highlighted on the photo, and layer toggles show the regions, subject mask, and crop geometry. Use **↑** and **↓** to step through findings and **Esc** to go back.
- While a review runs, a card on the photo shows the current step and a progress bar for that photo's tasks. While a photo is still being reviewed, the summary says so instead of reporting missing evidence.
- **Coverage & limits**, the technical record, and run history open in a resizable **Review Record** window that follows the photo selected in Review.
- Coverage & limits groups limitations into plain sentences about the image, subject, regions, dropped findings, and review steps, with repeats counted. The original pipeline wording stays behind **Show technical wording**.
- Removed the **Enlarge review image** button, which often showed a smaller image than the inline preview.

## AI assessment of every burst

- The burst list has an **AI assessment** card that runs the local Qwen assessment on each burst's recommended and alternate frame. It is available when the burst lists are ready and no other heavy or AI work runs, shows progress, and can be cancelled.
- Results are kept for the current session and shown in the AI Assessments window. MCP jobs wait while the assessment runs.

## RawCull MCP server (in development)

- RawCull bundles a signed, local Model Context Protocol helper, `rawcull-mcp`, for compatible desktop AI clients such as Codex. It communicates over stdio; there is no network port.
- An AI client can list authorized catalogs, read catalog summaries and photo lists, start and cancel burst analysis, poll progress, and page through ranked bursts with recommendation, alternate, confidence, reasons, cautions, and per-frame evidence. Results stay readable after RawCull quits while they remain shared.
- You choose which catalog to share and whether the client may start analysis in **Settings → MCP**. Each job asks for approval (**Approve This Job** or **Approve and Remember**). The AI client may send returned metadata and results to its configured model provider.
- MCP follow-up photo assessments run Qwen locally under the shared AI lease, with typed stop reasons for timeouts, model changes, and cancellation.
- The helper recovers interrupted and damaged job records without failing live workers, bounds setup storage and replay history, validates pagination and metadata, and uses nonblocking job locks.

## Burst ranking and focus accuracy

- Camera AF points (Sony FocusLocation and EXIF SubjectArea) are rotated into the upright image frame once when metadata loads, so portrait frames score, mask, and report AF location correctly everywhere. Sharpness scores computed from the old sensor-space points are recomputed.
- RAW Demosaic scoring renders are encoded to sRGB through Core Image colour management and use one shared decoder that prefers RAW 9.
- Subject-detail scoring is normalized for resolution and ISO, so sensor noise no longer favors a noisier frame. Cached measurements are recomputed.
- The focus-mask threshold is calibrated on the embedded preview the masks are drawn from, and only the active calibration run can apply its result.
- Sharpness scores are reused only when computed under the same settings signature.
- Cancelled grouping runs never publish partial burst groups; a sensitivity change during an active run is replayed when it finishes.
- Unsaved review marks win over the saved burst cache, and malformed burst caches are rejected instead of crashing the app.
- Superseded similarity indexing requests now finish indexing their uncovered files instead of leaving bursts without embeddings.

## Stability and performance

- Every Vision request runs through one shared request gate (PhotoAnalysisKit 1.4.0 and PhotoAIKit 3.1.0). This fixes a hang where concurrent sharpness scoring, feature prints, and face detection could block every worker thread. Full sharpness-scoring concurrency is restored with identical scores.
- Face and animal detection for Close-ups and People Ranking runs off the shared loader, behind a two-pass limiter.
- Heavy image work (full-size JPEG decodes, sidecar loads, thumbnail decodes, preview sharpening, TIFF conversions) stops when its caller is cancelled, for example after a catalog switch.
- The analysis artifact store no longer decodes every record on each write. A similarity record that cannot be read temporarily is kept instead of being deleted.
- Waiting for the review lease no longer busy-spins the main thread, and segmentation stages whose retained masks were evicted are re-run on resume.
- A changed preview thumbnail size takes effect without relaunching.

## Under the hood

- Split the main view model into focused slices and controllers (catalog, selection, ratings, burst workflow, zoom, loupe viewport, navigation, presentation, and file operations), with views reading them from the environment.
- Removed the legacy AI Analysis code, Deep AI Review, and the unreachable saved-files views. The Objects mask cache is deleted from disk at launch.
- Fixed `try?` uses that hid failures in cache keys, mask storage, and review cleanup; cache settings show **Unavailable** instead of 0 B on failure.
- Updated PhotoAnalysisKit to 1.4.0, PhotoAIKit to 3.1.0, and PhotoEvidenceKit to 1.1.5, and added the MCP Swift SDK 0.12.1.
- Added regression coverage for AF orientation, mask regeneration, model swaps, burst caches, review actions, MCP controller transitions, and the MCP helper. Documented the Metal `MDB_MAP_FULL` console messages and how to clear them.

These notes cover the changes from the `v3.4.5` tag through commit `bf7516c7` (RawCull 3.4.9, build 466).
