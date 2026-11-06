# 本地 VSIX 安装与验证

当前推荐 C++ 包为 `sl-rust-0.0.1-local-preview-20261107-cpp-r3.vsix`：用户已选择不为 C++ 添加可变性下划线，cpp-r4 仅保留为历史试验。主题已逐字节恢复为 cpp-r3。若装过 cpp-r4，用 --force 重装 cpp-r3 后重载；如果已在用 cpp-r3，无需操作。安装和观察入口见 [C++ 迁移记录](cpp-design.md)。以下保留 selection-r2 的历史安装说明，实际请使用 cpp-r3 文件名。

构建日期：2026-11-07。selection-r2 只调整活动/失焦文本选区，首包总体显示已获用户认可。用户随后认可新暖米色选区，保留该方案；仅打包供本地使用，不发布到 Marketplace。

## 包与身份

- 仓库产物：`/home/xiaobai/projects/LightGreen/dist/sl-rust-0.0.1-local-preview-20261107-selection-r2.vsix`
- Windows 安装副本：`C:\Users\36477\Downloads\sl-rust-0.0.1-local-preview-20261107-selection-r2.vsix`
- 扩展 ID：`JunkuiZhang.sl-rust`；版本：`0.0.1`。
- 主题选择器名称：**Solarized Light Rust (Local Preview)**；主题设置 ID 仍为 `Solarized Light Rust`。

保留原扩展 ID，所以在目标配置文件（Profile）中安装时替换同一扩展的旧副本，不会额外出现一个独立主题扩展。因为版本号仍为 0.0.1，不能只靠版本号判断新旧；应在普通窗口中确认 Local Preview 名称，并验证下面两个语法补丁。

## 安装

1. 停止 F5 调试并关闭 Extension Development Host 窗口。使用日常的普通 VS Code 窗口测试安装结果。
2. 在 **Windows PowerShell** 中执行（不是 WSL 终端）：

   ```powershell
   code --install-extension "C:\Users\36477\Downloads\sl-rust-0.0.1-local-preview-20261107-selection-r2.vsix" --force
   ```

   `--force` 用于明确重装同版本。如果使用命名 Profile，在命令末尾加上 `--profile "实际配置名称"`。也可以在目标窗口执行 **Extensions: Install from VSIX…（扩展：从 VSIX 安装）**，选取上述文件；同版本被跳过时再使用命令重装。

3. 回到普通窗口，执行 **Developer: Reload Window（开发人员：重新加载窗口）**。
4. 用 `Ctrl+K`、`Ctrl+T` 打开主题选择器，选择 **Solarized Light Rust (Local Preview)**。若仍只显示不带后缀的名称，先检查目标 Profile、扩展启用状态和是否完成重装/重载。
5. 在普通 WSL 窗口打开 `/home/xiaobai/projects/rust_theme_test`，沿用现有 rust-analyzer 与工作区设置，等待语言服务就绪。不要通过 F5 打开这一轮验证窗口。

官方说明：[从 VSIX 安装](https://code.visualstudio.com/docs/configure/extensions/extension-marketplace#_install-from-a-vsix)。VSIX 安装的扩展默认关闭该扩展的自动更新；这不会更改其他扩展的自动更新设置。

## 本轮验证

本次优先观察：同一段多行拖选应从橙色变为更淡的暖米色；双击单词也会一起变浅；切到终端后失焦选区更淡。Ctrl+F 配色保持上一版。下表是首包的功能检查，可按需回看。

| 位置 | 应看到的效果 |
| --- | --- |
| `src/attributes_macros.rs:59/61/63` | `$value` 的名称在声明、`println!` 和 `show_value($value)` 三处均为浅绿色、斜体；尤其检查普通函数调用处 |
| `src/literals.rs:10` 及 `:36–51` | raw C 字符串正确结束；后面的括号、逗号、分号不再大面积变成字符串色 |
| `src/advanced.rs:11/14/23/26` | 关联类型 `Output` 为类型名绿色、加粗 |
| 常用界面 | 双击选词、Ctrl+F、补全选中项及滚动条与此前确认的效果一致；不需要从头重复全部界面检查 |

需要确认宏参数识别时，将光标放到第 63 行 `value` 上，执行 **Developer: Inspect Editor Tokens and Scopes**，应能看到 `variable.other.metavariable.name.rust`。

`attributes_macros.rs:68` 的格式字符串 `{value}` 仍可能显示为普通变量色，这是已决定暂缓的 rust-analyzer 分类限制，不属于安装失败。两个语法补丁在扩展启用时参与 Rust 识别，切换其他主题不会停用补丁；具体颜色仍由所选主题决定。

安装后的 VSIX 是固定快照。后续修改仓库文件不会自动更新已安装版本；需要重新打包、安装和重载。继续开发时仍可用 F5 预览。

## 复现构建与恢复旧版

在已安装 Node.js（满足 vsce 的版本要求）的环境中，从仓库根目录运行：

```sh
npx --yes @vscode/vsce@4.1.0 ls --no-dependencies
npx --yes @vscode/vsce@4.1.0 package --no-dependencies --out dist/sl-rust-0.0.1-local-preview-20261107-selection-r2.vsix
```

先创建 `dist` 目录。本项目没有运行时依赖，因此使用 `--no-dependencies`。打包工具安装在临时位置即可，不需要给主题新增 npm 依赖。不要执行 `vsce publish`。

需要恢复商店版时，在目标 Profile 中卸载此扩展、重载，然后从 Marketplace 重新安装 `JunkuiZhang.sl-rust` 并选择原主题。

包内文件清单、哈希与检查结果见同目录的 `local-vsix-selection-r2-build.json`；这些记录保留在仓库中，不随扩展分发。用户安装后的视觉验证单独记录，不以打包成功代替。
