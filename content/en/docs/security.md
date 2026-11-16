+++
author = "Thomas Evensen"
title = "Security & Privacy"
date = "2026-07-15"
weight = 100
tags = ["security", "privacy"]
categories = ["user doc"]
lastmod = "2026-11-11"
+++

RawCull is designed for local photo culling. Core culling works offline, and AI indexing, search, and review run on the Mac.

## File Access

RawCull runs in the macOS App Sandbox. It can read a catalog or write to a destination only after you choose that folder. macOS lets RawCull reopen folders you’ve already approved.

When you copy selected RAW files, RawCull keeps the originals in the source folder.

## Local Data

RawCull stores settings, approved folder locations, ratings, sharpness results, burst choices, and rebuildable preview caches on your Mac. RawCull also saves its search index, subject outlines, and AI review results on your Mac. You can clear cached data in Settings.

RawCull does not use analytics, telemetry, cloud inference, cloud sync,
advertising, or tracking. Photographs, search descriptions, embeddings, masks,
and inference results are not sent to an external AI service.

Optional AI model downloads use macOS Managed Background Assets and therefore
require a network connection. App Store builds fetch Apple-hosted packs, and standalone Release builds fetch self-hosted packs. macOS stores and manages those model resources;
after installation, RawCull runs them locally. Model downloading does not
upload photographs.

RawCull's privacy manifest declares no tracking and lists only the required
system API access reasons.

RawCull does not request access to the Photos library, camera, microphone, location, contacts, calendars, Full Disk Access, iCloud, Bluetooth, screen recording, or accessibility services.

## This Documentation Website

The app's privacy behavior is separate from this website. The site is hosted on Netlify and is configured to use Google Analytics and Google Custom Search. Viewing or searching the site uses online services. Your photo catalogs stay on your Mac. See [Google's privacy policy](https://policies.google.com/privacy) for information about those Google services.

## Retained Review Inputs

Combined Review can retain exact model inputs and crops locally for inspection. Combined Review can also save a limited amount of model output that it rejected. That option is off by default. Its controls can clear retained inputs or rejected responses. **Settings > Cache > Delete Cache** also deletes saved Combined Reviews, masks, retained inputs, and review history. **Delete Saved Files** separately deletes saved catalog records, ratings, and analysis results; see [Cache](/docs/cache/).
