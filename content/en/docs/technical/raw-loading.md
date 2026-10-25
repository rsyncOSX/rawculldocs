+++
title = "RAW Loading — Technical Detail"
linkTitle = "RAW loading"
weight = 5
lastmod = "2026-10-25"
categories = ["tech doc"]
+++

This page describes the RawCull 3.3.5 source reviewed on 25 October 2026. The application resolves RawParserKit 1.3.2. The [user guide](/docs/raw-support/) lists recognized extensions and compatibility implications.

## Discovery and format dispatch

`DiscoverFiles` accepts the case-insensitive extension union from `RawFormatRegistry.allExtensions`. The registry resolves `SonyRawFormat`, `NikonRawFormat`, `DNGRawFormat`, then `GenericRawFormat`. Registration admits files into a catalog; it does not probe whether every file can be decoded.

## Embedded previews

The application adapter calls `RawImageLoader` for metadata, thumbnails, and previews. Preview loading first considers a matching `.jpg` sidecar, then the format extraction path. ARW has direct JPEG-location parsing; DNG has dedicated binary JPEG extraction; NEF has dedicated ImageIO extraction with binary fallback. These paths can recover a camera-authored JPEG independently of sensor demosaicing. ImageIO still decodes JPEG pixels and supplies other metadata and image operations.

`GenericRawFormat` delegates to generic ImageIO thumbnail and embedded-preview extractors without a vendor binary fallback. Preview availability is therefore constrained by the image sources and subimages exposed by the installed framework. Focus location falls back to standard EXIF SubjectArea when no dedicated parser supplies it.

## RAW development and inspection

The viewer development helper calls a shared Core Image rendering path through `SonyRawFormat.createFullSizeJPEG`. Despite that API's historical name, the implementation uses a system RAW filter without an ARW-extension gate. The installed decoder determines whether a file can develop. `RAW9Support` probes supported decoder versions and prefers version 9, then version 9 DNG. The RAW 9 label is per-file capability information rather than a new file format.

EV analysis uses a separate serialized `CIRAWFilter` render, applies the requested exposure, shadow boost, supported highlight recovery, and highlight compression, then converts to sRGB RGBA8. Adjusted inspection images do not enter the normal export/JPEG cache. Clipping warnings and the sampled histogram describe preview pixels, not sensor clipping.

Combined Review records source fidelity, orientation, render settings, and limitations. Its default source admission limits are 24 million pixels and an estimated 2 billion working bytes, with an estimate of 80 bytes per pixel. Oversized embedded previews can be reduced; full RAW admission can be denied. These are workflow guards, not guarantees about decoder peak memory. See [Combined Review](/docs/ai/combined-review/).

## Source map

- `RawCull/Actors/DiscoverFiles.swift`: catalog discovery.
- `RawCull/Model/RawImageLoading.swift`: adapter, metadata, and RAW 9 probe.
- `RawCull/Model/Handlers/ZoomPreviewHandler.swift`: viewer development and EV rendering.
- `RawCull/Intelligence/CombinedReview/ReviewImageSource.swift`: source provenance and memory admission.
- `RawParserKit/Sources/RawParserKit/RawFormatRegistry.swift`, `RawImageLoader.swift`, and the individual format conformers: format dispatch and preview loading.
