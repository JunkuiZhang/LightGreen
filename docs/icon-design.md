# 扩展图标

日期：2026-11-07。用户指出 Marketplace 使用默认图标，要求增加自定义 icon。

已实现一版原创图标：暖米白背景、橄榄绿代码括号、绿色叶片和橙色太阳。图形方案由助手拟定，不能将其记录为用户逐项确认的设计；用户此前的 Rust/C++ 配色决定保持原样。

- 文件：`images/icon.png`，256 × 256 PNG，51,348 字节；从最初的 1254 × 1254 生成结果按比例导出，无裁剪或重新设计；原始生成文件另存于本机 imagegen 输出目录。
- 扩展清单：`package.json` 的 `icon` 指向 `images/icon.png`。
- 发布：0.1.1 已在 VS Code Marketplace 与 Open VSX 上线，公开图标与 VSIX 均与本地文件逐字节一致；见 [0.1.1 记录](release-0.1.1.json)。
- 内容边界：仅新增图标、清单版本与 icon 字段、更新日志及记录；主题、Rust 语法补丁、README 与 0.1.0 包逐字节一致。
- 生成方式：内置 imagegen 工具；没有使用 CLI/API fallback，打包前使用 sharp 等比导出 256 × 256 PNG，满足 Marketplace 的 1 MB 和 1024 × 1024 上限；没有重绘图案。
- 图标 SHA-256：`2dd5f86bb67cdf0264ab4b3b9ef59911915d37802f52daa14c6eb17ea8536d6c`；原始生成文件 SHA-256：`5bd49e7a6485257bdbdd826275f9f3662513747e53b2a6c6d82cb187e591b9e9`。

[VS Code 扩展清单](https://code.visualstudio.com/api/references/extension-manifest)要求 icon 至少 128 × 128，Retina 建议 256 × 256；[发布要求](https://code.visualstudio.com/api/working-with-extensions/publishing-extension)不接受 SVG icon。本次 PNG 尺寸满足要求，VSIX 已验证包含 `Microsoft.VisualStudio.Services.Icons.Default` 图标资源。

## 最终生成提示词

```text
Use case: logo-brand.
Asset type: a single production-ready square PNG extension icon for the VS Code theme "Solarized Light Rust", which now supports Rust and C++.
Create one original, elegant, minimal flat graphic icon. Full-bleed solid warm ivory background, inspired by Solarized Light #FDF6E3. Center a bold, compact emblem combining two balanced dark olive-green curly code braces { } with one simple vivid leaf between them. The leaf has a clean negative-space vein. A small amber sun disk, integrated above the leaf within the emblem, subtly expresses "Solarized". Use the theme's palette: olive green #577909, leaf green #499504, a restrained amber accent #DB7800. The shape language must feel like a polished developer-tool brand, not a stock environmental poster. Strong recognizable silhouette at 32 and 64 px. Broad, optically consistent strokes, generous but not excessive spacing, the emblem fills about 75% of the square. Precisely centered and balanced. Crisp flat filled shapes and uniform flat background. No letters, words, slogans, watermarks, gradients, shadows, textures, gloss, perspective, device mockups, grids of alternatives, or existing Rust/C++/VS Code logos. Output only the icon itself as one square image, ideally 512 x 512 pixels.
```
