+++
author = "Thomas Evensen"
title = "Security & Privacy"
date = "2026-07-15"
weight = 50
tags = ["security", "privacy"]
categories = ["user doc"]
+++

RawCull is designed for local, offline photo culling.

## File Access

RawCull runs in the macOS App Sandbox. It can read a catalog or write to a destination only after you choose that folder. macOS security-scoped bookmarks allow previously approved folders to be used again.

Copying is non-destructive: RawCull copies the chosen RAW files with the system rsync tool and does not delete files from the source catalog.

## Local Data

RawCull stores settings, approved folder locations, ratings, sharpness results, burst choices, and rebuildable preview caches on your Mac. Caches can be cleared from Settings.

The app has no network entitlement and does not use analytics, telemetry, cloud sync, advertising, or tracking. Its privacy manifest declares no tracking and lists only the required File API and Disk Space access reasons.

RawCull does not request access to the Photos library, camera, microphone, location, contacts, calendars, Full Disk Access, iCloud, Bluetooth, screen recording, or accessibility services.
