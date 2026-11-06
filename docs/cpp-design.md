# C++ / clangd 第一轮迁移

日期：2026-11-07。

发布状态：正式 0.1.0 已发布到 VS Code Marketplace 与 Open VSX，两个公开版本和下载包均核对通过，沿用已确认的 cpp-r3 配色，主题名恢复 Solarized Light Rust。安装方法与发布结果见 [发布记录](release-0.1.0.md)。后文带 Local Preview 的安装记录对应此前 0.0.1 预览包，不代表最新发布状态。

用户要求把 Rust 着色思想引入 C++，明确公司主要用 clangd。用户最初先确认颜色和斜体，随后试加下划线；讨论取舍后已明确采用“Rust 保留下划线、C++ 不加下划线”。cpp-r4 试验撤回，主题恢复为 cpp-r3，此项样式决定已确认。已完成第一轮实现与样例；修复 C++20 构建配置后，用户反馈“看起来不错”，保留已观察效果。用户随后对模板 T/N、类型别名、concept 橙色与抽象类绿色明确回复“符合”，本组已确认。lambda 参数、捕获变量和结构化绑定也获用户确认；指针/引用组中，用户指出 readonly 浅青绿范围过大；用户已授权按符号角色收窄映射，现已实施；constexpr 精确识别暂缓。用户随后对 cpp-r2 反馈“目前看起来还行”，保留本轮已观察的收窄效果。用户随后对宏与属性组回复“okay，接下来呢”，保留当前显示；随后对构造/析构、this、运算符重载与特殊函数组回复“都没问题”，本组已观察状态通过。字面量组中，用户要求 true/false 与 Rust 同色，已实施玫红 #D33682；用户随后反馈“这一组没问题了”，本组已观察字面量、行注释及 cpp-r3 布尔值改色获确认。用户随后确认标准库、跨文件引用、条件编译和块注释补充组也没有问题；现有样例的常用配色首轮收尾。公司实际项目及 clangd 环境试用仍是后续建议；当前使用 cpp-r3 的无下划线方案，已知分类缺口继续保留。

样例项目：`/home/xiaobai/projects/cpp_theme_test`。主文件 `main.cpp` 共 12 组，覆盖类型、枚举、常量、静态项、参数、字段、this、函数、模板、concept、抽象类、重载、lambda、指针/引用、宏和字面量。使用 C++20，无外部依赖；具体观察步骤和分类边界写在样例 README。

## 本轮规则

首轮新增 15 条 `:cpp` 语义规则；cpp-r2 移除两条宽泛 readonly 改色后保留 13 条。cpp-r4 下划线试验已按用户选择撤回，当前恢复 cpp-r3 的 13 条 C++ 规则。Rust 和其他语言的语义选择器未改动：

| 类别 | 方案 |
| --- | --- |
| type/class/enum/typeParameter | `#577909`，加粗；包含 struct、类型别名及抽象类 |
| typeParameter.readonly | `#017A56B7`，不加粗；样例的非类型模板参数 N |
| concept | `#DB7800`，加粗；按约束角色采用 trait 橙色，用户已认可 |
| function/method | `#146DAD`，不加粗；const 方法保持函数角色 |
| function.static | `#017A56B7`，不加粗；沿用 Rust 静态函数的浅色思路 |
| variable（含普通 const 与只读指针/引用） | `#A57800`，不加粗、不斜体；含 lambda 捕获保留原变量角色 |
| variable.static | `#017A56B7`，不加粗；只读性本身不再触发此色 |
| parameter/property（含 const 参数与实例字段） | `#499504`，不加粗、斜体；保留各自角色 |
| C++ 可变性下划线 | 用户已选择不添加；保留类别颜色和既有斜体，Rust 下划线保持原样 |

enumMember、macro、数字、字符串与注释沿用现有共享规则。C++ TextMate 中新增函数/预处理参数的斜体兜底，将已有字段访问规则改为浅绿斜体，this 改为深色斜体。字段组移除对象接收者 scope，避免将接收对象当成字段；实际对象分类仍以 clangd 为准。没有新语法注入。

模板 T/N、类型别名、concept 橙色与抽象类绿色，以及 lambda 参数/捕获和结构化绑定，已获用户明确认可。cpp-r2 已按用户要求将 readonly 字段恢复浅绿斜体；用户反馈“目前看起来还行”，记录为本轮已观察效果获认可，不扩大到未逐项检查的类别。常量色沿用已认可的 alpha 色值，不声称透明文字兼容性限制已解决。

## 证据与边界

首轮以本机 clangd 23.1.0 实际采集得到 363 个 token，最后一轮诊断为空。44 个角色样本经过规则优先级与 TextMate 回退推导验证；30 个语义 token 的最终样式改变。全部原有语义规则保持原值，新增规则都限定 `:cpp`；9 个 Rust 文件的 6,653 个 TextMate token 样式完全一致。UI 颜色也保持原样，包括已认可的暖米色选区。

当前为 94 个界面颜色键、43 条语义规则、59 条 TextMate 规则、2 条原有 Rust 语法注入。JSONC 与颜色/样式检查通过；新增 concept 类型与 readonly/static 修饰符使用实际 clangd legend 校验，不能只用 rust-analyzer 的 token 集合判断是否支持。首轮证据见 [cpp-clangd-audit.json](cpp-clangd-audit.json)，cpp-r2 两规则移除及 26 个目标前景变化见 [readonly 修复审计](cpp-readonly-fix-audit.json)。

服务器的 readonly 也可能表示指向只读对象，并非“变量本身不可重新赋值”。文件作用域的 static 变量在此样本中没有 static 标记；普通宏与函数式宏同为 macro，宏替换体的参数名识别也有缺口；constexpr/consteval 函数未单独标注。以上保留实际分类，不用扩大选择器或按名称猜测来填补。公司的 clangd 版本未知，尚未验证 MS C++ 扩展、C、CUDA、模块、完整标准库和复杂依赖名。

依据：[clangd 语义高亮类别和修饰符](https://clangd.llvm.org/features#semantic-highlighting)、[VS Code 语义主题规则](https://code.visualstudio.com/api/language-extensions/semantic-highlight-guide#theming)。

## 预览与安装

- 开发窗口：主题仓库的 Run and Debug 选择 **Preview C++ Theme**，F5 打开兄弟目录的样例。
- 普通窗口：安装 `sl-rust-0.0.1-local-preview-20261107-cpp-r3.vsix`，重载，打开 C++ 样例项目，等待 clangd 就绪。
- 主题仍名为 **Solarized Light Rust (Local Preview)**，扩展 ID、版本均保留；未发布。

优先观察类型/枚举、常量/静态成员、参数/字段三组，再继续 C++ 独有类别。不将编译和规则检查等同于用户视觉验收。

## 样例 C++20 配置修复（2026-11-07）

用户截图显示 `constinit` 被报为未知类型。编辑器日志确认 clangd 使用 `build/compile_commands.json`，而非根目录的 `compile_flags.txt`。本机 GCC 16 默认 C++20，原先仅使用 `cxx_std_20` 最低特性要求时，CMake 省略了 `-std=`；clangd 因自身默认标准较旧而误报。此前直接编译和 compile_flags 模式的检查没有覆盖这个数据库切换后的状态。

已为样例目标设置 `CXX_STANDARD 20`、`CXX_STANDARD_REQUIRED YES`、`CXX_EXTENSIONS NO`，重新配置和构建原 build 目录；生成命令已包含 `-std=c++20`。新的实际 LSP 采集诊断为空，363 个语义 token 与最初正确 C++20 环境一致。主题和 VSIX 不变，只需在现有窗口执行 clangd: Restart language server。证据见 [构建配置审计](cpp-build-configuration-audit.json)，其中也单独记录 clangd 全功能自检的宏重构失败，不把它误报为源码错误或全部检查通过。

依据：[CMake 标准特性与省略标准参数的规则](https://cmake.org/cmake/help/latest/manual/cmake-compile-features.7.html#requiring-language-standards)。

## readonly 范围收窄（2026-11-07，已实施）

用户指出普通 C++ const 局部变量更接近 Rust 的不可变 let，不应因为 readonly 自动使用 const/static 色，并明确授权按收窄方案修改。已仅删除 `variable.readonly:cpp` 与 `property.readonly:cpp` 两条覆盖。普通 const 变量、只读指针/引用恢复普通变量色，const 实例字段和参数保留浅绿斜体。静态项、非类型模板参数仍保留原浅青绿；不增加可变性下划线。

constexpr 对象的精确识别按用户要求暂缓。样例 const fixed 与 constexpr extent 的 token 均为 variable.declaration.definition.readonly.functionScope，命名空间 LIMIT 与 DEFAULT_STEP 也同为 readonly.fileScope，因此这四个名称均恢复变量色；static constexpr capacity 仍可通过 static 标记保留浅青绿。没有用名称、作用域或大写习惯猜测 constexpr。初始原因与 token 证据见 [readonly 复核](cpp-readonly-review.json)。

配置精确差异仅两条语义规则删除，363 个已有实际 token 的样式推导对比中，26 个目标前景变化；其余 token、所有字体样式、UI、TextMate 和 Rust 规则不变。源码哈希匹配此前采集，本轮无需重复编译或启动语言服务。主题配置检查通过，仍保留已知透明度限制；用户随后反馈“目前看起来还行”，保留已观察的修改后效果。

观察 main.cpp:104–112 的 fixed/extent/只读指针引用，均应为普通变量色；:30 的 origin 应浅绿斜体。对照 :32/33 的 instances/capacity、:55 的 N、:106 的 local_visits 仍为浅青绿。使用新的 cpp-r2 包重装并重载即可。

语义依据：[Rust 不可变变量与常量的区别](https://doc.rust-lang.org/book/ch03-01-variables-and-mutability.html)、[C++ const 限定](https://eel.is/c++draft/dcl.type.cv)、[constexpr 对象](https://eel.is/c++draft/dcl.constexpr)。

## 宏与属性（用户反馈可继续）

2026-11-07，用户回复“okay，接下来呢”。保留本组当前显示；宏替换体参数的识别缺口仍未修复，未新增语法补丁。

- main.cpp:4/5/147：THEME_LIMIT 和 THEME_ADD 的定义与调用均使用函数蓝色；宏参数列表中的 left/right 为浅绿斜体。替换体中的同名参数目前仍为普通变量色，属于已记录的识别缺口，本轮未补语法。
- main.cpp:96/97：nodiscard、maybe_unused 当前通过 TextMate 显示普通深灰 #333333、不加粗；保留 token 中没有对应语义标记。先观察可读性，尚未改成 Rust 内置属性的关键字色。

宏与属性本组未改色；最新布尔值改色使用上方 cpp-r3 包。

## 构造/析构、this、运算符与特殊函数（用户已确认）

2026-11-07，用户回复“都没问题”，保留本组当前配色与样式，包含转换函数中 bool 的普通深灰。确认范围限于上一轮提供的观察组。

- main.cpp:35/36：构造和析构中的 Counter 名称为类型绿色加粗；:40 的 this 为深色斜体，成员 value 为浅绿斜体。
- main.cpp:90/91/126：operator 和重载的 + 保留深色、不加下划线；other 和字段 value 为浅绿斜体。转换函数 operator bool 中的 bool 在当前 TextMate 中为普通深灰 #333333（entity.name.operator.type.cpp），并非普通类型位置的绿色加粗，用户已反馈本组没有问题，保留当前样式。
- main.cpp:44/49–52：add、square、fixed_size 为函数蓝，static create/internal 为浅青绿。constexpr/consteval 函数没有独立标记，保持已有函数色，不声称已经按编译期执行能力区分。

上述技术核对来自与当前源码哈希匹配的留存 clangd token、当前规则推导和 TextMate 分词；视觉反馈单独记录为用户本轮的“都没问题”。下一组检查字面量与注释。

## 字面量与行注释（用户已确认）

2026-11-07，用户回复“这一组没问题了，接下来呢”。记录本组已观察显示通过，包含 cpp-r3 中 true/false 的玫红；不包含随后新增的块注释和标准库样例。

- main.cpp:129/130：普通和原始字符串的主体为 #258F94；普通字符串的转义序列为浅青绿 #017A56B7。原始字符串内部的反斜杠和引号不作为普通转义改色，结束后代码应恢复正常。
- main.cpp:131/132：普通字符沿用字符串色，字符转义采用浅青绿。
- main.cpp:133/134：数字主体为玫红 #D33682，但当前语法将指数 e/-、十六进制前缀 0x 和后缀 u 分为关键字/运算符，实际为深色 #073642；并非整个字面量同色。
- main.cpp:135/136：true/false 在 cpp-r2 实际为金黄 #B58900，先前样例表格曾误写为玫红。用户随后要求与 Rust 一致，cpp-r3 已为两个 C++ 专用 scope 设置 #D33682，不加粗、不斜体；无对应 clangd 语义 token，用户已确认本组显示没有问题。
- main.cpp:95/128：文档行注释与普通行注释均为灰色 #93A1A1；此样例没有块注释，不能据此宣称块注释或全部文档语法已检查。

字面量与注释的初次核对来自哈希匹配的留存源码；随后布尔值改色另做真实 grammar 前后对比，见下方记录。

## C++ 布尔值与 Rust 同色（2026-11-07，已实施）

用户明确要求 true/false 与 Rust 一致，已将 constant.language.true.cpp、constant.language.false.cpp 加入现有 constant.numeric 的玫红 #D33682 规则。保留通用 constant.language 规则，不波及其他常量、预处理器或 bool 类型名。规则数量仍为 94 个 UI 键、43 条语义规则、59 条 TextMate 规则。

真实 TextMate 比较检查 1,600 个 C++ token，仅两个布尔值前景变化，字体均不变；9 个 Rust 文件的 6,653 个 token 一致。未修改样例源码，留存 clangd 数据中这两个字面量没有语义覆盖。配置检查通过，已知透明度限制保持原样。证据见 [布尔值审计](cpp-boolean-color-audit.json)。

安装 cpp-r3 后重载，观察 main.cpp:135/136 的 true/false 是否与旁边数字主体同为玫红；不需重查其他已确认组。用户已确认本组显示没有问题；发布和代安装均未执行。

## 标准库、跨文件引用与预处理器（用户已确认）

2026-11-07，用户回复“这一组没问题了，接下来呢”，本组已观察的标准库、跨文件引用、预处理器与块注释显示获认可。未将泛指反馈扩展为公司工具链或所有标准库的验收。

原 main.cpp 和已确认配色保持原样。新增 standard_library.cpp（53 行）及 library_types.hpp（20 行），独立目标 cpp_theme_library_test 与原目标均显式使用 C++20。继续用 cpp-r3 包，无需重新打包或安装。

- standard_library.cpp:19–29：vector、optional、string、Entry/EntryList 与 size_type 按类型名绿色加粗；push_back、make_unique、describe、size 按函数蓝色。
- standard_library.cpp:32–39：迭代器调用和 sort 为函数蓝；回调参数 left/right、字段 count 为浅绿斜体。
- library_types.hpp:8/18 与 standard_library.cpp:12/28：对照头文件和实现/调用中的 Entry、describe，检查颜色一致。头文件文档块注释和 @param/@return 标签观察可读性与闭合。
- standard_library.cpp:1–8/41–52：观察 include 路径、宏、if/else 分支及块注释；未启用分支的显示包含在本组用户反馈中；LSP 采集本身仍未验证淡化的具体颜色或程度。

构建及新增程序运行成功；clangd 23.1.0 对源文件和头文件分别产生 96/11 个语义 token，诊断均为空，真实 TextMate 分词共 458 个 token。技术检查不代替用户视觉验收，证据见 [补充样例审计](cpp-library-sample-audit.json)。公司的工具链、第三方库和其他未列出的语法仍未覆盖。

## 后续建议与保留事项

现有样例的常用配色首轮可以收尾，继续使用已确认的 cpp-r3 包。下一步建议在公司实际项目中检查一两个常用 .cpp/.hpp 文件，确认所用 clangd 版本，并关注项目宏、第三方库类型/函数、复杂模板与跨文件显示的一致性。实际项目试用尚未执行，也不要求提供公司源码。

- C++ 可变性下划线：已确认不添加，cpp-r4 试验撤回，继续使用 cpp-r3；此项已定案，不再列为未试验。Rust 可变性下划线保留。
- constexpr 变量与普通 const 的精确区分：用户明确暂缓；现有语言服务分类仍相同。
- 宏替换体 left/right 等参数：尚未按参数识别，未新增 C++ 语法补丁。
- 文件作用域 static 等 clangd 分类边界、公司版本和第三方库差异：仍保留，不据样例检查宣称全部兼容。

以上是待办及建议；未改主题、源码、安装包或发布状态。

## C++ 下划线试用（2026-11-07，历史试验，已撤回）

用户以“那就这样吧”接受助手关于 Rust 保留、C++ 不加下划线的建议。下文仅保存 cpp-r4 的试验方案和验证证据；当前主题与推荐包均恢复 cpp-r3，不再等待本试验的视觉反馈。

用户明确要求加下划线试用。实际 clangd legend 提供 readonly，没有 Rust 的 mutable 修饰符。本版仅针对 variable/parameter/property 默认叠加下划线，再用三条 readonly 规则取消；不改前景、加粗、斜体或删除线。variable.readonly:cpp 和 property.readonly:cpp 的名称虽重新出现，但仅设置 underline:false，没有恢复 cpp-r2 移除的浅青绿覆盖。

这是一版按符号 readonly 分类近似可变性的试用，不是“本行发生写入”的提示。指针 pointer_to_const 与 const_pointer 都带 readonly，所以都不加线；字段及捕获按 clangd 分类，不表示完整表达式 const 分析。非 readonly 静态变量保留浅青绿并加线，static constexpr capacity 保持浅青绿但无下划线。未提供相应语义类别的宏替换体参数等不加新线。

三个实际样例共 470 个留存语义 token 与现有源码哈希一致，149 处只增加下划线：main.cpp 124 处，standard_library.cpp 23 处，library_types.hpp 2 处。所有颜色/加粗/斜体/删除线不变；函数、类型、宏、运算符均不加线。TextMate、UI、Rust 和其他语言规则逐项保持一致；配置检查通过。证据见 [下划线审计](cpp-underline-audit.json)，不把规则推导视为用户界面验收。

安装 cpp-r4 后重载，观察：

- main.cpp:103/104：local 有线、fixed 无线；原变量色保持一致。
- main.cpp:49：left 浅绿斜体加下划线，const 参数 right 保留浅绿斜体、不加线。
- main.cpp:29–33：value/cached/instances 有线，origin/capacity 无线；字段和静态项颜色保持原样。
- main.cpp:141/142：pointer/reference 有线，+= 运算符无下划线。

重点评价长函数里下划线的密度是否舒服。cpp-r3 旧包保留，可用原安装方式加 --force 回退。尚未发布或代安装。

依据：[clangd 语义类别与修饰符](https://clangd.llvm.org/features#semantic-highlighting)、[VS Code 语义样式支持](https://code.visualstudio.com/api/language-extensions/semantic-highlight-guide#theming)。

### 最终决定与回退验证

2026-11-07，用户确认 C++ 不加可变性下划线，Rust 保留。主题已从校验过 SHA-256 的 cpp-r3 包恢复，文件与该包逐字节一致（cc9394a152c0107d7170d3c6525c084f180720f373e35a879df6be06ed4040d6），并再次通过配置检查；三处新增 underline:true 与三条 readonly 排除规则均撤回，前景/加粗/斜体、UI 和 Rust 规则回到试验前状态。无需重新打包。

若已经安装 cpp-r4，在 Windows 用原安装命令加 --force 重装 Downloads 中的 cpp-r3，再执行 Developer: Reload Window。若仍在使用 cpp-r3，无需操作。cpp-r4 仅保留为历史试验产物，不是当前推荐版本；未代安装、发布或提交。
