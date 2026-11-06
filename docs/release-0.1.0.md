# 0.1.0 双平台发布记录

日期：2026-11-07。状态：用户已授权代为发布，源码已推送 GitHub。**Open VSX 0.1.0 已发布，公开下载包与本地包逐字节一致；Marketplace 保存的 PAT 已过期，尚未上传，等待用户更新令牌。** 未操作用户的已安装扩展。详细机器核对见 [发布结果](release-0.1.0-publication.json)。

## 包与身份

| 项目 | 值 |
| --- | --- |
| 扩展 ID | `JunkuiZhang.sl-rust`，保持不变 |
| 新版本 | `0.1.0` |
| 主题名称及设置 ID | `Solarized Light Rust`；正式包去掉 Local Preview 后缀 |
| 最低 VS Code 版本 | `^1.81.0` |
| 许可证 | `MIT`；沿用现有 LICENSE，未改版权署名 |
| 仓库包 | `dist/sl-rust-0.1.0.vsix` |
| Windows 副本 | `C:\Users\36477\Downloads\sl-rust-0.1.0.vsix` |
| 文件大小 | 13,507 字节 |
| SHA-256 | `430a17e908398f231be28c51e1bd415542301ed083c1739253127b32cf80b74b` |

两个平台可上传同一个通用 VSIX。主题和两条 Rust grammar 与已确认的 cpp-r3 逐字节一致；此次只改变版本/正式名称/描述/许可证元数据、README 与 CHANGELOG。Rust 保留可变性下划线，C++ 不加下划线。

## 本次更新摘要

- 完善 Rust 常量、关联类型、trait/derive、属性和宏参数的着色，保留可变项下划线并排除运算符。
- 补充 Rust 普通函数调用内宏参数和原始 C 字符串的语法识别。
- 为 C++ / clangd 引入对应的类型、概念、参数、字段、静态项和布尔值配色，普通 const 变量保留变量角色。
- 减淡选区、搜索/引用匹配和滚动条，修正补全列表与远程状态栏悬停文字。
- 替换弃用缩进线颜色键，最低 VS Code 版本升为 1.81.0。

已记录的语言服务分类、透明度约定和未覆盖场景继续保留，见 [兼容性记录](compatibility-review.md)。本次并未宣称所有版本和界面均已通过验证。

## 本地安装检查

Windows PowerShell：

```powershell
code --install-extension "C:\Users\36477\Downloads\sl-rust-0.1.0.vsix" --force
```

重载窗口，确认扩展版本为 **0.1.0**，主题名称为 **Solarized Light Rust**，不再带 Local Preview。配色沿用 cpp-r3；无需重做全部颜色验收，可快速确认 Rust 可变项有下划线、C++ 无可变性下划线、C++ true/false 为玫红，以及原始 C 字符串正确结束。

如需从源码开发预览，在 Run and Debug 选择 Preview Rust Theme 或 Preview C++ Theme 后 F5；兄弟目录样例存在时会自动打开。正式版与开发版显示名称相同，通过 Extension Development Host 窗口及扩展版本区分。

## 商店状态与剩余步骤

发布前通过两个公开 API 只读核对，两商店版本均为 **0.0.1**。当前状态：

- [VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=JunkuiZhang.sl-rust)：PAT 验证返回 `Access Denied: The Personal Access Token used has expired.`，没有执行发布请求。
- [Open VSX 0.1.0](https://open-vsx.org/extension/JunkuiZhang/sl-rust)：`ovsx 1.3.0 verify-pat` 通过，发布命令成功；[版本 API](https://open-vsx.org/api/JunkuiZhang/sl-rust/0.1.0) 与公开下载包确认 0.1.0、MIT、`^1.81.0` 和正式主题名称。下载 SHA-256 与本页包记录一致。

源码已同步至 GitHub，包内 README 的相对文档链接已由 vsce 改写为仓库链接。Open VSX 的发布权限已验证；Marketplace 需用更新后的令牌重新验证。

2026-11-07 发布执行记录：

- `git push origin master` 成功，远端 `master` 核对为 `394a92c21a759e48576bf2cdaf94db201ecc73a6`。
- WSL 的 `vsce ls-publishers` 未列出已登录发布者；WSL 未设置 `VSCE_PAT` / `OVSX_PAT`，Windows 的 Process/User/Machine 范围也未设置这两个变量；WSL 和 Windows 用户目录下均未找到 `.vsce`。这不代表用户在其他位置没有凭据。
- 尝试通过 computer-use 使用已有浏览器登录，初始化因 `sandboxCwd is not a local file URI: file:///home/xiaobai/projects/LightGreen` 失败，重置后仍失败；没有进入登录页或进行上传。
- 用户随后提供两个凭据文件的本机位置。令牌只在进程内读取并通过相应 CLI 的环境变量使用，没有写入 Git、命令行参数或发布记录，也未修改原凭据文件。
- Marketplace 的旧令牌验证失败，明确原因是过期。已向用户提供 Azure DevOps 重新创建 PAT 的步骤；等待更新原文件后再次验证。
- Open VSX 使用同一个 13,507 字节 VSIX 发布成功，公开版本和下载内容均已确认。最初 latest 响应仍缓存 0.0.1，因此另行核对精确 0.1.0 端点及实际文件；随后绕过缓存的 latest 查询也确认 0.1.0，没有重复上传。

本次使用的发布命令（通过环境变量传递对应凭据）：

```sh
npx @vscode/vsce@4.1.0 publish --packagePath ./dist/sl-rust-0.1.0.vsix
npx ovsx@1.3.0 publish ./dist/sl-rust-0.1.0.vsix
```

Open VSX 发布命令已执行成功，不再重跑。Marketplace 发布命令尚未执行；等用户更新令牌后，先运行 `vsce verify-pat JunkuiZhang`，成功后发布同一包。vsce 读取 VSCE_PAT，Open VSX CLI 读取 OVSX_PAT；不要把两平台令牌混用。

Marketplace 发布完成后还需核对公开 0.1.0 及下载内容。当前仅 Open VSX 已完成，不能将其成功扩大为两个商店均已更新。

依据：[VS Code 发布指南](https://code.visualstudio.com/api/working-with-extensions/publishing-extension)、[Open VSX 发布指南](https://github.com/eclipse-openvsx/openvsx/wiki/Publishing-Extensions)、[Open VSX 环境变量支持](https://github.com/eclipse-openvsx/openvsx/blob/master/cli/src/util.ts)。

## 构建与验证记录

```sh
npx @vscode/vsce@4.1.0 package --no-dependencies --out dist/sl-rust-0.1.0.vsix
```

使用 @vscode/vsce 4.1.0、Node 24.22.1。ZIP 完整性、9 个文件的清单、版本及身份、主题/语法补丁与 cpp-r3 一致性、Windows 副本哈希均通过。正式包不含开发配置、设计审计文档、测试项目或旧 VSIX。详细记录见 [release-0.1.0-build.json](release-0.1.0-build.json)。
