+++
author = "Thomas Evensen"
title = "Version 3.3.4 — In Development"
date = "2026-10-21"
tags = ["changelog", "version 3.3.4"]
categories = ["changelog"]
summary = "In development, not yet released: a combined Single Images review group, simpler cache and saved-data management, and cleaner image rating overlays."
+++

**In development — version 3.3.4 has not yet been released.** These notes describe the current source changes and may change before release.

This update brings single photographs together for review, simplifies cache and saved-data management, and reduces clutter over photographs.

**Changes:** v3.3.3 → v3.3.4 development

This changelog continues from the [3.3.3 release notes]({{< relref "version-3.3.3.md" >}}). It covers the 10 commits after the `v3.3.3` release tag through the source snapshot below, rather than the earlier development snapshot described in that post.

**Commit range:** `v3.3.3..69c0be8f`  
**Baseline tag:** `v3.3.3` at `e7516156` (2026-10-16), RawCull 3.3.3 Build 423  
**Latest source commit:** `69c0be8f` (2026-10-20)  
**Current source version:** RawCull 3.3.4 (Build 424)  
**Release status:** In development; not yet released

## Single Images and Burst Review

- Display photographs in the **Single Images** queue together in one review group, with an image count and a shared filmstrip.
- Open that group in the review workspace without assigning a burst winner or ranking to unrelated photographs.
- Apply **Reviewed** and **Deferred** workflow states to the constituent single-image groups, preserving those states in saved review data.
- Read review status from the stored workflow state even when a group has no analysis result.
- Double-click a thumbnail in the grouped grid to open its group for review.

## Cache and Saved-data Management

- Simplify **Settings > Cache** with a total cache size and a compact breakdown of preview, grid thumbnail, disk thumbnail, full-size JPG, similarity artifact, burst group, and Combined Review storage.
- Replace individual cache deletion controls with one **Delete Cache** action and a confirmation explaining what will be removed. This includes saved Combined Reviews, masks, retained input images, and review history. Original photographs, settings, and downloaded AI models are kept.
- Add a separate **Saved Files** section showing the size of saved catalog records, ratings, and analysis results, with a confirmed **Delete Saved Files** action. Original photographs are kept.
- Move the saved burst similarity evidence display from AI settings to Cache settings, retaining the CLIP/Vision breakdown and information about unreadable or outdated cache files.
- Remove the **Show Saved Files** command from the File menu.

## Image Review Presentation

- Replace the on-image rating action bars with a compact **Current rating** badge across review views. The badge uses an icon, label, and color to show unrated, rejected, keeper, and star ratings, with an accessible rating description.
- Move image pixel dimensions beside the current rating badge in the zoom overlay.
- Remove the extra zoom hint and rating action bar from the zoom overlay controls to leave more space for the photograph.

## Dependencies and Maintenance

- Update RawParserKit from **1.3.1** to **1.3.2**.
- Advance app and model downloader metadata to **3.3.4 Build 424** and update Xcode project and test scheme configuration.
- Add regression coverage for saving and restoring Single Images workflow states without choosing a winner.
- Apply source formatting and internal cleanup across image loading, catalog handling, and AI review components.

These notes were checked against the Git changes in the stated range. They do not represent a completed release validation or a new test run.
