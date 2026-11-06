# 兼容性复核与配置收尾

复核日期：2026-11-07，Asia/Shanghai。基于本机 VS Code **1.145.2**、rust-analyzer **0.3.3081** 、clangd **23.1.0** 及当前主题文件。

结论：本轮 Rust 常用样例与常用工作台的观察已完成整理，首个本地 VSIX 获用户“目前看起来一切正常”的总体反馈。用户随后要求多行选区更淡、偏回原色，已试调活动/失焦选区为 `#E7C49666` / `#E7C49644`，用户反馈“我天，好太多了”，新暖米色选区已获认可并保留。配置复核通过，生效界面键未发现未知项或弃用项；随后新增 C++ / clangd 首轮配色迁移；颜色获认可后曾试加下划线，用户已决定 Rust 保留、C++ 不加，主题恢复 cpp-r3，详见 [C++ 记录](cpp-design.md)；Rust 配色、两条语法补丁和 UI 保持原样，发布状态见下段。

验收范围：用户确认的类别和界面保持现状；原始 C 字符串修复已获实际显示反馈。宏参数补丁与关联类型 Output 已按明确设计实施并通过规则测试，但没有将其他区域的泛指反馈当成两者的单独修改后验收。未观察状态和其他语言不算全部通过。格式捕获分类与两类透明度约定仍是保留限制。C++ constexpr 变量与普通 const 的精确区分按用户要求暂缓：无其他可识别修饰符时均使用普通变量色，见 [readonly 修复](cpp-readonly-fix-audit.json)。

正式发布状态：源码已推送 GitHub，VS Code Marketplace 与 Open VSX 均已发布 0.1.0，两个公开商店的最新版本和下载包均核对通过。主题名为 Solarized Light Rust，最低版本与已确认主题/语法内容不变；Marketplace 旧 PAT 过期问题已由用户更新令牌解决。见 [发布记录](release-0.1.0.md)。下面的 cpp-r3 / Local Preview 内容保留为历史预览记录，不代表最新发布状态。

## 静态检查结果

| 项目 | 结果与范围 |
| --- | --- |
| 主题 JSONC | 可解析；无重复键；保留合法注释和尾随逗号 |
| 94 个生效界面颜色键 | 78 个匹配本机直接注册，16 个匹配终端 ANSI 循环注册；未知键 0，弃用键 0 |
| 43 条语义规则 | 收尾时修正 `comparision:rust` → `comparison:rust`，随后补齐内置属性、派生宏、属性宏、派生辅助属性和关联类型规则；首轮新增 15 条 C++ 专用规则，cpp-r2 按用户要求移除 variable/property 的 readonly 改色，cpp-r4 下划线试验撤回后恢复 13 条 C++ 规则；类型/修饰符与 rust-analyzer 及实际 clangd LSP legend 对照，无未识别项 |
| 59 条 TextMate 规则 | 颜色格式和字体样式检查通过，无不受支持的 token 背景设置；不据此声称每个 scope 都在所有语言中实际命中 |
| 2 条 Rust grammar 注入 | 通过 `contributes.grammars` 注册，分别补充函数调用内宏参数与原始 C 字符串的识别；实际分词前后对比通过；原始 C 字符串修复已获用户显示反馈，宏参数补丁未单独收到修改后反馈 |
| 479 条注释颜色条目 | 不参与主题配置，不算显式覆盖；原有注释保留，不能把其中旧值当成当前 VS Code 默认值 |
| 扩展入口 | 主题文件路径、`uiTheme: vs`、`type: light` 和语义高亮开关一致 |
| VS Code 版本 | 最低声明保持 `^1.81.0`，对应此前缩进线替代键的最低版本；本轮在 1.145.2 上静态核对，未逐版本启动验证 |

机器可读快照：[compatibility-audit.json](compatibility-audit.json)，包含主题 SHA-256、每个生效界面键的注册依据及待办列表。语义 token 证据沿用 2026-11-06 的实际 LSP 初始化信息与 [Rust 样本审计](rust-sample-coverage.json)，并核对当前安装扩展声明；本轮未重跑全部语义高亮或截图回归。

依据：[官方颜色接口](https://code.visualstudio.com/api/references/theme-color)、[主题 Schema](https://github.com/microsoft/vscode/blob/1.145.2/src/vs/workbench/services/themes/common/colorThemeSchema.ts)、[颜色注册约束](https://github.com/microsoft/vscode/blob/1.145.2/src/vs/platform/theme/common/colorUtils.ts)。

## 本轮保留的 Rust 配色

| 类别 | 当前方案与状态 |
| --- | --- |
| 类型、枚举类型、泛型类型参数、关联类型 | 绿色 `#577909`、加粗；Output 专用覆盖已实施并通过规则验证 |
| 数字、枚举成员、布尔值 | 玫红 `#D33682`；已观察字面量显示保留 |
| 常量、静态项、const 泛型、带 static/constant 的函数 | 保留浅青绿色 `#017A56B7` 及已确认的函数例外；不在收尾时去除 alpha |
| 普通函数与函数式/属性宏 | 沿用蓝色 `#146DAD` |
| trait、派生宏名称、保留的属性参数 | 橙色 `#DB7800`；derive 入口仍是函数蓝色 |
| 参数、字段、闭包参数、宏参数、循环标签 | 浅绿色 `#499504`，按各类别保留斜体；宏参数识别补丁已实施 |
| 可变性 | 仅叠加下划线；Rust 运算符排除；不改变类别本身的斜体 |
| self / Self / 生命周期 | self 深色斜体；Self 与生命周期深色普通字体，用户已认可 |
| 内置属性、派生辅助属性 | 关键字深色 `#073642`；宏片段说明如 expr/literal 按类型绿色加粗 |
| 字符串正文 | 保留已认可的 `#258F94`；转义、格式标记按自身类别处理 |
| 普通局部变量 / 注释正文 | `#A57800` / `#93A1A1`，用户最新确认可读性通过 |

当前不继续扩展专项观察。格式字符串捕获丢失 parameter/const 分类按用户决定暂缓；常量文字 alpha 与小地图选区透明度约定维持原样。具体证据与逐项反馈见 [设计记录](theme-design.md)。

## 配置实施记录

`comparision:rust` 拼写错误，实际已注册的类型是 `comparison`，父类型为 `operator`。改为 `comparison:rust` 后，原有前景色 `#146DAD` 保持不变。

这是让原规则能够匹配正确类型，不保证语言服务一定输出该分类。此前普通和细分运算符样本都未产生 `comparison`；如果以后语言服务输出此类型，新规则才会命中。不更改 rust-analyzer 的运算符细分选项，也不改 mutable 下划线例外。

后续设计实施（2026-11-07）：用户要求内置属性名采用关键字色，新增 `builtinAttribute:rust`，复用现有 `keyword` 的 `#073642` 和 `bold: false`。`cfg`/`cfg_attr` 原本已为 keyword；派生入口、派生宏名和属性参数不受新增规则影响。命中边界见 [内置属性审计](rust-builtin-attribute-audit.json)，整体宏设计仍在讨论。

随后用户截图确认 `Copy` 等 derive token 仍回退到类型绿色，已补加 `derive:rust` → `#DB7800`、加粗，与 Trait 一致；`derive` 入口、内置属性、其他宏和属性参数不受这条规则影响。见[派生宏着色审计](rust-derive-color-audit.json)。用户随后确认“现在正常了”，这项已获预览认可；属性宏、辅助属性和其他 Rust 类别仍按设计记录继续确认。

用户随后明确同意试用属性宏函数蓝色、辅助属性关键字色、参数保留橙色。已新增 `decorator:rust` → `#146DAD`、不加粗，以及 `deriveHelper:rust` → `#073642`、不加粗；参数和已认可的派生宏规则保持原样。匹配和边界证据见 [属性宏审计](rust-attribute-macro-audit.json)，用户随后反馈“这部分没问题”，已观察的属性宏、派生辅助属性及参数配色记录为通过；宏定义参数和其他 Rust 类别继续单独确认。

用户提议 `literal`、`expr` 等宏片段说明采用类型方案，已将 `variable.other.metavariable.specifier.rust` 加入原有类型名 TextMate 规则组，复用 `#577909` 和 bold；规则数量不变。使用真实 Rust grammar 前后分词验证，实际样本仅片段说明变化，宏参数名、标点和普通同名变量保持原样。见[片段说明着色审计](rust-fragment-color-audit.json)，用户随后反馈“这部分没问题”，片段说明配色已通过。

本轮按用户要求新增宏参数名称规则：`variable.other.metavariable.name.rust` 与 `entity.name.type.metavariable.rust` → `#499504`、italic。真实 grammar 对比确认当前两个样本的声明与宏体引用共 8 个名称命中，大写参数也有内存对照；普通名称、标点、`$crate` 与片段说明保持原样。见[宏参数着色审计](rust-macro-parameter-audit.json)。内置语法在普通函数调用 `helper($value)` 中会把 `value` 标为普通变量，仅靠主题规则无法区分。用户复现并要求修复后，已按下段补充语法注入。

用户随后确认普通函数调用缺口并要求修复。新增 `syntaxes/rust-macro-parameters.tmLanguage.json`，由 `package.json` 的 `contributes.grammars` 注入 `source.rust`，选择器限定函数调用且排除字符串/注释，匹配时排除 `$crate`。名称使用已有宏参数 scope，主题文件保持原样。安装版 grammar 对 10 个实际源码和边界对照验证通过，见 [注入修复审计](rust-macro-injection-audit.json)。该语法贡献随扩展启用，切换主题不会停用；当前仍待重启 F5 开发窗口后的视觉验收，不将分词验证等同于完整界面回归。

格式字符串的原符号类别仍存在独立限制：用户截图与新的实际 LSP 采集确认 `println!("{value}")` 中的函数参数输出普通 `variable`；内存对照中的常量也相同。准确恢复需要语言服务提供原类别，不能仅改主题规则或扩展现有 TextMate 注入。用户已选择暂缓，此项尚未修复，见 [格式捕获审计](rust-format-capture-audit.json)。显式写成 `println!("{}", value)` 时，字符串外实参仍可保持参数样式；本轮没有改动用户样例。

用户确认 Rust 关联类型 `Output` 应按类型名绿色加粗，已新增 `typeAlias.associated:rust` → `#577909`、bold，覆盖通用 static 样式。仅增加一条主题语义规则；三个留存模式每组 13 个关联类型样本变化，常量/静态项/const 泛型/函数等保持原样，详见 [关联类型审计](rust-associated-type-audit.json)。静态检查通过，修改后视觉验收待反馈。

用户截图中的后续括号/分隔符变成字符串色，已确认为 `literals.rs:10` raw C 字符串引起的内置 grammar 边界错误。新增 raw C 字符串注入，恢复正确的引号与 # 结束条件；主题文件不变。截图区域 40 处标点恢复正常 scope 与深色，其他 9 个实际源码及普通字符串/注释对照不变，详见 [raw C 字符串审计](rust-raw-c-string-audit.json)。扩展清单新增第二条语法贡献，用户随后反馈“现在一切正常了”，本次修复和已观察显示通过；未观察的字面量边界仍不宣称全部验收。

## 已观察界面的处理方式

“保留默认/继承”是有意选择，未显式定义的键不自动表示主题不兼容。具体反馈和观察边界见 [设计记录](theme-design.md)。

| 范围 | 当前处理 |
| --- | --- |
| Rust 语法与语义 | 已确认规则继续保留；48 组样本的正常显示不等于逐类验收完成，注释和普通局部变量可读性已获确认，本轮观察记录收尾 |
| 选区、搜索、引用、补全、悬浮、诊断 | 活动/失焦文本选区在本地安装后试调为 `#E7C49666` / `#E7C49644`，新暖米色方案已获用户认可；搜索、引用、补全、悬浮、诊断保留原方案 |
| 侧边栏、标签页、常用状态栏、远程连接项 | 保留已观察状态的配色，包含远程悬停白字修正 |
| Diff、已观察 Git 标记、终端 | 保留现有默认/继承项和明确决定保留的终端 16 色 |
| Peek、引用列表、Sticky Scroll、面包屑 | 保留现有显式或继承配置 |
| 小地图、滚动条、括号与缩进线 | 保留已认可方案；滚动条悬停 `#64646480`、拖动 `#64646499` |
| 设置页、已观察通知中心状态 | 保留默认/继承配置；未出现的通知弹出层不算已经覆盖 |
| 普通编辑器中的合并冲突标记 | 当前版本、传入版本、共同祖先及已观察操作链接通过；保留默认/继承配置 |

## 暂留的两类透明度问题

1. **常量/静态项文字透明度。** 当前 7 条语义前景（含 C++ 的 3 条复用）和 1 条 TextMate 规则使用 `#017A56B7`。[官方语义高亮指南](https://code.visualstudio.com/api/language-extensions/semantic-highlight-guide#theming)不支持透明文字前景，但此前已核对当前桌面实现会保留 alpha。直接删去 alpha 会变深；预先与编辑器背景混合也不能同时保持选区、当前行和悬浮背景上的表现。因此继续保留用户认可的颜色，列为兼容性限制。
2. **小地图选区背景。** `minimap.selectionHighlight: #EEE8D5` 为不透明色，而该项注册要求透明度。用户已确认当前观察效果可接受，本轮保留；不将它记为规范问题已解决。后续若修改，应单独观察小地图选区与缩略代码叠加，不能借收尾自动改色。

本轮没有为这两类问题添加用户设置覆盖，也没有通过修改 Schema 来掩盖提示。

## 暂不扩展的场景

用户明确表示专项界面很少用，优先完成兼容性和配置整理。因此调试界面、三路合并编辑器、Notebook、Chat/Agent、测试视图及其他未观察状态先记为**未覆盖**，继续沿用现有配置。不是已验收，也不推断为不需要支持；实际使用时再补查。

## 本地打包与发布状态

- 用户已授权仅制作本地 VSIX，尚未授权发布。已生成 `dist/sl-rust-0.0.1-local-preview-20261107-cpp-r3.vsix` 并复制到 Windows Downloads，见 [C++ 预览说明](cpp-design.md)及[通用安装方法](local-vsix.md)。
- 使用 @vscode/vsce 4.1.0 打包，ZIP 完整性、文件清单及运行文件与源码逐字节一致性检查通过；两条语法补丁完整包含。构建清单和 SHA-256 见 [本地包记录](local-vsix-cpp-r3-build.json)。
- 本地包保留 `Solarized Light Rust (Local Preview)` 标识、版本 `0.0.1` 与原扩展 ID；安装会替换目标 Profile 中的旧副本，同版本可用 `--force` 重装。首包已获用户总体正面反馈，selection-r2 的新暖米色选区已获用户正面反馈；cpp-r2 收窄 readonly 配色已获正面反馈；cpp-r3 将 true/false 与 Rust 的玫红统一，用户已确认本组字面量与行注释没有问题；cpp-r4 下划线试验已按用户决定撤回，当前推荐 cpp-r3；未代替用户安装或将打包成功当作视觉验收。
- `.vscodeignore` 排除开发配置、设计审计文档和构建产物；本次撤回 C++ 下划线试验，主题逐字节恢复 cpp-r3，已认可选区和 Rust 语法文件不变；未发布或提交 Git。
- 正式发布时再恢复主题显示名、确定新版本和发布说明。

本地安装不依赖 Marketplace 发布，后续修改仓库文件也不会自动改变已安装快照。

C++ 布尔值后续实施（2026-11-07）：按用户要求将 true/false 由金黄改为 Rust 的 #D33682，复用现有 TextMate 规则，计数仍为 59。实际分词前后对比仅两个 C++ 字面量前景变化；字体、其他 C++ token、Rust 样例与语义规则不变，见 [布尔值审计](cpp-boolean-color-audit.json)。用户已确认本组显示没有问题；随后标准库/头文件及预处理器、块注释补充组也获认可，见 [补充样例审计](cpp-library-sample-audit.json)。下一步建议在实际项目和公司 clangd 环境中试用，尚未执行；C++ 下划线试验已按用户决定撤回，此项选择已确认；已知分类缺口仍保留。

C++ 下划线试用（2026-11-07）：用户明确要求添加，已对 variable/parameter/property 开启 underline，并用三条 readonly 规则取消。只影响 C++，不改颜色/加粗/斜体/删除线，也不对函数、类型、宏或运算符加线。470 个实际留存 token 的样式对比仅 149 个名称增加下划线，配置检查通过；按 readonly 近似可变性的指针及表达式边界仍保留。见 [下划线审计](cpp-underline-audit.json)。用户随后接受 Rust 保留、C++ 不加的建议，本试验已撤回；当前主题与 cpp-r3 完全一致，静态复核通过，无需重新打包。
