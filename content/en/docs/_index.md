---
title: RawCull
linkTitle: Documentation
menu: { main: { weight: 20 } }
---

RawCull is a native macOS app for reviewing RAW photos before editing. It shows fast previews and camera information, records picks and ratings, groups similar frames, and copies the selected files to an editing folder.

This guide describes the RawCull 3.2 workflow. It requires **macOS 27 (Golden Gate)** and an **Apple Silicon Mac**. Optional AI features **run locally** for semantic search, visual similarity, and deeper analysis of selected photographs. See [Release Notes](/blog/releases/) for version-specific changes; a development changelog does not confirm App Store availability.

<div class="alert alert-secondary" role="alert">

RawCull is a photo-culling app, not a photo editor. Its tools are designed to help you decide which photographs to keep.

RawCull does not edit or delete the source photos.

</div>

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
| Similarity | Analyze bursts and review suggested frames |
| Semantic Search | Find locally indexed photos using a written description |
| AI Analysis | Review selected or rated photographs with SAM 3 + CLIP, Qwen Vision, or Objects |
| Rated | Show photos with saved culling data |
| Compare | Inspect up to four selected photos closely |

## Requirements and Supported Files

- macOS 27 (Golden Gate)
- Apple Silicon Mac
- Sony ARW catalogs
- Nikon NEF catalogs (experimental)

Sony ARW is the primary format. Some functions depend on camera metadata and the RAW support available in macOS. Demosaiced RAW previews and exports are Sony-specific; RawCull uses embedded previews when RAW development is unavailable.

## Find the Right Guide

| Task | Guide |
|---|---|
| Rate, compare, copy RAW files, or export JPGs | [Culling Photos](/docs/culling/) |
| Understand sharpness rankings | [Sharpness Scoring](/docs/sharpness/) |
| Check detected detail and camera autofocus locations | [Focus Mask](/docs/focuspeaking/) and [Focus Points](/docs/focuspoints/) |
| Review bursts or search with a description | [Similarity, Bursts, and Search](/docs/similarity/) |
| Review a small set with local AI | [AI Step by Step](/docs/ai/aistepbystep/) |
| Understand the models, downloads, and licences | [AI Analysis](/docs/ai/aianalysis/) |
| Read implementation details, formulas, and evidence pipelines (tech docs) | [Technical Documentation](/docs/technical/) |
| See the interface | [Screenshots](/docs/screenshots/screenshots/) and [AI Screenshots](/docs/screenshotsai/screenshotsai/) |
| Adjust preferences or manage previews and memory | [Settings](/docs/settings/), [Cache](/docs/cache/), and [Memory Pressure](/docs/memorypressure/) |
| Understand folder permissions and local storage | [Security & Privacy](/docs/security/) |

## If Something Is Missing

- **No focus marker:** check [Focus Points](/docs/focuspoints/). Not every file contains a supported autofocus location.
- **No developed RAW preview:** use the embedded JPG preview. Development support depends on the camera file and the installed macOS decoder.
- **Similarity or search unavailable:** check the CLIP model in [AI settings](/docs/settings/), then index the catalog as described in [Similarity, Bursts, and Search](/docs/similarity/).
- **Slow browsing or memory warnings:** check [Cache](/docs/cache/) and [Memory Pressure](/docs/memorypressure/).

For a bug report, include the app version, macOS version, camera model, file format, and steps that reproduce the problem. Contact details are on the [About](/about/) page.
