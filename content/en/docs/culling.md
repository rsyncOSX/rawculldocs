+++
author = "Thomas Evensen"
title = "Culling Photos"
date = "2026-07-15"
weight = 10
tags = ["culling"]
categories = ["user doc"]
lastmod = "2026-11-11"
+++

RawCull records decisions without changing source photos. Add a folder as a catalog, then use **Loupe** to review one photograph at a time or **Grid** to work with many photographs.

## Rate and Navigate

| Key | Action |
|---|---|
| `P` or `0` | Mark as keeper |
| `X` | Reject |
| `2`–`5` | Set star rating |
| `T` | Set the default 3-star rating |
| Arrow keys | Previous or next photo |
| `Z` | Open the embedded JPG at 100% zoom |

These shortcuts apply to the catalog culling workflow. In Burst Review, `P` means previous frame; use the shortcut help for the active view.

A rating key saves your decision and moves to the next photo. Use the colored toolbar controls to filter which photos you see. Your ratings stay the same.

In Grid view, use Command-click to select separate photographs or Shift-click to select a range. A rating key applies to the selection. Select two or more photographs and choose **Compare** to inspect up to four together.

## Inspect a Photo

Double-click a thumbnail to open the full-window viewer. From there you can zoom, rate, show focus aids, and switch between the embedded JPG and a developed RAW preview when supported.

While a developed preview is being prepared, the RAW control shows progress. The overlay reports RAW detail level and render-request time.

Open the information panel to see the histogram, review scores, file and camera details, and quick actions for the selected photograph.

Useful viewer keys are `+`/`-` for zoom, `J` for embedded JPG, `R` for developed RAW, `F` for focus mask, `A` for focus point, and Escape to close.

## Copy Selected RAW Files

Choose **Copy**, select a destination, and copy either all rated files or files at or above a rating from 2 to 5. **Dry run** is enabled initially so you can check the result before copying.

RawCull uses the system’s rsync tool to copy files. It keeps the originals and leaves newer files in the destination folder alone.

## Export JPGs

Select one or more photos and choose **Actions > Extract JPGs** (`Command-J`). Choose a destination folder and an export option.

- **Embedded JPG:** Saves the preview stored inside an ARW, NEF, or DNG file, if available. RawCull reads it directly from the file and may adjust its orientation for export.
- **Developed JPG:** Creates a JPG from the RAW data when macOS supports the file. The export can use a different decoder version from the viewer’s **RAW 9** mode.

Embedded exports use the original filename with a JPG extension. Developed exports add `_demosaic`. If the destination already has a file with that name, RawCull adds a number. See [Supported RAW Files](/docs/raw-support/) for compatibility.

For larger catalogs or difficult comparisons, use [Sharpness Scoring](/docs/sharpness/), [Similarity and Bursts](/docs/similarity/), or [AI Analysis](/docs/ai/).

## EV Analysis

In the full-window viewer, choose **EV analysis** to inspect highlights and shadows in a developed RAW preview. Adjust **Exposure** from −5 to +5 stops. The preview updates when you release the slider. **Reset** returns to 0 EV. **Close** restores the previous RAW settings.

**Clipping warning** marks near-white areas red and near-black areas blue. Both the warning and the histogram describe the preview. Use your RAW editor to check how much highlight or shadow detail you can recover. EV analysis leaves the original and exported JPGs untouched. Close EV analysis to use rating and navigation shortcuts again.
