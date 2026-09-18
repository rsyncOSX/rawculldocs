---
title: RawCull Documentation
linkTitle: Documentation
menu: { main: { weight: 20 } }
---

## RawCull

RawCull is a native macOS app for reviewing RAW photos before editing. It scans a folder, shows fast previews and camera information, records your picks and ratings, compares similar frames, and copies the selected files to another folder.

The latest released version is only supported on **macOS Golden Gate**. RawCull adds optional local AI for semantic search, visual similarity, and subject-aware burst review. 

<div class="alert alert-secondary" role="alert">

RawCull is solely a photo-culling app; it has no photo-editing features, nor are any planned, and all future additions will support the culling workflow.

RawCull does not edit or delete the source photos.

</div>

## Install

RawCull is only available for download from the [Apple App Store](https://apps.apple.com/no/app/rawcull/id6759362764?mt=12).   There are also some more  [detailed release notes on GitHub](https://github.com/rsyncOSX/RawCull/releases).

## Quick Start

1. Select **Add Catalog** and choose a folder of RAW files.
2. Review the photos in **Loupe** or **Grid** view.
3. Press `P` to keep, `X` to reject, or `2`–`5` to rate a photo.
4. Use **Sharpness** and **Similarity** when you need help comparing many frames.
5. Select **Copy** to copy photos rated 2 stars or higher to your editing folder.

Ratings, sharpness results, and catalog state are saved automatically on your Mac.

## Main Views

| View | Purpose |
|---|---|
| Loupe | Browse a list and inspect one photo at a time |
| Grid | Rate, filter, and select many thumbnails |
| Similarity | Analyze bursts and review suggested frames |
| Semantic Search | Find locally indexed photos using a written description in RawCull 3 |
| Rated | Show photos with saved culling data |
| Compare | Inspect up to four selected photos closely |

## Requirements and Files

- macOS 27 Golden Gate
- Apple Silicon Mac
- Sony ARW and Nikon NEF catalogs, NEF support is experimental

Sony ARW is the primary format. Some functions depend on the camera metadata and RAW support available in macOS. Demosaiced RAW preview and export are Sony-specific; embedded previews are used when RAW development is unavailable.

