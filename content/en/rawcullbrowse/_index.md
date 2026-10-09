---
title: RawBrowse
linkTitle: RawBrowse
type: docs
weight: 30
menu: { main: { weight: 30 } }
description: Browse local photo folders, develop supported RAW files, and optionally search and review photographs with two local AI models.
---

<div class="alert alert-info" role="alert">

**Release status:** RawBrowse **1.0.0** is available on [GitHub](https://github.com/rsyncOSX/RawBrowse/releases).

</div>

RawBrowse is built upon code from **RawCull**. It is a native macOS photo browser for exploring folders, inspecting photographs and camera information, and adjusting RAW files supported by **Apple's RAW 9 decoder on macOS 27 (Golden Gate)**.

Two optional AI models—**DataComp CLIP** and **Meta SAM 3**—add semantic search, visual similarity, and subject review. The models run locally on your Mac; ordinary browsing does not require them.

RawBrowse requires **macOS 27 or later** and an **Apple Silicon Mac**.

## Security

RawBrowse uses **macOS security-scoped access** for folders you select through the folder picker. This grants access to the selected folder and its contents, so you control which photo folders the app can use.

**Original RAW files remain unchanged.** RawBrowse reads the RAW data without writing changes back to the file. In RAW 9 mode, it creates and updates a separate `.rawcull-raw9.json` sidecar beside the original. This small file stores your development settings, allowing you to return to your adjustments while preserving the original sensor data.

The distributed DMG is **digitally signed and notarized by Apple**. Signing identifies the developer and lets macOS detect changes made after signing. Notarization means Apple's automated service has checked the submitted software for known malicious content and signing issues. These checks help macOS Gatekeeper verify the download before you open it. See [Apple's explanation of signing and notarization](https://developer.apple.com/help/account/certificates/create-developer-id-certificates).

[Read the feature guide and view the screenshots](/rawcullbrowse/guide/).

Use [RawCull](/docs/) when you want a catalog workflow for ratings, burst decisions, and copying selected RAW files. RawBrowse focuses on folder browsing and RAW adjustments.
