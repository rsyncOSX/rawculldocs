+++
title = "Qwen Vision Model — Technical Detail"
linkTitle = "Qwen Vision model"
weight = 70
categories = ["tech doc"]
tags = ["technical"]
description = "Technical documentation of RawCull analysis implementation and result computation."
+++

Qwen Vision performs vision-language assessment of selected photographs and detected objects. It generates a response from an image attachment and an instruction. Its scores are model-generated judgments, followed by explicit application validation and weighting.

## Loading and generation

`CoreAIQwenProvider` validates bundle metadata, a Qwen tokenizer identity, positive vocabulary and context lengths, and supported text or vision-language model kind. A vision bundle must contain vision configuration and the required embedding and vision assets. RawCull's model provenance identifies a Qwen3-VL-2B asset pack; the provider reads configuration from the actual selected bundle.

`QwenInferenceRuntime` lazily creates and reuses a `CoreAIVisionLanguageModel`. Each request creates a `LanguageModelSession`, attaches the image, supplies the instruction, and passes a maximum response-token budget through `GenerationOptions`. A generation gate serializes shared requests. Cancellation and model-generation checks prevent obsolete work from being accepted after runtime changes.

The ordinary photo request uses a 512-token response budget. This limits generated output length, not the image resolution or the entire model context. The call sets the token budget explicitly; these docs do not assume an application-fixed temperature or sampling configuration absent from that call.

## Photo assessment and numeric score

The default review criteria cover composition, exposure, subject visibility, expression, and obstructions. The structured assessment contains subject text, composition score, exposure score, subject-visibility score, optional eyes-open state, problems, strengths, and confidence.

The decoder extracts the first opening brace through the last closing brace and attempts JSON decoding. Validation requires each numeric assessment score to be in 1–5 and confidence in 0–1. RawCull computes the overall score from valid fields:

```text
overall = 0.50 × (compositionScore / 5)
          + 0.20 × (exposureScore / 5)
          + 0.30 × (subjectVisibilityScore / 5)
```

Thus the validated weighted score ranges from 0.2 to 1.0. Expression and eyes-open information can appear in the assessment but have no separate term in this formula. Generated confidence also does not multiply the overall score.

Nonempty responses that cannot be decoded as a valid structured assessment are retained as freeform text. They remain useful review output, but do not acquire an invented numeric score. Empty output fails. Per-file results keep structured assessment, freeform output, or failure information.

## Objects workflow

Automatic object discovery first asks Qwen for concepts with a 384-token budget; manual concept mode bypasses that request. SAM 3 segments each concept and the application deduplicates instances. It renders a review board with object identifiers, then sends that image and object-specific criteria to Qwen with a 1024-token budget.

`ObjectAnalysisResponseDecoder` checks the response against the board's allowed IDs. If structured decoding fails, the application retains the generated response as freeform output. If no instances survive segmentation, the workflow returns no object assessment rather than asking Qwen to assess an empty board.

This use of explicit instance IDs makes a generated observation traceable to a retained mask. It does not make the language model's judgment a physical focus measurement. Object confidence, segmentation confidence, and the photo weighted score remain separate outputs.

## What computes the result

The vision-language runtime encodes the attached image and generates response tokens conditioned on the instruction and image representation. RawCull delegates that learned inference to Core AI, then parses and validates output and applies the documented weighting formula. It does not compute composition or exposure ratings using a deterministic pixel formula.

Prompt wording, input image, model bundle, and generation behavior can influence assessments. Use Qwen explanations alongside the deterministic [sharpness measurement](/docs/technical/sharpness-scoring/) and [subject evidence](/docs/technical/subject-evidence/) when comparing finalists.

## Source map

- `PhotoAIKit/Sources/CoreAIQwenBackend/CoreAIQwenProvider.swift`: bundle validation and runtime construction.
- `RawCull/RawCull/Intelligence/Qwen/QwenInferenceRuntime.swift` and `QwenGenerationGate.swift`: request execution and token limits.
- `RawCull/RawCull/Intelligence/Qwen/QwenPhotoAssessment.swift`: schema, validation, freeform fallback, and score formula.
- `RawCull/RawCull/Intelligence/ObjectAnalysis/`: discovery, review-board rendering, and response decoding.
- `RawCull/ModelAssets/Notices/Qwen/PROVENANCE.json`: model asset provenance.
