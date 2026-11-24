+++
author = "Thomas Evensen"
title = "Settings"
date = "2026-07-15"
weight = 80
tags = ["settings"]
categories = ["user doc"]
lastmod = "2026-11-24"
+++

Open **RawCull > Settings** to change how the app previews, scores, and stores photos and results.

| Tab | Main controls |
|---|---|
| Cache | View memory and disk cache use; delete cache data or separately delete saved catalog records and ratings |
| Thumbnails | Set list and preview sizes; enable and adjust sharpened RAW zoom previews |
| Focus | Adjust the focus-mask threshold, pre-blur, amplification, erosion, and dilation |
| AI | Check which models are ready, manage downloads, and choose the similarity model |
| [MCP](#mcp) | Manage catalog access, shared burst results, and photo assessments for MCP clients such as Codex |
| Memory | See how much memory your Mac and RawCull are using |

**Sharpen Zoom Preview** uses macOS to develop the RAW file and sharpen fine detail. It is slower than using the embedded JPG and may not be available for every file.

Use **Save Settings** after changing thumbnail or focus values. **Reset to Defaults** restores the values in that settings area. Scoring options are available from **Scoring Parameters** in the main window.

**Settings > AI** reports whether DataComp CLIP, SAM 3, and Qwen are available. Turn on **Use selected CLIP model for similarity** before indexing a catalog with CLIP for similarity and semantic search. **Download AI Models** opens the model manager, and **Check Again** refreshes model status.

AI features require macOS 27 (Golden Gate) and an Apple Silicon Mac. See [AI Analysis](/docs/ai/) for the purpose of each model.

## MCP

Open **Settings → MCP** to control what an MCP client such as Codex can access
and run. The tab shows the authorized catalog and the selected catalog.

### Catalog access

Select a catalog and let it finish loading. Choose **Authorize Selected
Catalog** to share its saved photo metadata. Before authorizing, turn on
**Allow MCP burst analysis and photo assessments** if you want the client to
start analysis. For a folder you reuse across shoots, also turn on **Allow
analysis of future photos in this folder**. RawCull then rescans the folder for
each new job without asking for approval again.

**Revoke Access** removes the saved catalog authorization and the current job,
including its shared results.

### Shared results and photo assessments

- **Clear Last Shared Result** removes the shared burst rankings, coverage,
  and dependent assessments. Saved catalog authorization and photo metadata
  stay available.
- **Enable MCP photo assessments** controls whether the client can request
  Qwen follow-ups and read their findings. Turning it off cancels active
  assessments and clears their shared findings. Catalog access and burst
  rankings stay available. Run a fresh burst analysis after turning it back on.
- **Photo assessments** shows recent requests. Use **Cancel** for queued or
  running assessments and **Clear** to remove an individual assessment result.

An MCP client may retain copies it already received. For connection steps and
example prompts, see [Model Context Protocol (MCP)](/docs/mcp/).
