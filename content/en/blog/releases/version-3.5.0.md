+++
author = "Thomas Evensen"
title = "Version 3.5.0"
date = "2026-11-25"
tags = ["changelog", "version 3.5.0"]
categories = ["changelog"]
+++

Version 3.5.0 is submitted for update on the [Apple App Store](https://apps.apple.com/no/app/rawcull/id6759362764?mt=12).

RawCull 3.5.0 is a supplementary release to [3.4.9]({{< relref "version-3.4.9.md" >}}). It builds on the Review and burst AI assessment that 3.4.9 introduced, and adds no new models or package dependencies. It adds a **magnifier** and **focus overlays** to the Review photo, and shows where the AI and the measured best frames disagree in a burst. Compared with 3.4.9, it is also clearer which review type you are about to run, existing AI assessments are protected from an accidental rerun, and an AI run no longer cancels itself while loading its model.

## App Store changelog

The **What's New** text for App Store Connect:

```text
RawCull 3.5.0 is a supplementary release to 3.4.9. It refines Review and burst AI assessment.

Review
- New magnifier: choose 100% or 200% under the photo and hover to inspect focus at pixel level.
- Show the camera's focus point and the measured in-focus areas on the photo to compare them.
- Choose a review type first, then run it. Focus check is now the default on every Mac.

Bursts
- The burst workspace and AI Assessments window mark the sharpest frame, the Best Frame suggestion and the AI's top-scored frame.
- A "Best frame differs" bar shows when the AI and the measurements disagree. Click a pick to show that frame.
- Reassess asks before it replaces existing AI assessments.
- Back to burst returns from an AI assessment to the burst you came from.

Fixes
- A burst AI assessment no longer cancels itself while it loads the Qwen model.
```

## Inspect focus on the Review photo

- **Magnifier**: choose **Off**, **100%**, or **200%** under the photo, then hover to see a round lens at that pixel scale. It uses the full-size embedded JPG, and falls back to the smaller review preview until the JPG has loaded or when the two are framed differently. The magnifier is off each time Review opens. The lens is hidden while you choose a region for a rerun.
- **Focus point** shows the camera's recorded autofocus point. **Focus mask** shows the areas measured as in focus, so you can compare the two. Both are off when Review opens, and Focus point is disabled when the photo has no recorded autofocus point.

## Choose a review, then run it

- The presets are now a **Review type** choice with radio buttons, separate from the button that starts the review. The button names what it runs: **Run Focus Check**, **Run Describe**, or **Run Full Review**.
- **Focus check** is preselected on every Mac. In 3.4.9, Macs with enough memory started on Full.
- When Describe and Full are unavailable on a Mac, the reason is shown under the choice.

## See where the AI and the measurements disagree

- The burst workspace and the AI Assessments window show the same frame markers in their filmstrips: **Sharpest**, the **Best Frame** suggestion, and each assessed frame's AI score, which is the mean of its 1–5 rubric scores.
- The single highest-scored frame is marked **AI top**. No frame is marked when the top score is shared or fewer than two frames have a score.
- A **Best frame differs** bar appears when the frame with the top AI score is not the Best Frame suggestion or not the sharpest frame. It names each pick; click one to show that frame.

## AI assessments of bursts

- When results exist, **View Results** is the highlighted button on the burst list's AI assessment card, and **Assess All Bursts** becomes **Reassess**. Reassess asks for confirmation before it clears the current results.
- Opening **View AI Assessment** from the burst viewer now gives you **Back to burst** (**R**), which returns to the burst you came from. **Burst list** sits beside it.
- The main toolbar, **Re-index Catalog**, and **Index Similarity** are disabled while the burst AI assessment runs. The toolbar was already disabled during a review.
- Removed **Scoring Parameters** from the Burst Groups **More** menu. The toolbar button opens the same sheet.

## Fixes

- A burst AI assessment or an MCP follow-up photo assessment that had to load the Qwen model could be cancelled by its own model refresh. A run now survives the refresh it asked for. A refresh started elsewhere, for example from Settings, still stops it.

## Under the hood

- Added regression coverage for frame markers, the Best frame differs bar, and model refreshes during burst assessments and MCP follow-ups. Review tests now select the Full preset explicitly.
- `make test-full` first copies the app version and build number from the Xcode project into the README.
- The README release checklist now makes the App Store upload the release step, with a manual test of the TestFlight build, including an AI model download, before submission.
- Package dependencies are unchanged from 3.4.9.

These notes cover the changes from the `v3.4.9` tag through the `v3.5.0` tag at commit `c2e04a2a` (RawCull 3.5.0, build 470).
