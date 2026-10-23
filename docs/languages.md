# Language support

All languages share one extension and renderer. Structure and resolved calls use
the language tools installed in VS Code. Detailed Program flow additionally uses
an official syntax API when available. Codebase reads the current buffer,
including unsaved changes; it does not run the analyzed program, import its
modules or invoke its build scripts.

| Language | Editor IDs and closed-file suffixes | Structure and calls | Detailed statement syntax |
| --- | --- | --- | --- |
| TypeScript / JavaScript | `typescript`, `typescriptreact`, `javascript`, `javascriptreact`; `.ts`, `.tsx`, `.mts`, `.cts`, `.js`, `.jsx`, `.mjs`, `.cjs` | Built-in TypeScript and JavaScript Language Features | Official TypeScript compiler shipped with VS Code, with a bundled fallback |
| Python | `python`; `.py`, `.pyi`, `.pyw` | Pylance or basedpyright | CPython **3.10+**, standard-library `ast.parse` |
| Java | `java`; `.java` | Red Hat Java (`redhat.java`) | Full **JDK 21+**, `JavacTask.parse` |
| Kotlin | `kotlin`; `.kt`, `.kts` | Kotlin by JetBrains (`JetBrains.kotlin-server`) | Standalone Kotlin SDK, **tested with 2.2.21**, and JDK 21, official PSI API |
| C | `c`; `.c`, `.h` | clangd or Microsoft C/C++ | Active clangd with advertised `astProvider` |
| C++ | `cpp`; `.cpp`, `.cc`, `.cxx`, `.hpp`, `.hh`, `.hxx`, `.ixx`, `.cppm`, `.inl` | clangd or Microsoft C/C++ | Active clangd with `astProvider` |
| Rust | `rust`; `.rs` | rust-analyzer (`rust-lang.rust-analyzer`) | rust-analyzer's recognized `viewSyntaxTree` JSON schema |
| C# | `csharp`; `.cs`, `.csx` | C# (`ms-dotnettools.csharp`) | **.NET SDK 8+**, official Roslyn syntax assemblies |
| Go | `go`; `.go` | Go (`golang.go`) with gopls | **Go SDK 1.23+**, standard-library `go/parser` and `go/ast` |
| Dart | `dart`; `.dart` | Dart (`Dart-Code.dart-code`), with symbols and calls observed independently | Bundled official analyzer **14.6.0**, compiled to JavaScript; no end-user Dart SDK |
| PHP | `php`; `.php`, `.phtml` | PHP Tools (`DEVSENSE.phptools-vscode`) or another installed public provider; capabilities observed independently | Unavailable; disclosed provider call-site flow when available |
| Ruby | `ruby`; `.rb`, `.rake`, `.gemspec` | Ruby LSP (`Shopify.ruby-lsp`) or Solargraph (`castwide.solargraph`) for available symbols; call-hierarchy capability is separate | Bundled official Prism **1.9.0** WebAssembly parser; no end-user Ruby runtime |
<!-- language-rows -->

Syntax tools and providers have different jobs. Python, Java, Kotlin, C# and Go
helpers can parse statements independently of a provider. Structure, resolved
calls and provider-backed declaration hints still need the VS Code language
extension. Finish its project setup and indexing before refreshing calls.
The editor must also recognize the file's language mode. Codebase does not
register languages or grammars; an installed language extension may be needed
to register a mode such as Kotlin. If a file opens as Plain Text, install its
language support and select the correct language mode before requesting flow,
even when its syntax SDK is already installed.
Codebase reuses an active suggested provider or activates one installed candidate,
rather than starting competing alternatives together.
Dart and Ruby also parse statements independently through bundled official
parsers. Their syntax availability does not establish that an installed provider
can supply document symbols or calls. Ruby LSP or Solargraph being installed
must not be read as a call-hierarchy promise. Closed-file discovery uses the
suffixes above; special names such as `Gemfile` and `Rakefile` are not additional
closed-file patterns. An open buffer already recognized as Ruby still follows
its editor language mode.

## What Program flow represents

Detailed flow shows **static paths within the selected function**, rather than a
recording of a program run. The shared builder understands statement order,
condition branches and joins, loops, selection alternatives, abrupt completion,
handlers and cleanup. Frontends supply the constructs their syntax contains.

| Syntax family | Language-specific handling |
| --- | --- |
| Branches and loops | Source conditions, while/do/for/iteration loops, initializers and updates; literal truth facts come from syntax |
| Selection | Java/C/C++ switch fallthrough; nonfallthrough rule, match and when arms; Go's explicit fallthrough |
| Abrupt completion | Returns, explicit throws, break/continue and named targets; terminal paths do not connect to following statements |
| Cleanup | try/handler/finally completions; Python loop/try else, with, Java resources, C# using and Go defer have language-specific handling and notices |
| Nested declarations | Nested functions, methods and supported lambdas have separate targets; declaring them does not run their bodies |
| Unsupported or implicit behavior | Visible unsupported nodes and notices disclose summarized effects such as implicit exceptions, expression-level short circuit, scheduling, generators, dynamic dispatch, macro expansion and resource exception suppression |

This is not whole-program execution, complete data flow or proof of reachability.
Read the diagram's notices. Syntax errors, unknown tree schemas and size/depth
limits must not silently become apparently complete detailed diagrams.

Dart functions, methods, constructors and closures have separate targets;
top-level/static initialization is not treated as a sequential entry flow.
Async/generator scheduling, implicit exceptions and expression effects retain
notices. Assertions, unsupported constructor/pattern forms and transfers to
switch-case labels remain opaque. Ruby methods and arrow lambdas are separate
targets, with syntax-directed branches, loops and rescue/else/ensure. Ordinary
call blocks with nonlocal transfers, safe navigation, pattern matching,
redo/retry/yield and dynamic eval/catch/throw behavior remain opaque.
Ruby `raise`/`fail` are overridable calls, so syntax does not certify them as
unconditional throws. Non-UTF-8 Ruby magic encodings are rejected instead of
guessing coordinates.

When syntax is missing, unavailable, malformed or exceeds a documented bound,
Codebase explains the reason and falls back to **call-site flow** when the
provider supplies it. That view lists only reported calls in source order, with
separate caller and callee links. It does not invent branches or returns; source
order is not execution order. Empty results do not prove that a function performs
no work. VS Code can mask server failures as empty results, so call completeness
remains dependent on the installed provider.

## Local syntax tool setup

TypeScript/JavaScript, Dart and Ruby use bundled official syntax. Dart and Ruby
need no user SDK or runtime setting and can parse source in untrusted workspaces.
They run extension-owned parser code in a bounded local worker; source remains
data, and its imports, packages, initializers and programs are not executed or
resolved. A development Dart SDK is required to build the extension's parser,
separately from installing or using Codebase. PHP has no detailed statement
syntax backend or SDK setting; its available provider calls can supply the
explicitly labeled call-site fallback.

External helpers require a **trusted workspace**. Configure machine-scoped tool
settings on the extension host. For SSH, WSL or containers, use remote paths and
install the tools there. Choose trusted installations; Codebase does not select
project wrappers, workspace compiler plugins or build scripts as syntax backends.

| Setting | Value |
| --- | --- |
| `codebase.analysis.pythonPath` | CPython executable, 3.10+ |
| `codebase.analysis.javaPath` | Java executable from a full JDK 21+ |
| `codebase.analysis.kotlinHome` | Standalone Kotlin SDK home containing `lib/kotlin-compiler.jar`, tested with 2.2.21 |
| `codebase.analysis.dotnetPath` | dotnet executable with a .NET SDK 8+ |
| `codebase.analysis.goPath` | Go executable from an SDK 1.23+ |
| `codebase.analysis.statementFlow` | Resource-scoped boolean, default `true`; disable to inspect provider call-site flow explicitly |

An empty tool setting uses installed-tool discovery on the host. Codebase does
not download SDKs or language servers. Helper code and build inputs belong to
the extension; source travels as JSON over local stdin. Python uses isolated
mode with site initialization disabled. Java parses with annotation processing
disabled and never invokes analyze/generate. C# uses the SDK's Roslyn assemblies;
Go uses its standard library. Project restore or compilation is unnecessary.
Incompatible Kotlin PSI versions produce setup/fallback guidance.

At most two helper children run, with sixteen queued requests, finite 30-second
analysis waiting/running deadlines, 8 MiB input/output and 64 KiB stderr budgets.
First preparation of an extension-owned compiled helper can take up to sixty
seconds; later requests reuse its source/tool-keyed artifact in extension storage.
Cancellation detaches a caller from shared helper preparation; its finite
build can continue for other requests. Cancellation of an analysis child,
timeout and runtime shutdown terminate the applicable owned process trees;
an actual slot remains occupied until the child closes. Runtime shutdown also
stops shared builds. Source and syntax limits fail explicitly rather than
silently clipping a diagram.

Bundled Dart/Ruby syntax has a separate worker owner: one actual worker, four
queued requests and a 30-second caller deadline. Input and output are explicitly
bounded: source has at most 2,000,000 UTF-16 code units, the URI 10,000 characters,
and input/output messages 8 MiB. Cancellation, timeout and disposal terminate the worker, and its actual
lease remains occupied until exit. Engine old/young-generation and stack limits
are 128 MiB, 32 MiB and 4 MiB; these settings do not bound the entire extension
process or all native/WebAssembly memory. Unsupported constructs remain visible
with exact source ranges and notices. Parser/schema failures retain disclosed
provider call-site fallback instead of certifying complete syntax flow.

## Provider notes

- **Python:** Pylance (`ms-python.vscode-pylance`, with its Python dependency) or
  basedpyright (`detachhead.basedpyright`) resolves calls. Its selected project
  environment is separate from Codebase's trusted syntax-helper executable.
- **Java:** finish project import and use Standard mode for resolved calls.
  Lightweight mode is limited. The syntax helper needs a full JDK even when the
  provider launches with a bundled runtime.
- **Kotlin:** the language server supplies symbols/calls; the standalone SDK
  supplies PSI. Codebase does not access private server bundles. Scripts,
  multiplatform and project support depend on installed tool versions.
- **C/C++:** use one active service and configure compiler/build flags, such as
  compile_commands.json. clangd exposes its client and AST request publicly.
  Microsoft C/C++'s public API exposes no statement AST, so cpptools alone retains
  call-site flow. Implicit and expanded macro nodes may lack source ranges.
- **Rust:** the JSON tree request is a debug extension rather than a standard
  CFG API. Codebase checks recognized schemas and offsets against the buffer.
  Rust-looking string contents and opaque macro/token trees are excluded from
  executable flow. Unknown versions/schemas fall back with an explanation.
- **C# / Go:** configure C# or Go/gopls and wait for its index to resolve project
  symbols/calls. A working syntax helper does not replace that language service.
  C# `.csx` snapshots use Roslyn Script syntax mode; script declarations and
  top-level statements are parsed without resolving reference directives or
  executing the script. Available C# syntax depends on the installed SDK.
- **Dart:** wait for the installed Dart language service to index the project for
  symbols and resolved calls. Bundled statement parsing reads the unsaved string
  without running `pub get`, resolving imports or requiring the project's SDK.
- **PHP:** provider symbols and calls depend on the installed service. Detailed
  statement flow is unavailable; call-site flow preserves reported ranges and
  source order without inventing branches. Installing a provider does not prove
  complete calls or add statement syntax.
- **Ruby:** bundled Prism supplies statement syntax. Ruby LSP and Solargraph
  prerequisites belong to their respective language services; Codebase does not
  install them or assume they expose call hierarchy. Dynamic dispatch,
  metaprogramming and unsupported syntax retain explicit limitations.

Workspace discovery includes registered suffixes when files are closed and
respects custom file associations. The actual editor language chooses the
adapter. Included files remain visible when a provider is missing.

## Syntax tool references

[Python AST](https://docs.python.org/3.14/library/ast.html),
[JDK parsing](https://docs.oracle.com/en/java/javase/21/docs/api/jdk.compiler/com/sun/source/util/JavacTask.html),
[Kotlin Analysis API](https://github.com/Kotlin/analysis-api),
[clangd AST](https://clangd.llvm.org/extensions.html#ast),
[clangd extension API](https://github.com/clangd/vscode-clangd/blob/master/api/vscode-clangd.d.ts),
[rust-analyzer syntax](https://github.com/rust-lang/rust-analyzer/blob/master/docs/book/src/contributing/lsp-extensions.md#view-syntax-tree),
[Roslyn syntax](https://learn.microsoft.com/en-us/dotnet/csharp/roslyn-sdk/get-started/syntax-analysis)
[Go parser](https://pkg.go.dev/go/parser),
[Dart analyzer](https://pub.dev/packages/analyzer),
[Dart JavaScript compilation](https://dart.dev/tools/dart-compile#javascript-js)
and [Ruby Prism](https://ruby.github.io/prism/rb/docs/javascript_md.html).
