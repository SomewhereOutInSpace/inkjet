### 0.11.1
- (Bugfix) - Helix themes now properly inherit from parent names when not defined (that is, if `ui.x.y` isn't defined but `ui.x` is, `ui.x.y` copies `ui.x`.) (Thanks @guilhermeprokisch for catching this one.)
- (Bugfix) - example terminal highlighter now works properly.
- (Bugfix) - updated Rust and Ada highlighting queries to behave properly.

### 0.11.0
- (Improvement, **breaking**) - the `Theme` API has been rebuilt to work with [Helix editor themes](https://docs.helix-editor.com/themes.html#modifiers) instead of the previous bespoke implementation.
  - The `theme` module has been hoisted out of the `formatter` module, and now lives at the crate root.
  - The new `theme::vendored` module includes a large collection of themes (in TOML definition format) vendored from the Helix project.
  - The `ThemedHtml` formatter has been reworked to use the new `Theme` API.
  - The `theme` and `html` features are no longer mutually dependent.
- (Improvement) - a new `Terminal` formatter (using the `termcolor` crate) has been added. (@guilhermeprokisch)
- Updated `tree-sitter` to `0.23.0`. (@leandrocp)
- Replace `once_cell` dependency with `std::sync::LazyLock`.
- Update `ahash` and `toml` dependencies.
- Added languages:
  - Fish (shell)
  - Julia
- Updated languages:
  - Ada
  - Assembly (generic)
  - Astro
  - Awk
  - Bash
  - Bicep
  - Blueprint
  - C
  - Cap'n Proto
  - Clojure
  - C#
  - Common Lisp
  - C++
  - CSS
  - Cue
  - D
  - Dart
  - Diff
  - Dockerfile
  - Emacs Lisp
  - Elixir
  - Elm
  - Erlang
  - Forth
  - Fortran
  - GDScript
  - Gleam
  - GLSL
  - Go
  - Haskell
  - HCL
  - Heex
  - HTML
  - INI
  - Java
  - JS
  - JSON
  - Kotlin
  - Matlab
  - Meson
  - Nix
  - OCaml
  - OpenSCAD
  - Pascal
  - PHP
  - Python
  - R
  - Racket
  - Regex
  - Ruby
  - Rust
  - Scala
  - Scheme
  - SQL (generic)
  - Svelte
  - Swift
  - TS
  - TSX
  - Vimscript
  - x86 Assembly
  - Zig
- Removed languages due to a lack of working highlight queries:
  - Astro
  - Common Lisp
  - Iex
- Removed the extremely bloated Nim parser due to `crates.io` size limits; if you need to highlight Nim, you can [add the bindings](https://github.com/alaviss/tree-sitter-nim/tree/main/bindings/rust) as a separate dependency and use `Language::Runtime` to load the grammar.


### 0.10.5
- Add Objective C language.
- Updated Rust, Svelte, and Python languages.

### 0.10.4
All thanks to @leandrocp.
- (Bugfix) - Fixed Lua not highlighting correctly.
- Updated languages:
  - Elixir
  - Erlang
  - Gleam
  - HEEx
  - Lua
  - CSS
  - HTML
  - Javascript
  - Python
  - Ruby
  - SQL

### 0.10.3
- Added Svelte language.

### 0.10.2
- Added Vimscript, TSX and JSX languages.
- Update TOML dependency to `0.8`.

### 0.10.1
- (Bugfix) - Fix Javascript and Typescript highlighting (thanks @leandrop!)
- Updated Haskell grammar.

### 0.10.0
- (Bugfix, **breaking**) - The `ThemedHtml` formatter is now feature-gated behind `theme` instead of `html`.
- (Improvement) - Remove `rayon` and `indoc` from build dependencies, replacing them with `std::thread::spawn` and `format!`. This should speed up build times by a few seconds.
- (Improvement) - Gate development portions of the build script behind a feature, further improving build times.
- (Improvement) - `Language::from_token` and `Highlight::highlight_*` now accept an `AsRef<str>` instead of a plain `&str`.
- (Feature) - Added the `Runtime` variant to `Language`, which holds an `fn() -> &'static HighlightConfiguration`. This makes it possible to use languages not statically known by Inkjet.

### 0.9.2
- (Bugfix) - Fix missing `cfg` attribute that made compiling without the `theme` feature impossible.
- (Bugfix) - Fix typo in inline HTML formatter.
- (Feature) - Added new `plaintext` language. Under the hood, this uses the `diff` grammar (small and cheap to load) but doesn't provide any highlighting queries, which makes highlighting with it a no-op.
- Tidied up a few docs and added some additional tests.

### 0.9.1
- (Bugfix) - `highlight_to_writer` no longer erroneously shells out to `highlight_to_fmt`.

### 0.9.0
- (Bugfix, **Breaking**) - Due to an error in the test code, highlight configurations were not properly tested prior to this version. The following languages have been removed, as no working queries are available:
    - CMake
    - Jinja2
    - Julia
    - Just
- (Thanks again to @leandrop for the detective work. Hopefully I'll be able to re-introduce these languages down the road.)

### 0.8.1
- Added HEEx, IEx and EEx
- Fix some typos and missing names in `HIGHLIGHT_NAMES`
- Fix HCL not being properly included in the languages list

Thank you to @leandrocp for the PRs!

### 0.8.0
Changes to formatters:
- (Feature) Added the `IoWrapper` newtype, which allows instances of `io::Write` to be treated as an instance of `fmt::Write`.
- (Feature) Added optional `start_*` and `finish_*` methods to `Formatter`. These methods are called once before and after highlighting, which is useful for formatters that need (say) to write a header or footer. By default, these methods are a no-op.
- (Feature) Added theme-aware HTML formatter. This formatter generates HTML `span` elements with inline styles rather than class names, which is ideal for simpler use cases.
  - The new `Theme` type is additionally output-agnostic, making it usable in custom formatters for non-web use cases.
  - `Theme`s can be loaded from TOML files, or created programmatically.
  - Currently no themes are included, but I will likely bundle some in the future.
- (Improvement, <b style="color: #ff0000;">Breaking</b>) - `Formatter` now only requires an implementation of `write` (which takes an instance of `fmt::Write`). The `*_io` variants now have default implementations that use the aformentioned `IoWrapper`.

Changes to the primary highlighting API:
- (Feature) Added new `highlight_raw` method to `Highlighter`, which gives direct access to the underlying iterator of `HighlightEvents`. For advanced use only.
- (Feature) Added new `highlight_to_fmt` method to `Highlighter`, which highlights to an instance of `fmt::Write`. This is useful for highlighting into an existing `String` or more exotic implementors like `OsString`.

Other changes:
- (Bugfix) Corrected some erroneous documentation.

### 0.7.0
- (Feature) Added new languages:
  - WASM (WAT and WAST)
- (Enhancement, <b style="color: #ff0000;">Breaking</b>) `HIGHLIGHT_CLASS_NAMES` now separates selectors with spaces rather than dashes. For example, `type-builtin` is now `type builtin`. This makes theming in CSS easier, as you can simply style (say) the `keyword` class and have that automatically apply to every `keyword` variant. (Of course, you can override this by using chained selectors, such as `.keyword.function`.)

###  0.6.0
- (Feature) Added new languages:
  - Astro
  - Awk
  - Bicep
  - CMake
  - Forth
  - GDScript
  - Gleam
  - INI
  - Jinja2
  - Julia
  - Just
  - LaTeX
  - YAML
- (Feature) Highlighting queries from the [Helix](https://github.com/helix-editor/helix/) project are now used where possible.
- (Feature) Added the ability to enable and disable certain languages via Cargo features. All languages are enabled by default (using the `all-languages` feature.)

### 0.5.2
- (Bugfix) Removed a stray debug `println!` from the `write_io` implementation of the HTML formatter.
