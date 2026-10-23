# Changelog

## Unreleased

## 0.7.0 (2026-10-23)

- Follow the refreshed installation and language guides, with full-size screenshots of workspace structure, calls and function flow.
- Find documentation, screenshots and issue reporting in the public Codebase repository.
- Use the Freeware License for personal, educational and commercial use; modification and redistribution of the extension are restricted. Third-party licenses are unchanged.

## 0.6.0 (2026-10-23)

- Use the new Marketplace artwork and a matching circular code symbol in the diagram header, diagram tabs and editor toolbar.
- Open the current file's diagrams with the **Codebase: Open Diagrams** editor toolbar action.
- Rename the extension listing to **Aliot Codebase**.

## 0.5.0 (2026-10-22)

### Languages and program flow

- Explore Python, Rust, Kotlin, Java, C, C++, C#, Go, Dart, Ruby and PHP alongside TypeScript and JavaScript, using installed language extensions for structure and calls.
- Inspect statement flow for Python, Java, Kotlin, C, C++, Rust, C# and Go when the required syntax tools are installed. Dart and Ruby statement flow works without an additional SDK.
- See assignments, conditions, loops, alternatives and early exits, including in functions with no calls. When statement flow is unavailable, diagrams explain the limitation and show reported call sites where available.
- Check language health for structure, calls, statement flow and imports, with setup guidance for missing tools.
- Open a declaration's diagram from its inline **Show diagram** link. Functions open their flow; classes and types open their structure. Disable these links with `codebase.codeLens.enabled`.

### Workspace exploration and review

- Explore discovered callers, callees and directed call paths, with source navigation and explicit search limits.
- Inspect TypeScript and JavaScript module dependencies, import cycles and configured forbidden import boundaries in the **Modules** view.
- Review current and unsaved source against a local Git commit, or compare a diagram with a captured baseline. Removed items remain available for read-only review, with comparison limits explained.
- Save named workspace views, pin symbols and move through diagram history while preserving layout, search and selection.
- Export the complete included graph as JSON, or the visible diagram or current viewport as SVG and PNG.
- Use a consistent directed Tree layout, with expandable hierarchy for large workspaces.

### Usability and reliability

- Refresh workspaces more efficiently by reusing unchanged analysis, with visible progress, elapsed time and reuse information.
- Open source in the requested editor and keep diagram context during navigation, including unsaved edits.
- Preserve the completed diagram when another navigation action interrupts an inline request.
- Follow the illustrated user guide for exploration, architecture review, source changes, saved views, exports, commands and settings.

## 0.1.0 (2026-10-17)

First public release, consolidating the unpublished development builds.

- Explore complete TypeScript and JavaScript workspaces with structure, call graph, and per-function program-flow diagrams.
- Expand or collapse folders, files, and classes; search the complete model and export diagrams with source locations.
- Open source at the exact line with a single click while preserving diagram context.
- Choose directed Tree or evenly spread Network layouts, with background layout and lightweight large-workspace overviews.
- Follow unsaved edits and use compact light/dark diagram styling shared with Aliot's tools.
- Keep large-workspace diagrams complete and responsive, including when language tools stall or diagrams are repeatedly closed and reopened.
