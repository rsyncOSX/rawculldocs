+++
title = "RawCull testing and validation"
linkTitle = "Testing"
weight = 90
lastmod = "2026-11-11"
description = "Python regression coverage, Swift test plans, and recorded catalog and model validation for RawCull."
categories = ["tech doc"]
tags = ["testing", "validation", "performance"]
+++

The 11 Python test modules in `Scripts` exercise RawCull's test runners, evidence formats, and diagnostic tools. Most use temporary fixtures and mocked Xcode invocations. The optional AF crop test processes generated PNGs through the scoring CLI. [Scripts and reproduction instructions](https://github.com/rsyncOSX/RawCull/blob/3a92139b/Scripts/README.md)

## Checks run against the build 442 source

On 11 November 2026, the Python suite discovered **44 tests** at source commit `3a92139b`. **43 passed and one skipped** because OpenCV was unavailable. `make verify-model-provenance` passed for DataComp CLIP, SAM 3, and Qwen. The separate `make verify-ai-import-boundary` check failed on two findings: a `PhotoAIContracts` import in `RawCullSemanticSearchModel.swift` outside the allowlist, and direct access to `similarityModel.isIndexing` from `BurstCullingWorkspaceView.swift`.

Run the Python suite from the RawCull repository root:

```sh
make test-scripts
make test-scripts PYTHON=/absolute/path/to/python3
```

The second form selects an environment with `cv2` and `numpy` for the crop integration. The Makefile supplies the case-sensitive `Test*.py` discovery pattern.

## Python regression coverage

| Test module | Tests | Behavior exercised |
|---|---:|---|
| `TestCompareAFSharpness.py` | 4 | Compares labeled within-burst pairs in either direction, handles ties and uncertain labels, reports missing scores, and rejects reversed duplicates or mismatched burst membership. The optional integration scores generated sharp and blurred AF crops through the exported-crop workflow. |
| `TestFreezeCombinedReviewBaseline.py` | 4 | Preserves export identity, decoded JSON, and file hashes while inventorying an existing export. Reports absent exports and validates PNG dimensions, format, and headers, including truncated or invalid inputs. |
| `TestModelProvenance.py` | 1 | Exercises rejection of malformed, missing, blocked, or inconsistent evidence for enabled model releases. The standalone verifier checks the checked-in production catalog. |
| `TestRunAIAnalysis.py` | 3 | Selects filename-sorted ARWs nonrecursively, with no default count limit. Covers runner exit and evidence outcomes, and ignores malformed or incomplete progress diagnostics. |
| `TestRunCombinedReviewSix.py` | 4 | Covers catalog selection and frozen-fixture behavior, progress in execution order, terminal reports, rejection of an empty catalog before Xcode, and exit and evidence outcomes. |
| `TestRunnerPrivacy.py` | 1 | Checks private output permissions and restoration of the caller's umask on successful and failed runs. |
| `TestRunCatalogPerformance.py` | 9 | Checks stable nonrecursive catalog manifests and change detection, rejects empty catalogs and zero-byte inputs, and requires complete results and phases. Retains failures and tail percentiles, gates on median changes, rejects incompatible baselines, preserves unavailable resource measurements, and validates reports without invoking Xcode in validate-only mode. |
| `TestExportCatalogTrace.py` | 5 | Selects wait and I/O tables, validates XML while streaming, distinguishes missing, empty, and unverified signposts, resolves profiler references, detects conflicting weights, and rejects empty CPU exports or output overwrites. |
| `TestValidateCatalogSignposts.py` | 4 | Pairs overlapping signpost IDs using process and name, counts successful and partial cancellation paths, resolves XML schema references, and rejects truncated, unmatched, or duplicate events. |
| `TestAnalyzeGridPreparation.py` | 4 | Assigns measurements to completed preload passes, retains failed-grid counts, reports out-of-window and crossing spans as unassigned, and rejects ambiguous overlap and incomplete or duplicate windows. |
| `TestAnalyzeSourceLoadDiagnostics.py` | 5 | Separates parser waits from reporting costs, matches repeated files to containing operations, distinguishes cancellations from failures and outliers, rejects bad timestamps or duplicate IDs, and preserves unknown reporting costs when outer measurements are absent. |

## Swift and real-input validation

| Entry point | What it exercises |
|---|---|
| `make test-full` | Main RawCull test plan with Thread Sanitizer. Suites cover ratings, burst state, previews, metadata, caching, file access, concurrency, and AI workflow contracts. |
| `make test-smoke` | Enumerates the checked-in smoke selectors before running them. The Makefile expects 401 tests. |
| `make test-performance` | Verifies and runs two selectors for concurrent cache stress and Vision indexing and ranking. |
| `make releaseaiobjectstest`, `make combinedinputstest` | Opt-in Xcode runs for real Objects analysis and model input contracts. They require the relevant compiled model assets and configured test inputs. |
| `make releasesharpnesstest`, `make releaseafcomparison` | Opt-in real-catalog scoring and AF comparison export. `CompareAFSharpness.py` can compare exported crops against explicit within-burst labels. |
| `make combinedreviewtest`, `make combinedselectiontest` | Opt-in real-model Combined Review probes for a configured image or catalog selection. |
| `RunAIAnalysis.py`, `RunCombinedReviewSix.py` | Drive the corresponding Xcode tests using installed models and selected ARWs. The AI Analysis runner sequences SAM 3 + CLIP, Qwen Vision, and Objects. Combined Review can use the original hash-checked six-file fixture. |
| `RunCatalogPerformance.py`, `RunRAW9PreviewBenchmark.py` | Measure catalog preload and RAW preview delivery on actual ARWs. The catalog runner records phases and can compare compatible baselines. The preview runner checks geometry and exposure as well as delivery time and memory use. |
| `RunPhotoAnalysisCatalog.py`, `EvaluatePeopleRanking.py` | Compare PhotoAnalysisKit revisions on a matched catalog and evaluate proposed People order against supplied preferences. Agreement measures require labeled examples. |

These entry points describe available coverage. The Python results above come from the tooling suite. Full Swift, real-model, and real-catalog runs require their own results. [Makefile](https://github.com/rsyncOSX/RawCull/blob/3a92139b/Makefile) · [Release test inputs and commands](https://github.com/rsyncOSX/RawCull/tree/3a92139b/RawCullReleaseTests)

## Recorded development results

- **People close-ups.** Human face close-ups were checked in TestFlight. Controlled blur reduced measured sharpness in all 96 measurable face crops across 85 RAWs. The offline evaluator passed synthetic winner-change and fallback checks. Two real group pairs provided examples of retained order, with insufficient evidence to measure keeper agreement against RawCull's Best Frame order.
- **Catalog preload.** The historical 809-ARW run recorded 8.206 s and 8.415 s for cold thumbnail-cache preload, and 3.553 s and 3.647 s for warm preload. Harness fingerprints differed. Separate traces measured less sampled main-thread work after progress changes. End-to-end speedup remained unverified across matched runs.
- **PhotoAnalysisKit.** A single before-and-after run across the same 227 RAW files reduced standalone batch analysis including decode from 25.09 s to 17.48 s. Median combined analysis and mask rendering went from 194.12 ms to 99.38 ms. The benchmark used Automatic, Fast, and a 1024-pixel limit on the same Mac.
- **RAW 9 previews.** Six ARWs over three passes per mode passed geometry and exposure checks. The median uncached 3072-pixel fit preview took 1.418 s versus 4.382 s for the legacy native JPEG path, with peak RSS of 391 MiB versus 919 MiB. The focused suite passed 32 tests. The historical full run executed 696 tests and recorded three failures involving version metadata and a zoom source expectation.
- **Model delivery.** DataComp CLIP, SAM 3, and Qwen macOS asset packs were packaged and uploaded without reported errors or warnings in the earlier App Store migration. The maintainer reported availability in 3.2.4.

[Development summary and archived reports](https://github.com/rsyncOSX/RawCull/blob/3a92139b/Docs/summary.md)
