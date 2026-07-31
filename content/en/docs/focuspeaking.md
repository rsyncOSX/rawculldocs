+++
author = "Thomas Evensen"
title = "Focus Mask"
date = "2026-07-15"
weight = 20
tags = ["focus mask", "focus peaking"]
categories = ["user doc"]
+++

The Focus Mask highlights areas with strong edge detail. Use it in the full-window viewer or comparison view to check where a photo appears sharp.

Press `F` or use the focus-mask control. For a more detailed check, switch from the small thumbnail to the embedded JPG (`J`) or developed RAW preview (`R`) when supported.

Sharpness scoring calibrates the mask threshold for the current catalog. You can fine-tune it in **RawCull -> Settings -> Focus**:

| Control | Effect |
|---|---|
| Threshold | Lower shows more detail; higher keeps only stronger edges |
| Pre-blur | Reduces fine texture and high-ISO noise |
| Amplify | Strengthens the visible mask |
| Erosion | Removes isolated highlighted pixels |
| Dilation | Expands and joins nearby highlighted areas |

The mask is a visual guide. Noise, texture, depth of field, and sharpening in the camera preview can affect the result.
