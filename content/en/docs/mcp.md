+++
author = "Thomas Evensen"
title = "Model Context Protocol (MCP)"
description = "Connect Codex to RawCull’s local MCP helper, authorize catalog access, and run burst analysis."
date = "2026-11-13"
lastmod = "2026-11-16"
weight = 45
tags = ["mcp", "bursts"]
categories = ["user doc"]
+++

RawCull bundles a local Model Context Protocol (MCP) helper. A client such as
Codex can use it to read details from catalogs you approve, request burst
analysis, follow its progress, and show recommended frames. The setup described here
was validated with RawCull 3.4.6 (TestFlight build 452) and helper 0.7.0.

[Watch the RawCull MCP video on YouTube](https://youtu.be/ahWjBtW8iN0).

## How MCP works in RawCull

MCP gives Codex a standard way to find available tools, send them
requests, and read their results. Codex is the client, and `rawcull-mcp` is the
server. The helper and the RawCull app run as separate processes.

1. Codex starts the helper and communicates over standard input and output
   (`stdio`). There is no local web address or port to configure.
2. Codex asks the helper for its tools and the arguments each accepts.
3. When you ask for the helper version, Codex calls `rawcull_status` with no
   arguments. The helper receives the tool name and any arguments. Codex keeps
   the conversation.
4. The helper returns the version, connection type, and available tools. Codex
   can report the result or use it to choose the next tool.

The helper runs on your Mac. If Codex uses a cloud model, data returned by the
tools, such as filenames and capture metadata, may be sent to that model.

## Connect Codex to RawCull

Register the signed helper bundled with RawCull. Adjust the path if you installed
RawCull elsewhere. If `rawcull` is already registered to a different executable,
run `codex mcp remove rawcull` first.

```bash
helper=/Applications/RawCull.app/Contents/MacOS/rawcull-mcp
codesign --verify --strict --verbose=2 "$helper" &&
  codex mcp add rawcull -- "$helper"
```

Start a fresh Codex session and check the connection:

> Use the `rawcull` MCP server. Call `rawcull_status` and report the helper
> version, transport, and available tools.

## Control catalog access

After the catalog finishes loading, open **Settings → MCP** and choose
**Authorize Selected Catalog** to share saved details about the catalog and its photos. Before
authorizing, enable **Allow MCP to start burst analysis** if you want Codex to
start jobs. For a folder whose photos change between runs, also enable **Allow
analysis of future photos in this folder**.

Codex can also request analysis from a prompt. If you haven’t saved permission for
that run, RawCull opens and asks you to select or confirm a folder.
**Approve This Job** allows one run. **Approve and Remember** saves permission
to run analysis again. RawCull keeps one saved catalog authorization and runs
one MCP analysis job at a time.

The helper can read saved metadata and the last shared result after RawCull
quits. **Clear Last Shared Result** removes the result and keeps the saved
catalog access. **Revoke Access** removes the saved access and the current job.

## Run a burst analysis

Replace the example path with the full path to your photo folder:

> Use the `rawcull` MCP server to run a new burst analysis for
> `/Users/me/Pictures/Bursts`. Wait for me to approve in RawCull if prompted.
> Once the job completes, report how many photos were analyzed and show the
> recommended frame, alternate, and confidence for each of the first five bursts.

Codex uses `request_catalog_setup` to request the run and
`get_catalog_setup_status` to check whether it has been approved and queued.
Once it has the catalog and job IDs, it uses `get_analysis_status` to track the
run and `list_bursts` to retrieve the rankings. For a new run, use a fresh
request ID. To retry the same request, reuse its ID and arguments.

| Other tool | What it does |
| --- | --- |
| `list_catalogs` | Lists saved catalog access and whether it allows analysis. |
| `catalog_summary` | Reports the photo count, capture date range, and available camera breakdown for a catalog. |
| `list_photos` | Lists photo IDs, filenames, capture dates, and camera models in pages. |
| `start_catalog_analysis` | Starts analysis for a catalog that already has permission. |
| `cancel_analysis` | Requests cancellation of an active job. |

For help reviewing results in RawCull, see
[Similarity, Bursts, and Search](/docs/similarity/).
