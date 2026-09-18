---
title: RawCull Documentation
linkTitle: Documentation
menu: { main: { weight: 20 } }
---

## RawCull

RawCull is a native macOS app for reviewing RAW photos before editing. It shows fast previews and camera information, records picks and ratings, groups similar frames, and copies the selected files to an editing folder.

The current version requires **macOS 27 (Golden Gate)** and an **Apple Silicon Mac**. Optional AI features run locally for semantic search, visual similarity, and deeper analysis of selected photographs.

<div class="alert alert-secondary" role="alert">

RawCull is a photo-culling app, not a photo editor. Its tools are designed to help you decide which photographs to keep.

RawCull does not edit or delete the source photos.

</div>

## Install

Download RawCull from the [Apple App Store](https://apps.apple.com/no/app/rawcull/id6759362764?mt=12). See the [GitHub releases](https://github.com/rsyncOSX/RawCull/releases) for detailed release notes.

## Quick Start

1. Select **Add Catalog** and choose a folder of RAW files.
2. Review the photos in **Loupe** or **Grid** view.
3. Press `P` to keep, `X` to reject, or `2`–`5` to rate a photo.
4. Use **Sharpness**, **Similarity**, or **AI Analysis** when you need help comparing candidates.
5. Select **Copy** to copy photos rated 2 stars or higher to your editing folder.

Ratings, sharpness results, and catalog state are saved automatically on your Mac.

## Main Views

| View | Purpose |
|---|---|
| Loupe | Browse a list and inspect one photo at a time |
| Grid | Rate, filter, and select many thumbnails |
| Similarity | Analyze bursts and review suggested frames |
| Semantic Search | Find locally indexed photos using a written description |
| AI Analysis | Run SAM 3 + CLIP or Qwen on selected or rated photographs |
| Rated | Show photos with saved culling data |
| Compare | Inspect up to four selected photos closely |

## Requirements and Supported Files

- macOS 27 (Golden Gate)
- Apple Silicon Mac
- Sony ARW catalogs
- Nikon NEF catalogs (experimental)

Sony ARW is the primary format. Some functions depend on camera metadata and the RAW support available in macOS. Demosaiced RAW previews and exports are Sony-specific; RawCull uses embedded previews when RAW development is unavailable.

## Learn More

- [Cull and copy photographs](/docs/culling/)
- [Compare sharpness](/docs/sharpness/)
- [Group similar frames and search a catalog](/docs/similarity/)
- [Use local AI analysis](/docs/ai/)
- [View the screenshot tour](/docs/screenshots/)
