+++
title = "Supported RAW Files"
linkTitle = "RAW Support"
weight = 5
lastmod = "2026-10-25"
tags = ["RAW", "formats", "ARW", "DNG", "ImageIO"]
categories = ["user doc"]
+++

From **RawCull 3.3.5**, you can add catalogs containing a wider range of RAW files. The extension list tells RawCull which files to discover; successful previewing and RAW development also depend on the file, camera model, and installed macOS support.

## Recognized formats

| Format or camera family | Extensions | Preview support |
|---|---|---|
| Sony | ARW | Dedicated parser and direct embedded-JPEG extraction |
| Adobe DNG | DNG | Dedicated parser and direct embedded-JPEG extraction |
| Nikon | NEF | Dedicated parser with embedded-JPEG fallback |
| Canon | CR2, CR3, CRW | macOS ImageIO |
| Fujifilm | RAF | macOS ImageIO |
| Olympus / OM System | ORF | macOS ImageIO |
| Panasonic | RW2 | macOS ImageIO |
| Pentax / Ricoh | PEF, PTX | macOS ImageIO |
| Samsung | SRW | macOS ImageIO |
| Sigma | X3F | macOS ImageIO |
| Phase One | IIQ | macOS ImageIO |
| Hasselblad | 3FR, FFF | macOS ImageIO |
| Kodak | DCR, KDC | macOS ImageIO |
| Minolta / Konica Minolta | MRW | macOS ImageIO |
| Epson | ERF | macOS ImageIO |
| Mamiya | MEF | macOS ImageIO |
| Leaf | MOS | macOS ImageIO |
| Nikon compact cameras | NRW | macOS ImageIO |
| Leica | RWL | macOS ImageIO |
| Casio | BAY | macOS ImageIO |
| Legacy Sony | SR2, SRF | macOS ImageIO |

Extensions are matched without regard to letter case. A recognized extension is not a guarantee that every camera model, compression mode, or file variant is supported. JPEG, PNG, and TIFF helpers exist internally, but catalog discovery here selects the registered RAW extensions.

## Direct ARW and DNG support versus ImageIO

**Direct support** means RawCull's RawParserKit library understands enough of the container to locate and extract an embedded JPEG without first developing the RAW sensor data. ARW and DNG have dedicated thumbnail and preview extractors with binary parsing paths. NEF also has a dedicated parser and binary fallback. ImageIO is still used to decode the extracted JPEG pixels and for other image operations.

For users, this can allow fast browsing of a file whose camera RAW data the installed macOS decoder cannot yet develop, provided the file contains an embedded JPEG the parser can read. It does not guarantee support for every ARW or DNG variant. A DNG without a usable embedded JPEG may still need macOS decoding support.

**ImageIO support** means the other formats use Apple's image framework to expose thumbnails and previews. RawCull has no dedicated vendor binary-preview fallback for those extensions. A new camera or unsupported compression mode may appear in the catalog but fail to produce a preview, show a smaller preview, or expose less metadata. Results may change with macOS updates.

## Browsing is different from RAW development

The **JPG** source normally shows a camera-authored embedded preview; a matching lowercase `.jpg` sidecar can take precedence when loading the preview. That image already contains the camera's color, sharpening, noise reduction, and exposure choices. Its resolution may be lower than the sensor image.

The **RAW** source develops sensor data through macOS Core Image. The control reads **RAW 9** when the selected file advertises a supported version 9 decoder; otherwise it reads **RAW** and uses the system decoder. Direct embedded-preview support does not install a new RAW decoder. A file can work in JPG mode while RAW development fails. Use the JPG source when development is unavailable.

Sharpness, focus masks, similarity, and AI review depend on successfully decoded pixels. Preview resolution and camera processing can affect detail measurements, and changing the source can change results. Autofocus markers also depend on readable camera metadata: format recognition alone does not guarantee a marker or an AF-point sharpness measurement.

Embedded-JPG export currently extracts bytes from ARW, NEF, and DNG only. Browsing another recognized format through ImageIO does not enable that export path. Developed export depends separately on the system RAW decoder.

## Check your camera files

Try a small catalog from each camera and compression mode you use. Check thumbnails, JPG zoom, metadata, and RAW mode separately before relying on a large review session. Keep macOS updated when possible. If a file fails, include the RawCull version, macOS version, camera model, extension, compression setting, and failing operation in your report.

For viewer and export instructions, see [Culling Photos](/docs/culling/). For the parser and decoder boundaries, see [RAW Loading — Technical Detail](/docs/technical/raw-loading/).
