+++
author = "Thomas Evensen"
title = "Sharpness Scoring"
date = "2026-07-15"
weight = 20
tags = ["sharpness"]
categories = ["user doc"]
+++

Sharpness scoring estimates image detail and sorts stronger candidates first. It is a comparison aid, not an automatic reason to reject a photograph.

## Score Photos

1. Open **Grid** view.
2. Choose **Score AF Sharpness** in the 3.2.8 workflow. Earlier versions label the action **Score Sharpness**.
3. Enable the sharpness sort to show stronger candidates first. In the 3.2.8 workflow, **AF-point sharpness** sorts by measured detail around the camera autofocus location.

For AF-point sorting, files without a usable recorded autofocus location or measurement appear after measured files. The general sharpness and Deep Review scores remain separate comparison aids.

RawCull scores the current multi-selection, the active star-rating filter, or the full catalog. It calibrates the focus threshold for those photographs, then saves the scores and detected subject labels.

Choose **Re-score** after changing scoring parameters. Canceling a run discards that run's results.

## Scoring Parameters

For normal culling, use **Fast** quality with **Embedded Preview**. Use **Balanced** or **High Precision** when fine detail matters, and **RAW Demosaic** only for slower final checks on supported Sony files.

The parameter sheet also controls thumbnail size, border exclusion, subject classification, and how strongly the detected subject affects the score. Larger images and RAW demosaicing take longer.

## Good Practice

- Compare scores only within the current catalog and scoring setup.
- Inspect important candidates at high zoom.
- Use [Focus Mask](/docs/focuspeaking/) to see where RawCull detects detail.
- In a burst, combine sharpness with expression, pose, framing, and timing.

See [Grid View by AF-Point Sharpness](/docs/screenshots/screenshots/#grid-view-by-af-point-sharpness) for an example of the ordering.
