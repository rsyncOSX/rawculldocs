+++
author = "Thomas Evensen"
title = "Version 3.2.4"
date = "2026-09-20"
tags = ["changelog", "version 3.2.4"]
categories = ["changelog"]
+++

# RawCull Changelog: v3.2.1 → v3.2.4

Version 3.2.4 is submitted for update on Apple App Store. 

<div class="alert alert-secondary" role="alert">

RawCull 3.2.4 adds a dedicated AI Analysis workspace for selected or rated photographs. It combines the existing SAM 3 and DataComp CLIP review workflow with a new local Qwen3-VL vision-language assessment tool. All inference remains on the Mac, and the production model catalog now supports Apple-hosted Managed Background Assets for DataComp CLIP, SAM 3, and Qwen3-VL-2B-Instruct.

</div>

RawCull 3.2.4 requires macOS 27 (Golden Gate), an Apple Silicon Mac, and Xcode 27 when building from source.

**Commit range:** `v3.2.1..3b39d82`  
**Latest commit:** `3b39d82` (`AI`, 2026-09-20)  
**Current version:** RawCull 3.2.4 (Build 380)

## 🚀 Major Features

- Added a dedicated **AI Analysis** workspace for photographs selected in Grid View or rated two stars and higher.
- Added a choice between **SAM 3 + CLIP** Deep Review and local **Qwen** photo assessment.
- Added Qwen3-VL-2B-Instruct to RawCull's verified model catalog and Managed Background Assets workflow.
- Added Apple-hosted model delivery for DataComp CLIP, SAM 3, and Qwen3-VL-2B-Instruct.
- Added local, criteria-driven vision-language assessment with structured scores, strengths, problems, confidence, and subject details.
- Improved SAM 3 prompt selection by using cached CLIP artifacts to infer coarse subject labels.

## ✨ AI Analysis Workspace

- Added an **AI Analysis** toolbar action that becomes available when a catalog has selected or rated images to review.
- Added separate input choices for the current Grid selection and photographs rated two stars or higher.
- Preserved the selected photographs when moving between Grid View and AI Analysis.
- Added a horizontal thumbnail strip so the complete analysis set remains visible while reviewing results.
- Added empty-state guidance when the chosen input source has no photographs.
- Moved Deep Review out of burst-group headers and into the focused AI Analysis workflow.
- Allowed SAM 3 + CLIP analysis to operate on an arbitrary selected or rated set, not only an existing burst group.
- Applied the SAM 3 + CLIP recommendation back to RawCull's current selection and returned directly to Grid View when the review closes.

## 🧠 Qwen Photo Assessment

- Added local Qwen3-VL vision-language inference through PhotoAIKit's Core AI Qwen backend.
- Added editable assessment criteria, with a default review covering composition, exposure, subject visibility, expression, and obstructions.
- Added sequential batch analysis with per-image progress, cancellation, partial-failure handling, and protection against stale results.
- Added structured results for subject, composition, exposure, visibility, eye state, strengths, problems, overall score, and confidence.
- Added a sortable results table and a detail panel for each analyzed photograph.
- Preserved useful free-form model responses when Qwen does not return the structured schema.
- Added clear unavailable, validating, missing, invalid, and ready states for the active Qwen model.
- Rejected text-only Qwen models because RawCull requires a compatible vision-language bundle.
- Kept Qwen assessments advisory so photographers can verify the model's conclusions before making culling decisions.

## 📦 AI Models and Downloads

- Added the verified Qwen3-VL-2B-Instruct Core AI model pack to the model-download catalog.
- Added Qwen provenance, Apache 2.0 licensing, Apple Core AI conversion notices, archive size, installed size, and SHA-256 verification.
- Added Qwen to the Managed Background Assets manifest beside DataComp CLIP and SAM 3.
- Added automatic use of the downloaded managed Qwen model with an optional custom Core AI model-folder override.
- Added security-scoped bookmark persistence for custom Qwen model folders.
- Added model revalidation, source switching, clearing, and recovery when a saved custom model can no longer be accessed.
- Updated DataComp CLIP and SAM 3 provenance records for the new model release.
- Updated the production download service to use Apple's hosted asset-pack manifest.
- Added App Store Background Assets metadata, shared app-group configuration, and packaging support for all three model packs.
- Updated model release documentation with the v4 asset-pack workflow and verified archive information.

## 🎯 SAM 3 and CLIP Review

- Added CLIP-based coarse subject classification for person, bird, deer, animal, car, and landscape prompts.
- Reused compatible cached CLIP embeddings when selecting a SAM 3 subject prompt, without decoding source images again.
- Retained saliency-derived subject labels as a fallback when CLIP evidence is unavailable.
- Added support for Deep Review contexts created from selected photographs that do not belong to a saved burst group.
- Improved subject-mask outlines so disconnected subjects each receive a visible contour.
- Kept Deep Review cancellation, recommendation, focus-mask, and subject-detail behavior within the focused controller.

## 🖼️ Grid, Selection, and Sorting

- Kept multi-image Grid selections intact when moving between RawCull workspaces.
- Added a focused-image fallback when no multi-selection exists.
- Preserved catalog order when passing selected photographs into AI Analysis.
- Updated sharpness sorting so enabling it selects the sharpest visible photograph and resets the grid position consistently.
- Preserved the current selection when sharpness sorting is disabled.
- Simplified Grid and Similarity views after moving Deep Review presentation into AI Analysis.

## 🧪 Testing and Quality

- Added tests for AI Analysis input selection, catalog ordering, focused-image fallback, and selection persistence across view changes.
- Added Qwen tests for compatible model validation, text-only model rejection, structured response decoding, free-form fallback, invalid scores, and empty responses.
- Added model-catalog tests for the Qwen asset pack, provenance revision, sizes, checksums, and release readiness.
- Added tests for managed Qwen installation, custom-model selection, model-source switching, and saved-folder recovery.
- Added tests for CLIP-assisted Deep Review subject labels and arbitrary selected-image review contexts.
- Added coverage for disconnected SAM 3 mask contours and sharpness-sort selection behavior.
- Expanded release-metadata verification for version 3.2.4, Build 376, Apple-hosted Background Assets, app-group settings, and the three production asset packs.
- Updated the smoke-test manifest for the new release.

## 🛠️ Build and Documentation

- Advanced RawCull from version 3.2.1 Build 360 to version 3.2.4 Build 376.
- Added the CoreAIQwenBackend product and updated PhotoAIKit to revision `c5c76590c3d79ad508d24d893cd7d8d6aa873355`.
- Added the App Store-specific Background Assets information property list and export configuration.
- Updated archive and verification tasks for Apple-hosted model assets.
- Updated the README with Qwen, AI Analysis, local-inference, model-setup, architecture, and TestFlight information.
- Documented the DataComp CLIP, SAM 3, and Qwen3-VL roles and how their evidence differs.

## 📦 Version Progression

- Started from RawCull 3.2.1, Build 360.
- Refined Grid View selection and sharpness-sorting behavior.
- Added the Qwen3-VL inference layer, model validation, settings, and batch-analysis presentation.
- Added the combined AI Analysis workspace for SAM 3 + CLIP and Qwen.
- Added the Qwen model pack and migrated production model delivery to Apple-hosted Managed Background Assets.
- Completed release metadata and packaging for RawCull 3.2.4, Build 376.
- Updated this changelog through commit `a6e1d1c` on 2026-09-18.
