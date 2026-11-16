+++
title = "Supported RAW Files"
linkTitle = "RAW Support"
weight = 5
lastmod = "2026-10-25"
tags = ["RAW", "formats", "ARW", "DNG", "ImageIO"]
categories = ["user doc"]
+++

RawCull 3.3.5 added support for more RAW file extensions. Use the table to check which files RawCull looks for when you add a folder. Whether a file opens depends on the camera, its RAW settings, and your version of macOS.

## Recognized formats

| Format or camera family | Extensions | Preview support |
|---|---|---|
| Sony | ARW | Reads the embedded JPG directly |
| Adobe DNG | DNG | Reads the embedded JPG directly |
| Nikon | NEF | Uses a Nikon reader with an embedded JPG fallback |
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

RawCull accepts uppercase and lowercase extensions. Test a few files from your camera to check which RAW modes work.

## How RawCull loads previews

For ARW, DNG, and NEF files, RawCull can show a usable JPG stored inside the file even when macOS cannot develop the RAW data. For the other formats, RawCull uses Apple’s ImageIO to load thumbnails, previews, and camera information. A new camera or RAW compression mode may give you a smaller preview, fewer camera details, or no preview at all.

## Browsing is different from RAW development

Choose **JPG** to see the camera’s preview stored inside the RAW file. If a matching lowercase `.jpg` file sits next to it, RawCull may show that file first. The preview can be smaller than the full RAW image.

Choose **RAW** to have macOS render the image from the camera’s RAW data. The control reads **RAW 9** when that Apple decoder supports the file. If RAW mode cannot open the file, try **JPG**.

The size and processing of the preview can affect sharpness, focus masks, similarity, and AI review. Autofocus markers and AF-point scores also require focus information that RawCull can read.

You can export embedded JPGs from ARW, NEF, and DNG. To export a developed JPG, macOS must support the file’s RAW data.

## Check your camera files

Try a small catalog from each camera and compression mode you use. Check thumbnails, JPG zoom, metadata, and RAW mode separately before relying on a large review session. Keep macOS updated when possible. If a file fails, include the RawCull version, macOS version, camera model, extension, compression setting, and failing operation in your report.

For viewer and export instructions, see [Culling Photos](/docs/culling/). For the parser and decoder boundaries, see [RAW Loading — Technical Detail](/docs/technical/raw-loading/).
