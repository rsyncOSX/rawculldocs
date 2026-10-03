+++
author = "Thomas Evensen"
title = "Security & Privacy"
date = "2026-07-15"
weight = 100
tags = ["security", "privacy"]
categories = ["user doc"]
+++

RawCull is designed for local photo culling. Core culling works offline, and AI indexing, search, and review run on the Mac.

## File Access

RawCull runs in the macOS App Sandbox. It can read a catalog or write to a destination only after you choose that folder. macOS security-scoped bookmarks allow previously approved folders to be used again.

Copying is non-destructive: RawCull copies the chosen RAW files with the system rsync tool and does not delete files from the source catalog.

## Local Data

RawCull stores settings, approved folder locations, ratings, sharpness results, burst choices, and rebuildable preview caches on your Mac. Embeddings, masks, and AI review results are also stored locally. Caches can be cleared from Settings.

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

## This Documentation Website

The app's privacy behavior is separate from this website. The site is hosted on Netlify and is configured to use Google Analytics and Google Custom Search. Visiting the site or using its search involves online services; it does not give the website access to your photo catalogs. See [Google's privacy policy](https://policies.google.com/privacy) for information about those Google services.
