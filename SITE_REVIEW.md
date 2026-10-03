# RawCull Website Review — October 3, 2026

The site has useful, concrete documentation, especially the screenshot tours and short feature guides. Its main weakness was the route into that material: the homepage explained the implementation rather than the purpose, the sidebar mixed workflow and maintenance topics, and the release archive exposed long development logs before explaining what changed for a photographer.

## Changes Made

- Rewrote the homepage around reviewing, comparing, and copying photographs, with direct links to the quick start and screenshot tour.
- Labelled the main navigation Documentation and Release Notes; moved About after the product and documentation links.
- Ordered the documentation around the culling workflow, followed by settings, cache, memory, and privacy. Put AI Step by Step before the model reference.
- Added a task-to-guide table and common missing-feature explanations to the documentation landing page.
- Clarified catalog-wide CLIP indexing versus detailed SAM 3 and Qwen review of selected candidates. Replaced an arbitrary hierarchy of AI evidence with questions each tool helps answer.
- Corrected the copy quick start, current AF-point scoring terminology, version-dependent AI settings controls, and the context-dependent P shortcut.
- Fixed two obsolete AI links, the dated release-post link, and the footer's missing Site info destination. Preserved existing published page paths and the screenshot redirect.
- Added concise summaries and explicit summary breaks to all 22 version posts. Removed duplicate page headings and oversized separators while keeping the historical changelog details.
- Added a recent-release guide and explained that submission notes and source snapshots do not confirm current App Store availability.
- Reduced duplicated RawCullBrowse introduction and status text, and added links explaining which app to choose.
- Distinguished app privacy from the website's configured Google Analytics and Google Custom Search services.
- Replaced example-site descriptions, package identity, the unrelated preview URL, and the inherited Google contribution instructions. Kept dependency versions and licence declarations unchanged.

## Editorial Assessment

The task guides should remain concise. More text would make the normal workflow harder to find. Screenshots are the right place for detailed descriptions of panels and results; the model reference is the right place for indexing and licence details.

The release history is still technical because it records implementation and validation work. Short summaries now provide an entry point for ordinary users without discarding that record. Future posts should lead with the effect on browsing or review, then put build and test details later.

The screenshot split is useful. Keeping Screenshots and AI Screenshots as separate sections makes the optional deeper review easier to distinguish from everyday culling.

## Validation and Limits

Source checks covered all 45 content pages: TOML and YAML front matter, 16 image references, 90 local Markdown links, four Hugo release references, linked heading anchors, paired content shortcodes, package JSON consistency, and documentation weights. Git whitespace checks passed.

Hugo compilation and rendered desktop/mobile inspection are left to the maintainer, as requested. The published homepage was checked, but the browser fetch could not retrieve the documentation and release index pages. This review therefore validates repository content and organization; it does not claim a complete visual review of the deployed site or new verification of application behavior.

Version-specific behavior was aligned with the repository's existing release notes and reviewed screenshots. Current App Store approval and download status were not independently verified or advanced.
