+++
title = "Combined Review"
weight = 30
lastmod = "2026-10-25"
categories = ["user doc"]
+++

Combined Review brings image, subject, regional, and model evidence into one local review workspace. It complements the SAM 3 + CLIP, Qwen Vision, and Objects tabs.

1. Enable **Settings > AI > Enable Combined Review** and prepare the required local models.
2. Select **1–8** finalists, open **Combined Review**, and check the input set. Split larger selections into smaller runs.
3. Enter **Review criteria**, choose **Depth**, and choose **High-quality preview** or **RAW detail**. Unavailable source options show a reason. Full-resolution RAW can exceed this workflow's memory limit even when it opens elsewhere in RawCull.
4. Choose **Review**. Use **Cancel** to stop work; **Resume** is available only when the saved run remains compatible.
5. Inspect the photo presentation, reviewed regions, evidence, limitations, and task history before making a culling decision. You can choose a region for a rerun or inspect comparison evidence when available.

High-quality previews contain camera processing. RAW detail requires macOS decoding support. If RAW rendering falls back to a preview, the result records that limitation; a reduced preview limits fine-detail evidence. The output is SDR sRGB, so it does not establish RAW highlight recoverability.

**Retain exact inputs** saves model input images and crops for later inspection. **Retain bounded rejected responses** is separate and off by default; it stores diagnostic text outside accepted evidence. **Clear retained inputs** and **Clear rejected responses** remove those retained artifacts. Clearing all cache data also removes saved Combined Reviews and their history; see [Cache](/docs/cache/).

A model observation, measured detail, segmentation confidence, and a supported conclusion carry different meanings. Read the evidence and any abstention or limitation alongside the original photograph. Final ratings remain your decision.
