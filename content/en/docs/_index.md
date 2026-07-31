---
title: RawCull Documentation
linkTitle: Documentation
menu: { main: { weight: 20 } }
---

## RawCull

RawCull is a native macOS app for reviewing RAW photos before editing. It scans a folder, shows fast previews and camera information, records your picks and ratings, compares similar frames, and copies the selected files to another folder.

RawCull does not edit or delete the source photos.

## Quick Start

1. Select **Add Catalog** and choose a folder of RAW files.
2. Review the photos in **Loupe** or **Grid** view.
3. Press `P` to keep, `X` to reject, or `2`–`5` to rate a photo.
4. Use **Sharpness** and **Similarity** when you need help comparing many frames.
5. Select **Copy** to copy photos rated 2 stars or higher to your editing folder.

Ratings, sharpness results, and catalog state are saved automatically on your Mac.

## Main Views

| View | Purpose |
|---|---|
| Loupe | Browse a list and inspect one photo at a time |
| Grid | Rate, filter, and select many thumbnails |
| Similarity | Analyze bursts and review suggested frames |
| Rated | Show photos with saved culling data |
| Compare | Inspect up to four selected photos closely |

## Requirements and Files

- macOS Tahoe 26 or later
- Apple Silicon Mac
- Sony ARW and Nikon NEF catalogs

Sony ARW is the primary format. Some functions depend on the camera metadata and RAW support available in macOS. Demosaiced RAW preview and export are Sony-specific; embedded previews are used when RAW development is unavailable.

## Install

Install RawCull from the [Apple App Store](https://apps.apple.com/no/app/rawcull/id6759362764?mt=12), [GitHub Releases](https://github.com/rsyncOSX/RawCull/releases), or Homebrew:

```bash
brew tap rsyncOSX/cask && brew install --cask rawcull
```

GitHub releases are signed and notarized by Apple and may be newer than the App Store version. RawCull is sandboxed, works offline, and keeps photos and culling data on your Mac. See [Security & Privacy](/docs/security/).
