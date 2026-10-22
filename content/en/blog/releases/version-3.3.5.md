+++
author = "Thomas Evensen"
title = "Version 3.3.5 — In Development"
date = "2026-10-22"
tags = ["changelog", "version 3.3.5"]
categories = ["changelog"]
summary = "In development, not yet released: RAW exposure inspection, AF-point sharpness sorting, a combined Single Images review group, and simpler cache management."
+++

**In development — version 3.3.5 has not yet been released.** These notes describe the current source changes and may change before release.

This update adds RAW exposure inspection and AF-point sharpness sorting, brings single photographs together for review, and simplifies cache and saved-data management.

**Changes:** v3.3.3 → v3.3.5 development

This changelog continues from the [3.3.3 release notes]({{< relref "version-3.3.3.md" >}}). It covers the 32 commits after the `v3.3.3` release tag through the source snapshot below, rather than the earlier development snapshot described in that post.

**Commit range:** `v3.3.3..ad5ac9af`  
**Baseline tag:** `v3.3.3` at `e7516156` (2026-10-16), RawCull 3.3.3 Build 423  
**Latest source commit:** `ad5ac9af` (2026-10-22)  
**Current source version:** RawCull 3.3.5 (Build 425)  
**Release status:** In development; not yet released

## RAW Exposure Inspection

- Add **EV analysis** in the zoom viewer to inspect a developed RAW preview with exposure adjustments from **−5 to +5 EV** in 0.1 EV steps. The preview updates when the slider is released, with a Reset action to return to zero.
- Add optional clipping warnings and an expandable RGB preview histogram with near-black and near-white percentages. These describe the tone-adjusted, displayed preview; they do not measure RAW sensor clipping or prove that detail is unrecoverable.
- Restore the previous image source and RAW inspection settings when EV analysis closes, and prevent image navigation during inspection.
- Keep adjusted inspection previews separate from the export/JPEG cache, with background rendering and cancellation handling.

## AF-point Sharpness Sorting

- Add a burst display-order picker for **Best frame** or **AF-point sharpness**. Load missing focus-point measurements when needed and place frames without a usable measurement last.
- Select the first frame when changing sort order and after focus-point measurements finish loading, and refresh the filmstrip for the selected order.
- Stack image dimensions, rating, suggestion, and subject badges vertically in the burst viewer.

## Single Images and Burst Review

- Display photographs in the **Single Images** queue together in one review group, with an image count and a shared filmstrip.
- Open that group in the review workspace without assigning a burst winner or ranking to unrelated photographs.
- Apply **Reviewed** and **Deferred** workflow states to the constituent single-image groups, preserving those states in saved review data.
- Read review status from the stored workflow state even when a group has no analysis result.
- Double-click a thumbnail in the grouped grid to open its group for review.

## Cache and Saved-data Management

- Simplify **Settings > Cache** with a total cache size and a compact breakdown of preview, grid thumbnail, disk thumbnail, full-size JPG, similarity artifact, burst group, and Combined Review storage.
- Replace individual cache deletion controls with one **Delete Cache** action and a confirmation explaining what will be removed. This includes saved Combined Reviews, masks, retained input images, and review history. Original photographs, settings, and downloaded AI models are kept.
- Add a separate **Saved Files** section showing the size of saved catalog records, ratings, and analysis results, with a confirmed **Delete Saved Files** action. Original photographs are kept.
- Move the saved burst similarity evidence display from AI settings to Cache settings, retaining the CLIP/Vision breakdown and information about unreadable or outdated cache files.
- Remove the **Show Saved Files** command from the File menu.

## Image Review Presentation

- Replace the on-image rating action bars with a compact **Current rating** badge across review views. The badge uses an icon, label, and color to show unrated, rejected, keeper, and star ratings, with an accessible rating description.
- Move image pixel dimensions beside the current rating badge in the zoom overlay.
- Remove the extra zoom hint and rating action bar from the zoom overlay controls to leave more space for the photograph.

## Broader RAW Support with RawParserKit

- Update [RawParserKit](https://github.com/rsyncOSX/RawParserKit) from **1.3.1** to **1.3.2**, adding generic RAW handling for more camera brands alongside the dedicated Sony ARW, Nikon NEF, and DNG handlers.
- Recognize additional formats including Canon CR2/CR3/CRW, Fujifilm RAF, Olympus / OM System ORF, Panasonic RW2, Pentax / Ricoh PEF/PTX, and formats from Samsung, Sigma, Phase One, Hasselblad, Kodak, Minolta, Epson, Mamiya, Leaf, Leica, and Casio, plus Nikon NRW and legacy Sony SR2/SRF.
- These additional formats use macOS ImageIO for thumbnails and embedded previews. Availability depends on the installed macOS camera support and the previews exposed by each file; recognizing an extension does not guarantee support for every camera or compression mode.

### Why Direct Sony ARW Parsing Helps

Sony ARW keeps its dedicated parser. RawParserKit can read the file's TIFF structure and Sony MakerNotes to locate the camera's embedded JPEG previews and autofocus information directly. For preview-based culling, this provides a fallback when Apple's ImageIO cannot expose a usable preview and avoids requiring Core Image to develop the sensor data just to display the photograph.

That makes preview access less dependent on Apple's support for a particular Sony camera or RAW compression mode. It also uses the camera-rendered JPEG, preserving the camera's rendering for that preview, while the Sony MakerNote parser supplies focus information that generic image loading may not expose.

This reliability advantage applies to embedded previews and supported metadata. Developing the RAW sensor data, including creating a new full-size JPEG from it, still uses Apple's Core Image RAW decoder and depends on its camera and compression support. The broader format support does not change the dedicated Sony ARW loading path.

## Dependencies and Maintenance
- Advance app and model downloader metadata to **3.3.5 Build 425** and update Xcode project and test scheme configuration.
- Split viewer and analysis screens into focused components, share image overlays and preview preparation, and centralize keyboard monitor and image-loading task lifecycles.
- Add regression coverage for RAW inspection state restoration, viewer keyboard handling, overlay presentation, and AF-point sorting, plus release sharpness comparison tooling.
- Add regression coverage for saving and restoring Single Images workflow states without choosing a winner.
- Apply source formatting and internal cleanup across image loading, catalog handling, and AI review components.

These notes were checked against the Git changes in the stated range. They do not represent a completed release validation or a new test run.
