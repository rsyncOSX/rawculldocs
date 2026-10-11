---
title: RAW 9 Controls
linkTitle: RAW 9 Controls
type: docs
author: Thomas Evensen
date: 2026-10-11
weight: 20
description: Sony camera support on macOS 27.0.1 and RAW 9 adjustment controls, their ranges and defaults, plus crop, reset, copy, paste, and export actions.
---

## What is RAW 9?

RAW 9 is version 9 of Apple's built-in RAW processing engine. It develops supported camera RAW files into viewable images, using the Apple Neural Engine to improve image detail and color. RawBrowse uses this engine for its RAW 9 preview and editing controls.

Apple explains the engine in [Enhance RAW image processing with Core Image](https://developer.apple.com/videos/play/wwdc2026/305/). Although this presentation is aimed at developers, its introduction describes the improvements in sharpness, color, and processing performance.

For general guidance aimed at users, see Apple's [How the Photos app and iCloud Photos handle RAW images](https://support.apple.com/en-us/104964). That article covers RAW images generally rather than decoder version 9.

## Sony cameras supporting RAW 9

As of **October 11, 2026**, the Images settings panel on **macOS 27.0.1**
reports these **18 Sony cameras** as supporting RAW 9. This list was transcribed
from the supplied settings screenshots.

Camera support is provided by the RAW 9 decoder installed on the Mac and updates
with macOS. Support can vary by RAW file format and camera shooting mode.

- Sony Alpha ILCE-7 II
- Sony Alpha ILCE-7C
- Sony Alpha ILCE-7C II
- Sony Alpha ILCE-7M IV
- Sony Alpha ILCE-7M3
- Sony Alpha ILCE-7M4
- Sony Alpha ILCE-7R III
- Sony Alpha ILCE-7R III A
- Sony Alpha ILCE-7R V
- Sony Alpha ILCE-7RM6B
- Sony Alpha ILCE-7S III
- Sony Alpha ILCE-6100
- Sony Alpha ILCE-6100A
- Sony Alpha ILCE-6400
- Sony Alpha ILCE-6400A
- Sony Alpha ILCE-6700
- Sony Alpha ZV-E10
- Sony Cyber-shot DSC-RX1R III

## Using RAW 9 in RawBrowse

Open a RAW photograph in Zoom View and select **RAW 9** to develop it. The adjustment panel appears when Apple's RAW 9 or RAW 9 DNG decoder supports the selected file. See the [feature guide](/rawcullbrowse/guide/) for the browsing workflow.

Changes update the developed preview and are saved automatically in a `.rawcull-raw9.json` sidecar beside the RAW file. The original RAW file remains unchanged. Export uses the same adjustments.

## Reset an individual control

**Double-click a slider to reset that adjustment to its default value.** You can also use the slider's **Reset to default** accessibility action. Camera and decoder defaults can vary between photographs; resetting restores the selected photograph's default rather than a fixed value shared by every image.

## White balance

| Control | Range | Default | What it does |
|---|---|---|---|
| **Temp K** | 2,000–50,000 kelvin | Camera white balance temperature | Sets the white balance temperature. Higher values generally warm the rendering; lower values cool it. |
| **Tint** | −150 to +150 | Camera white balance tint | Adjusts the green–magenta balance. Lower values shift toward green; higher values toward magenta. |

**White balance eyedropper:** click the eyedropper, then click a neutral white or gray area in the photograph. RawBrowse samples that area and adjusts temperature and tint together. Click **Cancel picker** to leave picking mode. The picker has no numeric range.

Resetting Temp K or Tint restores only that component of white balance.

## Light

| Control | Range | Default | What it does |
|---|---|---|---|
| **Exposure** | −5 to +5 stops | 0 stops | Adjusts exposure compensation. Negative values darken the image; positive values brighten it. |
| **Contrast** | Displayed amount: 0–2 | 1 | Adjusts overall image contrast. Values below 1 reduce contrast; values above 1 increase it. The slider gives small changes near the neutral value more travel for fine adjustment. |
| **Shadows** | 0–2 | Photograph's decoder shadow amount | Controls shadow boosting. Lower values darken shadows; higher values lighten them. This control is disabled when Tone curve is 0. |
| **Tone curve** | 0–1 | Photograph's decoder tone curve amount | Controls the amount of RAW tone mapping. 0 is linear; 1 applies the full decoder tone curve. This is an amount, rather than an adjustment centered on zero. |

**Contrast** in this group affects the overall rendered image. **Detail contrast**, described below, is a separate RAW decoder adjustment.

## Color

| Control | Range | Default | What it does |
|---|---|---|---|
| **Saturation** | 0–2 | 1 | Controls overall color intensity. 0 removes color, 1 preserves the developed image's saturation, and higher values increase it. |
| **Vibrance** | −1 to +1 | 0 | Applies a vibrance adjustment to color intensity. Negative values reduce vibrance; positive values increase it. |

## Detail

| Control | Range | Default | What it does |
|---|---|---|---|
| **Noise** | Offset: −D to 1 − D | 0 offset | Adjusts luminance noise reduction relative to the photograph's decoder default. Negative offsets reduce noise reduction; positive offsets increase it. |
| **Sharpness** | Amount: 0–1 | Photograph's decoder sharpness amount | Controls RAW sharpening. 0 disables sharpening; 1 is the maximum amount. |
| **Detail contrast** | Offset: −D to 1 − D | 0 offset | Adjusts the RAW decoder's contrast amount relative to its default. Negative offsets reduce the amount; positive offsets increase it. |

For **Noise** and **Detail contrast**, **D** is that control's decoder default for the selected photograph. The displayed value is an offset; the effective decoder amount is **D + offset**, limited to 0–1. For example, if the default noise amount is 0.3, its slider range is −0.3 to +0.7. Double-clicking returns the offset to 0, restoring an effective amount of 0.3.

**Sharpness** displays the effective amount directly. Double-clicking restores the decoder's original amount, which may be greater than zero.

## Crop

Click **Crop** to open a separate crop editor. Drag the crop rectangle to position it.

| Control | Range or choices | Default / reset behavior |
|---|---|---|
| **Constraint** | Free, 1:1, 4:3, 3:2, 16:9, 3:4, 2:3, 9:16 | A new crop starts with Free. A saved crop retains its constraint. |
| **Width** | 1%–100% of image width, further limited by a fixed aspect ratio when necessary | A new unconstrained crop starts at 100%. Double-click restores the maximum width that fits the selected constraint. |
| **Height** | 1%–100% of image height; shown only with Free constraint | A new crop starts at 100%. Double-click restores full height and moves its top edge to the top of the image. |

With a fixed aspect ratio, changing Width adjusts Height automatically. Moving or resizing the rectangle keeps it inside the image.

**Apply Crop** saves the draft crop. **Cancel** closes the editor without applying draft changes. **Remove Crop** removes the saved crop and restores the full image area.

## Panel actions

These controls perform actions and have no numeric range.

| Action | What it does |
|---|---|
| **Reset adjustments** | Resets all development adjustments to their camera or decoder defaults and removes the crop. |
| **Copy adjustments** | Copies the current RAW 9 adjustments for reuse on another photograph. In Zoom View, use **⌘C** for supported RAW 9 files. |
| **Paste adjustments** | Applies previously copied adjustments to the selected photograph. Available after copying; use **⌘V** in Zoom View. |
| **Export** | Saves the developed image with the current adjustments and crop. The menu lists formats supported by the Mac, plus HEIF (10-bit) and OpenEXR options. PNG and TIFF use 16-bit output. Exports run in request order in the background and continue after Zoom View closes. |
| **Collapse / Show RAW 9 controls** | Hides or reveals the adjustment panel's controls. |

The progress indicator shows when RAW 9 changes are being applied. If saving the adjustment sidecar fails, a warning icon provides the error details.
