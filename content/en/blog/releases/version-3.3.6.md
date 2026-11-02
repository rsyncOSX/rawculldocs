+++
author = "Thomas Evensen"
title = "Version 3.3.6"
date = "2026-11-02"
tags = ["changelog", "version 3.3.6"]
categories = ["changelog"]
+++

**Changes:** v3.3.5 → v3.3.6 development

This changelog continues from the [3.3.5 release notes]({{< relref "version-3.3.5.md" >}}) and covers the eight commits after the `v3.3.5` release tag through the source snapshot below.

**Commit range:** `v3.3.5..c78f891e`\
**Baseline tag:** `v3.3.5` at `8613323b` (2026-10-25), RawCull 3.3.5 Build 426\
**Latest source commit:** `c78f891e` (2026-11-01)\
**Current source version:** RawCull 3.3.6 (Build 428)\
**Release status:** In development; not yet released

## Faster Fit-to-window RAW Previews

- Develop RAW previews to match the Zoom View viewport in backing pixels when a completed preview or compatible saved developed JPEG is unavailable. Preview sizes round up to 256-pixel steps to avoid unnecessary work during small window resizes.
- Render completed sRGB display pixels in the background, avoiding deferred RAW rendering on the drawing thread.
- Request native resolution when zooming above fit or opening at actual pixels. Keep original image dimensions for the dimensions label, actual-pixel framing, and focus positioning, even when displaying a smaller preview.
- Keep the current photograph visible while a higher-resolution preview loads. Coalesce resize requests and prevent cancelled or superseded loads from replacing the selected photograph.
- Display a ready preview without waiting for the separate analysis thumbnail.
- Preserve the existing full-size developed-JPEG path and persistent disk cache for normal native zoom, and reuse compatible saved developed JPEGs in fit view across app launches. The first native zoom can still require full RAW development when that cache is empty.

## Preview Cache and Exposure Inspection

- Add a **256 MiB**, byte-bounded memory cache for completed RAW previews, evicting least-recently-used entries. Cache identity includes the source file revision, requested size, and inspection exposure; oversized previews are displayed without being retained.
- Keep fit, native, and exposure-inspection entries distinct, and replace older inspection exposures at the same size.
- Release the active RAW rendering graph when Zoom View closes while retaining completed previews for revisits. Memory-pressure events and **Delete Cache** also clear the preview cache and rendering resources.
- Keep exposure and clipping inspection at native resolution. Reuse the source filter between exposure adjustments, and reset it when entering or leaving inspection so inspection settings do not carry into normal viewing.
- Keep adjusted inspection previews separate from the persistent JPEG/export cache. Loupe and burst previews retain their existing full-size loading policy.

## Zoom View and Metadata

- Open Zoom View with the **embedded JPEG** source by default, including the standard open action.
- Label the Loupe histogram **Histogram · Displayed Preview** and show the preview source and its pixel dimensions beneath it.
- Add an **Open Zoom View** button and double-click guidance beside the histogram.
- Label file dimensions **Original Dimensions**, move Camera Settings before File Attributes, and simplify Quick Actions to **Show in Finder**.
- Stop substituting a generated thumbnail for a selected Loupe source that has not loaded, keeping the histogram tied to the displayed preview.

## Recorded RAW Preview Measurements

The standalone benchmark used six landscape Sony ARW files, each developing to **9984 × 6656 pixels**, with three passes per performance mode. The fit-preview target was **3072 pixels** on the longest edge.

| Pipeline | Median image delivery | Process peak memory (RSS) |
| --- | ---: | ---: |
| Previous full-size JPEG path, cache miss | 4382 ms | 919 MiB |
| Native-resolution policy, cache miss | 4372 ms | 919 MiB |
| Direct fit preview, 3072 pixels | 1418 ms | 391 MiB |

Uncached fit-preview delivery was approximately **3.1× faster**, with approximately **57% lower process peak memory** in this workload. Native-resolution performance remained effectively unchanged. Completed fit-preview cache lookup and image delivery had a median of **0.040 ms**; this does not measure drawing a frame.

Geometry and pixel-comparison checks passed on all six photographs, including changes between fit sizes and native resolution. The maximum fit-preview mean absolute RGB error against the native JPEG reference was **0.779/255**, below the benchmark's **3/255** bound. Sampled exposure-inspection comparisons at −1, 0, and +1 EV matched the previous inspection algorithm.

These are standalone pipeline measurements, not application-wide memory measurements or first-painted-frame timings. UI hitches, other cameras, and actual portrait RAW files were not measured; portrait sizing has unit coverage. The measurements do not establish the same gains for every catalog or camera.

## Dependencies, Tooling, and Verification

- Update RawParserKit from **1.3.2** to **1.3.3** and advance app metadata to **3.3.6 Build 428**.
- Add a reproducible RAW preview benchmark with geometry, pixel-comparison, cache-revisit, and exposure-inspection checks, retaining measurements and source hashes in the repository.
- Add regression coverage for preview sizing, cache limits and invalidation, resize coalescing, cancellation, stale completions, saved-JPEG reuse, inspection cache separation, and display publication before analysis completes.
- Refresh the README and scripts guide with RAW preview behavior, catalog benchmark commands, diagnostics, and script checks. Refine benchmark helpers and progress-test synchronization.

The recorded RAW preview validation passed **32 focused tests**, including **10 new preview tests**. Its full run executed **696 tests** and reported **three failures**: two README release-metadata mismatches and a Zoom View default-source expectation. The README version and dependency entries were subsequently updated in the source range covered here, but these notes do not claim a passing full-suite rerun.

These notes were checked against the stated Git range and the recorded RAW preview investigation. No new app benchmark or test run was performed for this documentation update. Version 3.3.6 remains in development and has not been released.
