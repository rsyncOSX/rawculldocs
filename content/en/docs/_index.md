---
title: RawCull
linkTitle: Documentation
menu: { main: { weight: 20 } }
lastmod: 2026-11-11
---

Use RawCull to decide which RAW photos to keep before editing. You can preview and rate photos, compare similar frames, and copy your picks to an editing folder. RawCull leaves the originals untouched.

RawCull requires **macOS 27 (Golden Gate)** and an **Apple Silicon Mac**. The main workflow in this guide is based on RawCull **3.4.5** (build **442**). See [Release Notes](/blog/releases/) for changes by version.

## Install

Download RawCull from the [Apple App Store](https://apps.apple.com/no/app/rawcull/id6759362764?mt=12).

## Quick Start

1. Select **Add Catalog** and choose a folder of RAW files.
2. Review the photos in **Loupe** or **Grid** view.
3. Press `P` to keep, `X` to reject, or `2`–`5` to rate a photo.
4. Use **Sharpness**, **Similarity**, or **AI Analysis** when you need help comparing candidates.
5. Select **Copy**, choose all rated photos or a minimum rating from 2 to 5, and choose your editing folder. Check the **Dry run** result before copying.

Ratings, sharpness results, and catalog state are saved automatically on your Mac.

## Main Views

| View | Purpose |
|---|---|
| Loupe | Browse a list and inspect one photo at a time |
| Grid | Rate, filter, and select many thumbnails |
| Similarity | Analyze bursts, review suggested frames, and compare close-ups |
| Semantic Search | Find locally indexed photos using a written description |
| AI Analysis | Review selected or rated photographs with SAM 3 + CLIP, Qwen Vision, or Objects |
| Combined Review | Compare 1–8 finalists using the full photo, close-ups, and AI results. Enable it in AI settings. |
| Rated | Show photos with saved culling data |
| Compare | Inspect up to four selected photos closely |

## Supported Files

Starting with **RawCull 3.3.5**, catalogs accept more RAW formats, including Adobe DNG, Canon CR2/CR3/CRW, Fujifilm RAF, Olympus/OM System ORF, Panasonic RW2, and others alongside Sony ARW and Nikon NEF.

RawCull can read embedded previews directly from ARW, DNG, and NEF files. For other recognized formats, it uses macOS to load previews. Preview and RAW development support can vary by camera and macOS version. See [Supported RAW Files](/docs/raw-support/) to check your files.

## Find the Right Guide

| Task | Guide |
|---|---|
| Check camera formats and preview compatibility | [Supported RAW Files](/docs/raw-support/) |
| Rate, compare, copy RAW files, or export JPGs | [Culling Photos](/docs/culling/) |
| Understand sharpness rankings | [Sharpness Scoring](/docs/sharpness/) |
| Check detected detail and camera autofocus locations | [Focus Mask](/docs/focuspeaking/) and [Focus Points](/docs/focuspoints/) |
| Review bursts or search with a description | [Similarity, Bursts, and Search](/docs/similarity/) |
| See a burst analysis run from Codex | [Model Context Protocol (MCP)](/docs/mcp/) |
| Set up local AI and review selected photos | [AI Analysis](/docs/ai/) |
| Adjust preferences or manage previews and memory | [Settings](/docs/settings/), [Cache](/docs/cache/), and [Memory Pressure](/docs/memorypressure/) |
| Understand folder permissions and local storage | [Security & Privacy](/docs/security/) |
| See how RawCull measures, groups, and ranks photos | [Technical Documentation](/docs/technical/) |

## If Something Is Missing

- **No focus marker:** check [Focus Points](/docs/focuspoints/). Not every file contains a supported autofocus location.
- **No developed RAW preview:** use the embedded JPG preview. RAW development depends on whether your version of macOS can read the camera’s RAW data.
- **Similarity or search unavailable:** check the CLIP model in [AI settings](/docs/settings/), then index the catalog as described in [Similarity, Bursts, and Search](/docs/similarity/).
- **Slow browsing or memory warnings:** check [Cache](/docs/cache/) and [Memory Pressure](/docs/memorypressure/).

For a bug report, include the app version, macOS version, camera model, file format, and steps that reproduce the problem. Contact details are on the [About](/about/) page.
