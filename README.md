# Solarized Light Rust

A light VS Code theme based on Solarized Light, with custom Rust syntax colors.

Requires VS Code 1.81.0 or later. Rust semantic highlighting depends on rust-analyzer and the token classifications it provides.

The extension also contributes a small Rust grammar injection for macro parameters such as `$value` inside function calls. It reuses Rust's existing macro-parameter scopes and excludes strings, comments, and `$crate`. This recognition fix remains active while the extension is enabled, including when another color theme is selected; the selected theme determines its appearance. See [VS Code's injection grammar guide](https://code.visualstudio.com/api/language-extensions/syntax-highlight-guide#injection-grammars).

A second Rust grammar injection recognizes raw C strings (`cr#"…"#`) so embedded quotes and backslashes do not cause subsequent code to be colored as a string. It uses the existing string palette and also remains active while the extension is enabled.

## Language coverage

- **Rust:** the main focus; 48 sample groups cover constants, mutable items, traits, macros, and optional diagnostics. The current round of common Rust color checks and readability feedback has been recorded. Coverage and rule-level tests are not a claim that every Rust construct has been visually verified.
- **C++ / clangd:** preview of the Rust color roles, including types, static items, parameters, fields, and concepts. Boolean literals `true` and `false` use the same rose color as Rust. Ordinary const/readonly variables keep their variable color, and const instance fields keep their green italic member style. C++ uses the confirmed colors and italics without mutability underlines; Rust retains its existing mutable-item underlines. A sibling `../cpp_theme_test` project provides 12 main sample groups plus standard-library/header examples; actual clangd token checks passed. Separate constexpr-variable coloring is deferred because the tested clangd emits the same readonly classification as ordinary const variables. Details and visual feedback are recorded in `docs/cpp-design.md` in the source checkout.
- **Other languages:** may use shared TextMate and semantic rules; not individually verified.

## Local preview

1. Open this repository in VS Code.
2. Select **Preview Rust Theme** in Run and Debug, then press F5. This opens the sibling `../rust_theme_test` sample project when it is available.
3. In the Extension Development Host, choose **Solarized Light Rust (Local Preview)**. The suffix distinguishes the development theme from the installed Marketplace version.

The theme setting ID remains `Solarized Light Rust`. The local preview suffix is temporary; restore the published label before packaging a release. This checkout has not been republished.

After adding or changing the grammar contribution, stop and restart the F5 development session so VS Code reloads the extension manifest and grammar. Color-only changes continue to update live.

For C++ development preview, select **Preview C++ Theme** instead. It opens the sibling `../cpp_theme_test` project; use clangd for semantic highlighting.

## Local VSIX installation

For local installation testing, keep the **Solarized Light Rust (Local Preview)** label. The package retains extension ID `JunkuiZhang.sl-rust` and version `0.0.1`, so installing it replaces the Marketplace copy in the target profile rather than creating a second extension. Use `code --install-extension <local-package.vsix> --force` to reinstall the same version, then reload a normal VS Code window and select the Local Preview theme.

The package contains the theme, both Rust grammar injections, README, change log, and license. Development settings and the repository's design/audit documents are excluded. Local installation does not publish anything. Repository documentation links below refer to the source checkout; see `docs/local-vsix.md` there for installation and sample checks.

## Review status

The current review records Rust highlighting, including comments and local-variable readability, and the observed common editor, navigation, search, completion, diagnostic, sidebar, terminal, settings, notification, and conflict-marker states. Unspecified UI colors use VS Code defaults or inheritance; they are not automatically missing features.

Debugging, the three-way merge editor, Notebook, and other specialized views remain unverified. Existing token foreground transparency and the minimap selection color are retained to preserve the accepted appearance; their compatibility limitations remain documented.

Implicit captures in format strings can lose their original parameter/constant classification in the installed rust-analyzer version. This known limitation is deferred; the theme does not recolor all captures as parameters.

- [Compatibility review and remaining work](docs/compatibility-review.md)
- [Design decisions and visual feedback](docs/theme-design.md)
- [Change log](CHANGELOG.md)
