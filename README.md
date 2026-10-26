# Aliot Codebase

Explore your codebase through interactive diagrams of structure, calls, program flow and module dependencies, right inside VS Code. Start with a workspace overview, follow a connection, and jump to the exact source without losing your place.

[![Workspace structure with symbol search, expandable folders and source properties](docs/images/01-workspace-structure.png)](docs/images/01-workspace-structure.png)

*Explore a workspace, find a declaration and jump to its source. Screenshots use a fictional Order platform project. Select an image to view it at full size.*

## Get started

1. In **VS Code 1.96 or newer**, open Extensions, search for **Aliot Codebase** by **voidaliot**, and install it. Open a folder or multi-root workspace.
2. Open the **Codebase** icon in the Activity Bar to browse the workspace, or run **Codebase: Open Workspace Diagrams** from the Command Palette.
3. Choose **Structure**, **Call graph**, **Program flow** or **Modules**.
4. Expand a folder and select a file or declaration in the sidebar to open its diagram. Selecting a function opens its inside flow directly.

For a single file, use the **Codebase: Open Diagrams** icon in the editor toolbar. You can also select **Show diagram** above a declaration to open its structure or function flow beside your code.

If the publisher supplies a VSIX, install it with `code --install-extension codebase-vsc-ext-0.9.1.vsix` or **Extensions: Install from VSIX…**.

Open **Language health** to check available capabilities and setup guidance. The [user guide](docs/user-guide.md) walks through each feature, command and setting.

## Explore from workspace to function

| View | Use it to |
| --- | --- |
| **Structure** | Understand how folders, files, classes and functions fit together. |
| **Call graph** | Follow discovered calls, explore callers and callees, and find directed call paths. |
| **Program flow** | Browse workspace functions and inspect the statement flow of a selected function. |
| **Modules** | Inspect TypeScript and JavaScript imports, cycles and configured dependency boundaries. |

The native **Workspace browser** under the Codebase icon keeps the whole included workspace visible while you open files, classes and functions. Its expanded branches stay in place, and selection follows diagram navigation. Select the workspace root to return to the overview; use a row's **Open Source** action for its source range. Diagrams follow unsaved edits and support light and dark themes.

The top toolbar contains view types, file selection, symbol search, refresh and export. Zoom, fit, hierarchy and layout controls sit together at the bottom of the canvas.

Workspace diagrams retain every included file and discovered symbol. Collapsing groups and filtering the view control what you see; exclusions and configured file limits remain explicit.

## Investigate a change

- **Explore impact:** follow direct or transitive callers and callees, or find a shortest discovered directed path between two symbols.
- **Review source changes:** compare current code, including unsaved edits, with a local Git commit.
- **Compare diagrams:** capture a baseline, make a change, then inspect added or removed declarations and relationships.

[![Call graph with caller and callee exploration and highlighted results](docs/images/02-call-impact.png)](docs/images/02-call-impact.png)

*Trace the discovered callers and callees of a selected function, with results linked back to source.*

Call graphs reflect the relationships reported by your language tools. Dynamic or unresolved calls may be missing, so an absent path does not prove that a change has no runtime impact. A captured baseline compares the diagram you saved; it does not reconstruct a historical call graph from Git.

## Look inside a function

Select a function in the **Workspace browser**, open **Program flow**, or use a declaration's **Show diagram** link to inspect branches, loops, returns and cleanup paths. Select a statement to return to its source.

[![Function flow showing a loop, conditional branches, returns and cleanup](docs/images/04-detailed-flow.png)](docs/images/04-detailed-flow.png)

*Follow branches, loop exits and cleanup paths inside a function.*

Flow diagrams describe static source paths. Unsupported or approximate behavior is explained in diagram notes. When detailed statement flow is unavailable, Codebase can show reported call sites in source order, with the limitation clearly marked.

## Save your place and share findings

- Save named views with their scope, layout, expansion, search and selection.
- Pin important symbols and use back/forward history to return to recent diagram contexts.
- Export the **complete included graph as JSON**, or the **visible diagram or current viewport as SVG or PNG**.

Saved views and pins stay local to the workspace. Share exported diagrams through your existing documentation and code review tools.

## Language support

Structure and call graphs use the language support available in VS Code. Capabilities are checked independently: a language extension may provide symbols without providing calls.

| Languages | Program flow |
| --- | --- |
| TypeScript and JavaScript, including JSX and TSX | Detailed statement flow built in |
| Dart and Ruby | Detailed statement flow built in; no additional SDK or runtime needed |
| Python, Java, Kotlin, C# and Go | Detailed statement flow with the required installed syntax tools |
| C, C++ and Rust | Detailed statement flow with supported language-extension syntax capabilities |
| PHP | Reported call-site flow; detailed statement flow is currently unavailable |

See the [language setup guide](docs/languages.md) for required extensions, optional toolchains and language-specific limits.

## Local analysis

Codebase's analysis and diagram layout make no external requests, upload no source and do not run the analyzed program. Source navigation preserves unsaved buffers.

Codebase supports desktop VS Code and remote workspaces, including SSH, WSL and containers. Configure optional syntax tools on the machine where the extension runs. Browser-only hosts such as vscode.dev are not supported.

Optional external syntax tools require a trusted workspace. Git review requires a trusted filesystem workspace and **Git 2.45 or newer**; it reads local objects without fetching, checking out files or changing the Git index.

## Help and feedback

- [User guide](docs/user-guide.md) — illustrated workflows, commands, settings and troubleshooting
- [Language setup](docs/languages.md) — extensions, optional tools and capability limits
- [Changelog](CHANGELOG.md) — features and fixes by version
- [Report an issue](https://github.com/voidaliot/codebase/issues) — bugs, feature requests and questions
- [Support](SUPPORT.md) — what to include when reporting a problem

## License

[Freeware License](LICENSE): free to use for personal, educational and commercial purposes. Modification and redistribution of the extension are restricted. Bundled third-party components retain their own licenses and notices.
