# Solarized Light Rust

A Solarized-based light theme for Rust and C++, with a warm background, green type names, and distinct colors for functions, traits, constants, and values.

## Install

Install **Solarized Light Rust** from the [VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=JunkuiZhang.sl-rust) or [Open VSX](https://open-vsx.org/extension/JunkuiZhang/sl-rust). Then run **Preferences: Color Theme** and select **Solarized Light Rust**.

Requires VS Code **1.81.0 or later**, or a compatible editor. Use **rust-analyzer** for Rust and **clangd** for C++ to get semantic highlighting.

## Highlighting

| Role | Appearance |
| --- | --- |
| Types, structs, classes, and enum type names | Green and bold |
| Rust traits and derive names; C++ concepts | Orange and bold |
| Ordinary functions and macros | Blue |
| Parameters and instance fields | Light green and italic |
| Rust constants/static items; recognized C++ static items and non-type template parameters | Light teal |
| Enum members, numeric values, and booleans | Rose |
| Strings | Teal |
| Comments | Muted gray |

Rust mutable symbols have an underline; operators do not. C++ keeps the colors and italics without mutability underlines. Ordinary C++ `const` variables retain the variable color, while `const` instance fields retain their field style. Recognized static/constant function categories may use light teal instead of ordinary function blue.

Selections, search matches, symbol occurrences, completion lists, and scrollbars use softer highlights suited to the light background. Other workbench colors inherit VS Code's light-theme defaults where appropriate.

## Rust syntax fixes

The extension includes grammar injections for macro parameters inside function calls and raw C strings. They preserve the intended parameter and string colors and prevent raw C strings from incorrectly coloring subsequent code. These syntax fixes remain active while the extension is enabled, including when you select another theme.

## Known limits

Semantic colors depend on the language server's classifications. Rust implicit captures in format strings can lose their original parameter or constant classification. The tested clangd does not distinguish ordinary `const` variables from `constexpr` variables, and C++ macro replacement-body parameters are not always recognized as parameters.

C++ numeric prefixes, suffixes, and exponent markers can retain the keyword/operator color around rose digits. Specialized views, other languages, and all language-server versions have not been fully verified. Existing foreground-transparency and minimap-selection compatibility exceptions are documented in the [compatibility review](docs/compatibility-review.md).

See the [change log](CHANGELOG.md) for updates and the [design record](docs/theme-design.md) for detailed decisions. The extension is distributed under the [MIT license](LICENSE).
