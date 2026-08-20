+++
author = "Thomas Evensen"
title = "Security & Privacy"
date = "2026-07-15"
weight = 50
tags = ["security", "privacy"]
categories = ["user doc"]
+++

RawCull is designed for local photo culling. Core culling works offline, and
RawCull 3 performs AI indexing, search, and review on the Mac.

## File Access

RawCull runs in the macOS App Sandbox. It can read a catalog or write to a destination only after you choose that folder. macOS security-scoped bookmarks allow previously approved folders to be used again.

Copying is non-destructive: RawCull copies the chosen RAW files with the system rsync tool and does not delete files from the source catalog.

## Local Data

RawCull stores settings, approved folder locations, ratings, sharpness results, burst choices, and rebuildable preview caches on your Mac. RawCull 3 also stores embeddings, masks, and AI review results locally. Caches can be cleared from Settings.

RawCull does not use analytics, telemetry, cloud inference, cloud sync,
advertising, or tracking. Photographs, search descriptions, embeddings, masks,
and inference results are not sent to an external AI service.

Optional AI model downloads use macOS Managed Background Assets and therefore
require a network connection. macOS stores and manages those model resources;
after installation, RawCull runs them locally. Model downloading does not
upload photographs.

RawCull's privacy manifest declares no tracking and lists only the required
system API access reasons.

RawCull does not request access to the Photos library, camera, microphone, location, contacts, calendars, Full Disk Access, iCloud, Bluetooth, screen recording, or accessibility services.
