+++
author = "Thomas Evensen"
title = "Version 1.8.1"
date = "2026-05-16"
tags = ["changelog","version 1.8.1"]
categories = ["changelog"]
+++

# RawCull Changelog — v1.8.0 → 1.8.1

A stability and resource-hygiene release. Focus is on eliminating main-thread hangs in the zoom preview, tightening security-scoped resource lifecycles, and fixing
actor-isolation gaps in the rsync copy path.

---

## 🔒 Security-Scoped Resource Lifecycle

- **Catalog access is now scoped to the *active* catalog**, not every catalog ever opened in the sidebar. Long sessions no longer accumulate stale security-scoped accesses.
- Added explicit `startSecurityScopedAccess(for:)`, `stopSecurityScopedAccess(for:)`, and `stopActiveSecurityScopedAccess()` on `RawCullViewModel`.
- Catalog cancellation and app shutdown both route through the same cleanup path; `deinit` is now a last-resort fallback rather than the primary stop point.
- Picker flow simplified — view no longer manually pairs start/stop calls.

## ⚡ Zoom Preview — No More Main-Thread Hangs

- `ZoomPreviewHandler` sidecar JPG decode moved **off the MainActor** into a `Task.detached(priority: .userInitiated)`.
- Opening the zoom overlay on a large Sony JPEG no longer blocks the UI while ImageIO opens and decodes the file.
- Cancellation is now checked between every async step, so dismissing the overlay early stops in-flight work cleanly.
- `loadCGImage(from:)` and the disk-cache accessor are marked `nonisolated` to make the off-main path explicit.

## 🛠️ Rsync Copy Path (`ExecuteCopyFiles`)

- Security-scoped URLs for source and destination are now paired with a single, idempotent `cleanup()` call covering: rsync success, launch failure, user close/cancel, sheet dismissal, and `deinit`.
- Progress streaming callback hops to `@MainActor` before touching `progressContinuation`, removing a latent actor-isolation race that strict concurrency would flag.
- Progress continuations and streaming handler state are finished/cleared exactly once.

## 🧵 Catalog Loading & Cancellation

- `startCatalogLoad(for:)` short-circuits cleanly when the active catalog already has live security-scoped access — no redundant cancel/restart cycles.
- `cancelCatalogLoad()` now also clears `currentselectedSource` and stops active security-scoped access, preventing a new load from racing previous cleanup.

## 🧪 Tests

- New `RawCullViewModelSecurityScopeTests` covering start/stop pairing, re-selection no-op behavior, and cancellation cleanup.
- `ThumbnailProvider*` tests updated for the refreshed memory and cancellation surface.

## 🧹 Cleanup

- Removed legacy `documents/ver176.md`; added `documents/ver181.md` capturing the v1.8.0 review findings actually shipped in this release.
- Minor follow-ups in `RawCullViewModel+Catalog`, `SharpnessScoringModel`, and project settings.
