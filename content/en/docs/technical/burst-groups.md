+++
title = "Burst Groups — Technical Detail"
linkTitle = "Burst groups"
weight = 40
categories = ["tech doc"]
tags = ["technical"]
description = "Technical documentation of RawCull analysis implementation and result computation."
+++

Burst analysis has two independent parts: deciding which adjacent photographs belong together, then ranking candidates within each group. `RawCullCore` implements these as deterministic engines. Optional deeper AI review is a subsequent stage.

## Group boundaries

`BurstGroupingEngine` processes the supplied file order, compares each file with its predecessor, and starts a new group if any configured boundary condition fires. This is adjacent linkage: A may match B and B may match C even when A and C would differ. It is not all-pairs clustering.

| Boundary evidence | Default rule |
|---|---|
| Visual distance | Split at distance >= 0.25 |
| Missing visual distance | Always split |
| Capture time | Split when absolute gap > 2 seconds |
| Modification-date fallback | Use a 10-second maximum gap |
| Camera | Require the same normalized camera value |
| Focal length | Split when available focal lengths differ by > 3 mm |
| Shutter, aperture, ISO | Split when an individual change exceeds 0.5 EV |
| Exposure compensation | Split when change exceeds 0.34 EV |

The defaults belong to grouping algorithm version 4. Camera and focal-length requirements are configurable. Lens changes are recorded in boundary evidence but do not independently split a group in this engine; they do affect metadata stability during ranking.

Exposure changes are compared individually, rather than canceled into a net exposure difference. Shutter and ISO deltas use `abs(log2(new/old))`; aperture uses `2 × abs(log2(new/old))`; exposure compensation uses an absolute linear difference. If numeric conversion fails but both textual shutter, aperture, or ISO values exist and differ, an unquantified exposure change still creates a boundary.

The engine records the pair IDs, distance, absolute gap, fallback-time status, focal delta, maximum exposure adjustment, metadata-change flags, and boundary reasons. Missing focal data alone does not trigger the focal-length rule.

## Candidate score

Let `S = clamp(rawSharpness/catalogMaximum, 0, 1)`. Missing, non-finite, or invalidly normalized sharpness contributes zero. If at least two measured candidates have a normalized spread of at least 0.03, compute burst-relative sharpness `R = (S−groupMinimum)/(groupMaximum−groupMinimum)`. The ranking sharpness component is then `0.65S + 0.35R`; otherwise it is S.

```text
overall = 0.62 × rankingSharpness
          + 0.12 × focusPointComponent
          + 0.10 × saliencyComponent
          + 0.16 × metadataComponent
```

The focus component is 0.70 with an AF coordinate and 0.45 without one. It measures availability of AF evidence, not AF sharpness. The saliency component is 0.45 without a subject label, 0.60 if no dominant label is available, 0.75 for the dominant group label, and 0.25 for another label.

Metadata starts at 0.70 when exposure, camera, and lens are stable, or 0.40 otherwise. Tight similarity adds 0.15; an aperture <= f/5.6 adds 0.05. ISO above 800 subtracts 0.05 per stop, capped at 0.15. Lower estimated motion risk adds 0.05; elevated risk subtracts 0.05 per stop, capped at 0.15. The final component is clamped to 0–1.

Motion risk uses shutter time multiplied by focal length when focal data exists: a ratio <= 0.5 is lower risk and > 1 is elevated risk. Without focal data, shutter times <= 1/500 second are lower risk and >= 1/60 second are elevated risk. These are metadata heuristics, not observed subject-motion estimates.

Candidates sort by descending overall score; ties retain input group order. The first and second become the recommendation and runner-up.

## Confidence and review

Tight similarity means every internal boundary distance is below 0.22. Metadata stability excludes any internal exposure, camera, or lens change. Reliable capture time requires every file to avoid modification-date fallback.

High confidence requires at least three group members, a best-versus-second absolute overall gap >= 0.12, best normalized sharpness >= 0.65, stable metadata, tight similarity, and reliable capture times. A gap >= 0.05 with stable metadata gives medium confidence. Missing scores or other cases give low confidence.

The result's `isSafeForOneClickCulling` flag is true only for high confidence. The engine returns scores, reasons, cautions, recommendation IDs, and review state; it does not itself mutate file ratings. Expression, moment, and framing are not directly measured by this ordinary ranking formula. Use [subject evidence](/docs/technical/subject-evidence/) or [Qwen Vision](/docs/technical/qwen-model/) to understand the separate deeper-review stages.

## Source map

- `RawCullCore/Sources/RawCullCore/BurstGroupingEngine.swift`: boundary calculation.
- `RawCullCore/Sources/RawCullCore/BurstAnalysisModels.swift`: defaults, version, and evidence types.
- `RawCullCore/Sources/RawCullCore/BurstRankingEngine.swift`: ranking and confidence.
- `RawCull/RawCull/Intelligence/BurstAnalysis/`: orchestration and cache compatibility.
