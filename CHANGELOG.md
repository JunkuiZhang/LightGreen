# Change Log

## [0.1.1] - 2026-11-07

### Added

- Add a custom extension icon with green code braces, a leaf, and an amber sun on a warm light background.

## [0.1.0] - 2026-11-07

### Changed

- Match C++ `true` and `false` to the Rust boolean color (`#D33682`) using their specific TextMate scopes.
- Add a C++ / clangd preview of the Rust color roles: green bold types, teal recognized static items and non-type template parameters, green italic parameters and fields, and orange concepts. Preserve Rust rules and UI colors.
- Replace deprecated indentation guide color keys with `editorIndentGuide.background1` and `editorIndentGuide.activeBackground1`, preserving their color values.
- Raise the minimum VS Code version to 1.81.0, which introduced these replacement keys.
- Soften matching-text and symbol occurrence highlights, including an explicit translucent background for write-access highlights.
- Soften text selections with the original warm-beige hue and a lighter inactive selection background after local VSIX feedback.
- Use pale orange for the current find match and subtle gray for other matches.
- Adjust string foreground to `#258F94` after previewing it alongside constants and other syntax colors.
- Add a subtle gold border to other search matches, also improving matches in the Search view.
- Soften scrollbar hover and drag colors while retaining distinct states.
- Explicitly give Rust built-in attribute names the keyword color and weight, keeping derive macros and attribute arguments separate.
- Color Rust derive macro names such as `Debug`, `Clone`, and `Copy` orange and bold like traits, instead of falling back to the green type-name style.
- Explicitly style Rust attribute macros and the `derive` entry like ordinary functions, and derive helper attributes like keywords; retain orange attribute parameter names.
- Style Rust macro fragment specifiers such as `literal` and `expr` green and bold like type names.
- Style recognized Rust macro parameter names light green and italic in matcher declarations and transcriber uses, including uppercase metavariable names.

### Fixed

- Keep ordinary C++ const/readonly variables in the variable color and const instance fields green and italic. Remove the broad readonly-to-constant color mapping; defer separate constexpr-variable coloring where clangd does not distinguish it.
- Recognize raw Rust C-string boundaries so embedded quotes and backslashes do not leave subsequent code and punctuation colored as strings.
- Keep Rust associated types green and bold instead of inheriting the light teal static-item style; preserve constant, static, and function colors.
- Recognize Rust macro parameters inside function calls with a scoped grammar injection, reusing their existing theme style while preserving ordinary variables, comments, strings, and `$crate`.
- Match Rust constant and static item names explicitly and keep `const` keywords in the keyword color. Functions with constant/static modifiers retain the original light teal color.
- Exclude Rust operators from mutable-token underlines, including specialized arithmetic and bitwise tokens, while preserving underlines on mutable identifiers.
- Keep selected completion-item text as dark as ordinary suggestions instead of inheriting the lighter global list selection foreground.
- Keep remote-connection status-bar text white on hover.
- Correct the Rust semantic selector `comparision:rust` to `comparison:rust`, preserving its foreground color. It applies when the language server emits that specialized token type.

## [0.0.1] - 2021-07-24
### Added
- Initial commit
- Support for Rust, C++
