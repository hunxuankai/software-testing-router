# software-testing-router

当前版本：**1.1.0**。按验收要求选择 CLI/API、DOM、Windows UIA、macOS Accessibility、Qt 内部测试或视觉证据。

Choose the interface that proves the required behavior: CLI/API, DOM, Windows UI Automation, macOS Accessibility, Qt tests, or visual evidence.

## 操作系统与能力

系统是否具备入口与本包是否实测分别列出。CLI/API、DOM、Python Qt 依赖目标提供相应接口及匹配环境；原生 GUI 另受系统可访问性与权限限制。

| 系统 | CLI/API | 网页 DOM | Python Qt 内部测试 | 本包原生 GUI 驱动 | 验证状态 |
|---|---|---|---|---|---|
| Windows 10 x64 | 有入口时可用 | 有匹配浏览器时可用 | 有源码和匹配绑定时可用 | UIA，Python + PowerShell 7 | **已实测**下述具体样例与记录环境 |
| Windows 11 | 条件可用 | 条件可用 | 条件可用 | 同一 Windows UIA 实现 | **未实测**，不能由 Windows 10 外推 |
| macOS，Intel / Apple Silicon | 条件可用 | 条件可用 | 条件可用 | 新增 AX 原生实现；64 位 Python、已有辅助功能授权、交互桌面 | **已实现，未实机验证**；本次没有 Mac 硬件，ABI、权限与 GUI 兼容性未核验 |
| Linux | 条件可用 | 条件可用 | 条件可用 | **没有本包原生 GUI 驱动** | 可移植路线说明与安装实现已提供，**未在 Linux 执行** |

Mac 只验收正式 CLI 的业务结果时仍走 CLI/API；需要真实按钮证据时才采用 [macOS 原生路线](skills/software-testing-router/references/macos-accessibility.md)。移动平台没有本包专用 GUI 驱动。

## 适用语言

下表中的语言指**被测软件的开发语言**。选型依据是软件实际提供的 CLI、HTTP API、DOM 或可访问控件；UIA 助手用 Python 和 PowerShell 实现，不要求目标 EXE 也用 Python 编写。

| 被测软件语言 | 可采用的测试路线 | 本包的实际验证情况 |
| --- | --- | --- |
| Python | CLI/HTTP；PySide6、PyQt5、PyQt6 的 Qt 内部测试；Windows 桌面可探测 UIA | 已实测 Python CLI/HTTP、PySide6 控件/信号/取消，以及 PySide6 的 Windows UIA 样例 |
| JavaScript / TypeScript | 网页 DOM 自动化；Electron 等嵌入式网页需具备可用调试入口 | 已实测普通 HTML/JavaScript 页面与 Python Playwright + Chromium；TypeScript 项目和各前端框架未分别实测 |
| C# / .NET | CLI/HTTP；WinForms、WPF 等 Windows 桌面的 UIA | 已实测 C# / .NET Framework 4.x WinForms 编译后的独立 EXE；WPF 未实测 |
| C / C++ | CLI/HTTP；Windows 原生界面或 Qt 的可访问控件可探测 UIA；已有 Qt Test 由项目提供 | 未独立实测 C/C++ 应用，未提供 C++ Qt 专用内部测试驱动 |
| Java、Go、Rust 等其他语言 | 有正式 CLI/HTTP 或项目已有测试命令时可复用通用路线；GUI 另看框架与可访问性入口 | 未按这些语言分别实测，未提供它们的专用 GUI 驱动 |

## GUI 框架与网页支持

**已实测**指下面列出的具体环境和样例；**有条件可用**表示需先确认目标提供所需入口，不能按框架名称直接判定测试已经可运行。

| 框架 / 界面类型 | 常见语言 | 推荐入口 | 覆盖与验证状态 |
| --- | --- | --- | --- |
| WinForms | C# / .NET | Windows UIA | **已实测** .NET Framework 4.x 独立 EXE 的文本框、按钮、状态等待、窗口/进程探测及超时与日志恢复样例 |
| WPF | C# / .NET | Windows UIA | **有条件可用，未实测**；目标控件需暴露所需 UIA Pattern |
| Win32 / MFC | C / C++ 等 | Windows UIA | **有条件可用，未实测**；原生或自绘控件的可访问性支持需实际探测 |
| PySide6 / Qt 6 | Python | pytest-qt 内部事件循环；Windows UIA | **已实测** PySide6 / Qt 6.12.0 的控件、异步信号、取消与 UIA 样例；Qt 样例由解释器启动，未验证 PyInstaller 打包 EXE |
| PyQt5 / PyQt6 | Python | pytest-qt；Windows UIA 需探测 | **有条件可用，未实测这些绑定**；使用项目已有 Qt 绑定与对应环境 |
| Qt（C++） | C++ | 按系统探测 UIA/AX；项目已有 Qt Test | **有条件可用，未实测**；Python 的 pytest-qt 路线不能直接代替 C++ Qt 内部测试 |
| 普通网页；React / Vue / Angular 等 | JavaScript / TypeScript 等 | Playwright、Cypress、WebDriver 等 DOM 工具 | **已实测普通 HTML/JavaScript + Chromium**；DOM 路线不依赖前端框架，各框架应用未分别实测 |
| Electron / CEF / WebView2 | JavaScript / TypeScript、C#、C++ 等 | 已可用且获授权的 DOM/CDP 连接；按系统探测原生 UIA/AX | **有条件可用，未实测真实宿主**；普通 Chromium 通过不能代替宿主兼容验证 |
| macOS AppKit 等原生窗口 | Swift / Objective-C / 其他语言 | AX 属性及 AXPress/AXValue | **新增实现，未实机验证**；有标识、唯一目标和所需动作时可探测，不按框架名推断可运行 |
| Tkinter / wxPython；Swing / JavaFX 等 | Python / Java 等 | 项目已有测试接口；实际可访问性能力决定其他入口 | **未提供专用驱动、未实测**；需在目标项目先探测和验证后扩展 |

UIA/AX 助手实现 `set`、`invoke`、`wait`，并提供唯一控件定位、父区域与窗口范围、进程身份校验、硬超时和进度日志。`scripts/run_desktop.py` 按系统选择入口；既有 Windows `scripts/run_uia.py` 和 Windows 步骤格式保持可用，Mac 使用原始 AX selector。快捷键、拖拽、复杂选择操作及自绘 Canvas/WebGL 对象需要相应的项目驱动或其他可观察入口。

### 平台与证据

- **实测环境：** Windows 10 x64、PowerShell 7.6.9、Python 3.11.13、PySide6 / Qt 6.12.0、Python Playwright + Chromium。Windows 11、其他浏览器和完整打包流程未在本包实测。
- **其他平台：** 新增 Mac 原生驱动没有实机证据；CLI/HTTP、DOM 或 Python Qt 内部路线按目标环境选择。Linux 没有本包原生 GUI 驱动，Mac/Linux 的可移植路线也没有本次平台执行结果。
- **证据范围：** 2026-11-05 初版校准为 19 项通过，优化版为 37 项通过，均无失败或跳过；这些结果覆盖记录中的样例，不代表所有列出的框架或所有生产应用都通过验证。
- **2026-11-07 最终整合验证：** Windows 10 源码全套 **339 通过、7 跳过、0 失败/错误，191.28 秒**；受控安装包运行子集 **193 通过、0 跳过，18.28 秒**。跳过为 3 项 Mac 原生、2 项原生 POSIX 大小写不敏感文件系统、2 项 OS symlink 权限检查。受控更新精确备份旧副本，安装包为 40 文件/48 本地引用。前一轮 324 通过的整合记录、候选归档清单核对及后续源缓存污染失败/恢复均保留，详见 [验证记录](skills/software-testing-router/references/validation.md)。

整合结果包含实际既有 Windows GUI 路线，以及 Windows 宿主上的 Mac 函数边界、策略和子进程检查；Mac ABI、TCC、Swift GUI 与真实 Mac 文件系统仍未实测。开发中的 fixture 失败和后续边界回归均保留失败/恢复记录，最终通过不覆盖旧失败。

详细范围见 [实测记录](skills/software-testing-router/references/validation.md)、[Windows UIA](skills/software-testing-router/references/windows-uia.md)、[macOS AX](skills/software-testing-router/references/macos-accessibility.md)、[Python Qt](skills/software-testing-router/references/qt.md)、[网页 DOM](skills/software-testing-router/references/web.md) 和 [扩展方法](skills/software-testing-router/references/extending.md)。pytest-qt 的绑定支持以其 [官方说明](https://pytest-qt.readthedocs.io/en/latest/intro.html#requirements) 为准。

## 安装

```sh
npx skills add https://github.com/hunxuankai/software-testing-router/tree/v1.1.0/skills/software-testing-router --skill software-testing-router
```

也可从 [GitHub Release](https://github.com/hunxuankai/software-testing-router/releases/tag/v1.1.0) 下载 `software-testing-router-1.1.0.zip`，核对 `.sha256` 后，将外层 `software-testing-router/` 完整目录安装到所用代理支持的技能目录。保留全部 scripts、references 和 agents 文件。GitHub 自动生成的整仓 Source code ZIP 与独立技能附件范围不同。

## 运行要求与使用

Windows 原生 UIA 需要 Windows、Python 3.10+ 标准库、PowerShell 7 与可交互桌面。Mac 原生 AX 需要 macOS、64 位 Python 3.10+、已有辅助功能授权和交互桌面；不需要 PowerShell、PyObjC、Swift 或 Xcode。Swift 编译器仅是维护验收样例的依赖。

从独立技能目录检查入口：

```sh
python3 scripts/run_desktop.py --help
```

Windows 可使用 `python scripts/run_desktop.py --help`；原 `run_uia.py` 入口和步骤格式保持兼容。`run_desktop.py` 根据本机系统选择 Windows 或 Mac 驱动，Linux 没有本包原生 GUI 驱动。帮助成功只证明入口加载。

需要原生界面证据时先核对实际 PID、可执行文件和窗口，再 inspect 真实控件和所需动作。使用 `set`、`invoke`、`wait` 步骤，重新读取本次产物并独立断言。Mac 使用原始 AX 字段，不能照搬 Windows 的选择器。

完整方法见 [SKILL.md](skills/software-testing-router/SKILL.md)、[Windows UIA](skills/software-testing-router/references/windows-uia.md)、[macOS AX](skills/software-testing-router/references/macos-accessibility.md) 和 [维护边界](skills/software-testing-router/references/maintenance.md)。浏览器、Qt、pytest 与维护解析依赖按项目准备，不随本技能的标准库运行助手安装。

## 本版变化与证据

新增独立 Mac 系统 API 适配器、步骤执行器与按系统分发入口。原 Windows 的四个运行脚本及历史证据字节保留。执行器核对进程身份、窗口与祖先范围、唯一目标和完整计划，保留硬超时、UTF-8 输入、原子报告及动作日志；未知动作不会自动重放。

维护仓库另提供统一 Python 安装器，Windows PowerShell 包装器保持兼容；这些维护工具不属于独立运行包。

2026-11-07 Windows 10 完整回归 **339 通过、7 跳过**；受控安装运行子集 **193 通过**。跳过为 3 项 Mac 原生、2 项 POSIX 文件系统、2 项 Windows 符号链接权限检查。Mac 实际 ABI、TCC、Swift/AppKit GUI 和真实 Mac 文件系统仍 **未实机验证**。失败与恢复记录见 [验证摘要](skills/software-testing-router/references/validation.md)；替身、帮助和跳过均不算 Mac 原生通过。

旧版 1.0.1 加入 MIT 许可与公开证据脱敏，1.0.2 将两份日志改名为 .txt 并保持内容字节。旧标签、旧发行包和原始维护历史保留。脱敏范围见 [历史说明](skills/software-testing-router/references/evidence/public-redactions-1.0.1.md)。

本仓库是唯一维护源码的经校验公开快照；不包含私人维护历史。`SOURCE.json` 记录来源和技能 tree，Release `.commit.txt` 指向本公开仓库的发行提交，`.sha256` 对应实际独立 ZIP。

## 许可

[MIT License](LICENSE) · Copyright (c) 2026 hunxuankai。允许使用、修改、分发和商业使用，分发时保留版权及许可声明。包内完整许可位于 `references/license.txt`。
