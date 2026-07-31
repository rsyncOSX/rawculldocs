+++
author = "Thomas Evensen"
title = "Sharpness Scoring"
date = "2026-07-15"
weight = 10
tags = ["sharpness"]
categories = ["user doc"]
+++

Sharpness scoring estimates image detail and sorts the strongest candidates first. It is a comparison aid, not an automatic reason to reject a photo.

## Score Photos

1. Open **Grid** view.
2. Choose **Score Sharpness**.
3. Leave the **Sharpness** sort enabled to show higher scores first.

RawCull scores the current multi-selection, the active star-rating filter, or the full catalog. It first calibrates the focus threshold to the selected photos, then saves the resulting scores and detected subject labels.

Choose **Re-score** after changing scoring parameters. Canceling a run discards that run's results.

## Scoring Parameters

For normal culling, use **Fast** quality with **Embedded Preview**. Use **Balanced** or **High Precision** when small detail matters, and **RAW Demosaic** only for slower final checks on supported files.

The parameter sheet also controls thumbnail size, border exclusion, subject classification, and how strongly the detected subject affects the score. Larger images and RAW demosaicing take longer.

## Good Practice

- Compare scores only within the current catalog and scoring setup.
- Inspect important candidates at high zoom.
- Use [Focus Mask](/docs/focuspeaking/) to see where RawCull detects detail.
- In a burst, combine sharpness with expression, pose, framing, and timing.
