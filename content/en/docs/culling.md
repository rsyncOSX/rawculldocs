+++
author = "Thomas Evensen"
title = "Culling Photos"
date = "2026-07-15"
weight = 10
tags = ["culling"]
categories = ["user doc"]
lastmod = "2026-10-25"
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
| `Z` | Open the embedded JPG at actual-pixel view |

These shortcuts apply to the catalog culling workflow. In Burst Review, `P` means previous frame; use the shortcut help for the active view.

A rating key saves the decision and advances to the next photograph. The colored toolbar controls filter what is shown; they do not change ratings.

In Grid view, use Command-click to select separate photographs or Shift-click to select a range. A rating key applies to the selection. Select two or more photographs and choose **Compare** to inspect up to four together.

## Inspect a Photo

Double-click a thumbnail to open the full-window viewer. From there you can zoom, rate, show focus aids, and switch between the embedded JPG and a developed RAW preview when supported.

Open the information panel to see the histogram, culling evidence, file and camera details, and quick actions for the selected photograph.

Useful viewer keys are `+`/`-` for zoom, `J` for embedded JPG, `R` for developed RAW, `F` for focus mask, `A` for focus point, and Escape to close.

## Copy Selected RAW Files

Choose **Copy**, select a destination, and copy either all rated files or files at or above a rating from 2 to 5. **Dry run** is enabled initially so you can check the result before copying.

RawCull copies files with the system rsync tool. It does not delete source files, and existing newer destination files are not overwritten.

## Export JPGs

Select one or more photos and use **Actions > Extract JPGs** (`Command-J`). You can export an embedded JPG from ARW, NEF, or DNG when a usable embedded JPEG is present, or a demosaiced RAW JPEG when the installed macOS decoder supports the file. Embedded export reads the RAW file itself, rather than a sidecar or display cache; orientation may be normalized for a standalone JPEG. Other ImageIO-backed formats can be browsed without supporting direct embedded-JPG export. The viewer’s **RAW 9** label describes its selected decoder and does not promise that export uses the same decoder version. Choose a destination folder before starting.

For larger catalogs or difficult comparisons, use [Sharpness Scoring](/docs/sharpness/), [Similarity and Bursts](/docs/similarity/), or [AI Analysis](/docs/ai/).

## EV Analysis

In the full-window viewer, choose **EV analysis** to inspect highlights and shadows in a developed RAW preview. Adjust **Exposure** from −5 to +5 EV; the preview is developed when you release the slider. **Reset** returns to 0 EV. **Close** restores the previous RAW settings.

**Clipping warning** marks near-white preview pixels red and near-black pixels blue. The **Preview histogram** describes the displayed sRGB preview. These are preview warnings, not measurements of RAW sensor clipping or proof that detail is unrecoverable. EV analysis does not edit the source photo or change exported JPGs. Normal rating and navigation shortcuts are suspended during this inspection.

Exported embedded previews use the source basename; developed exports add `_demosaic`. If a destination filename already exists, RawCull adds a numbered suffix instead of overwriting it. See [Supported RAW Files](/docs/raw-support/) for compatibility limits.
