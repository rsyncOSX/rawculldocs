+++
author = "Thomas Evensen"
title = "Focus Points"
date = "2026-07-15"
weight = 31
tags = ["focus points"]
categories = ["user doc"]
lastmod = "2026-10-25"
+++

Focus Point marks where the camera reported focus. RawCull can read this information from supported Sony ARW, Nikon NEF, and DNG files. Other RAW formats can provide the location if they store it in the standard EXIF SubjectArea field.

Open a photograph in the full-window viewer or Compare view, then press `A` or use the focus-point control. The marker appears only when RawCull can decode a valid focus location for that file.

To check whether the subject is sharp, use a high-resolution preview and [Focus Mask](/docs/focuspeaking/).

Support varies by camera model, firmware, and RAW format. If no marker appears, the file may not contain a supported focus location.

Converting a file to DNG can drop the camera’s original autofocus information. See [Supported RAW Files](/docs/raw-support/) for format details.
