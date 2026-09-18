+++
author = "Thomas Evensen"
title = "Settings"
date = "2026-07-15"
weight = 40
tags = ["settings"]
categories = ["user doc"]
+++

Open **RawCull > Settings**. The current release provides five settings tabs.

| Tab | Main controls |
|---|---|
| Cache | View memory and disk cache use; clear thumbnail or full-size JPG caches |
| Thumbnails | Set list and preview sizes; enable and adjust sharpened RAW zoom previews |
| Focus | Adjust the focus-mask threshold, pre-blur, amplification, erosion, and dilation |
| AI | Check DataComp CLIP, SAM 3, and Qwen readiness; manage optional model downloads |
| Memory | View unified memory use, RawCull memory use, and macOS pressure |

**Sharpen Zoom Preview** develops the RAW through macOS and applies micro-detail sharpening. It is slower than using the embedded JPG and may not be available for every file.

Use **Save Settings** after changing thumbnail or focus values. **Reset to Defaults** restores the values in that settings area. Scoring options are available from **Scoring Parameters** in the main window.

**Settings > AI** reports whether DataComp CLIP, SAM 3, and Qwen are available. Enable DataComp CLIP before indexing a catalog for similarity and semantic search. **Download AI Models** opens the model manager, while **Check Again** refreshes availability after an installation or removal.

AI features require macOS 27 (Golden Gate) and an Apple Silicon Mac. See [AI Analysis](/docs/ai/) for the purpose of each model.
