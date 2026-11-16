+++
author = "Thomas Evensen"
title = "Sharpness Scoring"
date = "2026-07-15"
weight = 20
tags = ["sharpness"]
categories = ["user doc"]
lastmod = "2026-10-25"
+++

Sharpness scoring measures detail to help you decide which photos to inspect first.

## Score Photos

1. Open **Grid** view.
2. Choose **Score AF Sharpness**.
3. Enable the sharpness sort to show stronger candidates first. **AF-point sharpness** sorts by measured detail around the camera autofocus location.

When you sort by AF-point sharpness, photos with a measurement appear first. General sharpness and Deep Review each have their own scores.

RawCull scores the selected photos, the photos shown by the current star filter, or the whole catalog. It adjusts the focus threshold for those photos and saves their scores and detected subject labels.

Choose **Re-score** after changing scoring parameters. Canceling a run discards that run's results.

## Scoring Parameters

For normal culling, use **Fast** quality with **Embedded Preview**. Use **Balanced** or **High Precision** when fine detail matters, and **RAW Demosaic** only for slower final checks when the installed macOS decoder supports the file.

The parameter sheet also controls thumbnail size, border exclusion, subject classification, and how strongly the detected subject affects the score. Larger images and developing RAW data take longer.

## Good Practice

- Compare scores only within the current catalog and scoring setup.
- Inspect important candidates at high zoom.
- Use [Focus Mask](/docs/focuspeaking/) to see where RawCull detects detail.
- In a burst, combine sharpness with expression, pose, framing, and timing.

Files without supported autofocus metadata can still be reviewed visually and analyzed from available pixels, but cannot supply an AF-point measurement. Preview support and RAW development are separate; see [Supported RAW Files](/docs/raw-support/).
