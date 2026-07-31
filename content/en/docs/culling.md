+++
author = "Thomas Evensen"
title = "Culling Photos"
date = "2026-07-15"
weight = 2
tags = ["culling"]
categories = ["user doc"]
+++

RawCull records decisions without changing the source photos. Add a folder as a catalog, then use **Loupe** for one-photo review or **Grid** for a broader overview.

## Rate and Navigate

| Key | Action |
|---|---|
| `P` or `0` | Mark as keeper |
| `X` | Reject |
| `2`–`5` | Set star rating |
| `T` | Set the default 3-star rating |
| Arrow keys | Previous or next photo |
| `Z` | Open the embedded JPG at actual-pixel view |

A rating key saves the decision and advances to the next photo. The colored controls in the toolbar filter what is shown; they do not change ratings.

In Grid view, use Command-click to select separate photos or Shift-click to select a range. A rating key applies to the selection. Select two or more photos and choose **Compare**; RawCull compares up to the first four selected photos.

## Inspect a Photo

Double-click a thumbnail to open the full-window viewer. From there you can zoom, rate, show focus aids, and switch between the embedded JPG and a developed RAW preview when supported.

Useful viewer keys are `+`/`-` for zoom, `J` for embedded JPG, `R` for developed RAW, `F` for focus mask, `A` for focus point, and Escape to close.

## Copy Selected RAW Files

Choose **Copy**, select a destination, and choose either all rated files or a minimum rating from 2 to 5. **Dry run** is enabled initially so you can check the result before copying.

RawCull copies files with the system rsync tool. It does not delete source files, and existing newer destination files are not overwritten.

## Export JPGs

Select one or more photos and use **Actions -> Extract JPGs** (`Command-J`). You can export the embedded JPG or, for supported Sony files, a demosaiced RAW JPEG. Choose a destination folder before starting.

Use [Sharpness Scoring](/docs/sharpness/) and [Similarity](/docs/similarity/) for large catalogs or burst sequences.
