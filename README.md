# software-testing-router

按验收目标与软件实际可观察能力，选择 CLI/API、DOM、Windows UIA、Qt 内部测试或视觉证据。附带具有硬超时和进度日志的 Windows UIA 助手。

Choose the test interface that can prove the required behavior: CLI/API, DOM, Windows UI Automation, Qt tests, or visual evidence.

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
| Qt（C++） | C++ | 可用 UIA；项目已有 Qt Test | **有条件可用，未实测**；Python 的 pytest-qt 路线不能直接代替 C++ Qt 内部测试 |
| 普通网页；React / Vue / Angular 等 | JavaScript / TypeScript 等 | Playwright、Cypress、WebDriver 等 DOM 工具 | **已实测普通 HTML/JavaScript + Chromium**；DOM 路线不依赖前端框架，各框架应用未分别实测 |
| Electron / CEF / WebView2 | JavaScript / TypeScript、C#、C++ 等 | 已可用且获授权的 DOM/CDP 连接；原生对话框可探测 UIA | **有条件可用，未实测真实宿主**；普通 Chromium 通过不能代替宿主兼容验证 |
| Tkinter / wxPython；Swing / JavaFX 等 | Python / Java 等 | 项目已有测试接口；实际可访问性能力决定其他入口 | **未提供专用驱动、未实测**；需在目标项目先探测和验证后扩展 |

UIA 助手当前实现 `set`、`invoke`、`wait`，并提供唯一控件定位、父区域与窗口范围、进程身份校验、硬超时和进度日志。快捷键、拖拽、复杂选择操作及自绘 Canvas/WebGL 对象需要相应的项目驱动或其他可观察入口。

### 平台与证据

- **实测环境：** Windows 10 x64、PowerShell 7.6.9、Python 3.11.13、PySide6 / Qt 6.12.0、Python Playwright + Chromium。Windows 11、其他浏览器和完整打包流程未在本包实测。
- **其他平台：** CLI/HTTP、网页 DOM 或 Python Qt 内部测试可按项目环境选择；本包未提供 macOS 原生、Linux 桌面或 Android/iOS 的专用 GUI 驱动，这些平台也未获得本包的执行验证。
- **证据范围：** 2026-11-05 初版校准为 19 项通过，优化版为 37 项通过，均无失败或跳过；这些结果覆盖记录中的样例，不代表所有列出的框架或所有生产应用都通过验证。

详细范围见 [实测记录](https://github.com/hunxuankai/software-testing-router/blob/main/skills/software-testing-router/references/validation.md)、[Windows UIA](https://github.com/hunxuankai/software-testing-router/blob/main/skills/software-testing-router/references/windows-uia.md)、[Python Qt](https://github.com/hunxuankai/software-testing-router/blob/main/skills/software-testing-router/references/qt.md)、[网页 DOM](https://github.com/hunxuankai/software-testing-router/blob/main/skills/software-testing-router/references/web.md) 和 [扩展方法](https://github.com/hunxuankai/software-testing-router/blob/main/skills/software-testing-router/references/extending.md)。pytest-qt 的绑定支持以其 [官方说明](https://pytest-qt.readthedocs.io/en/latest/intro.html#requirements) 为准。

## 安装

当前版本：**1.0.2**。

```sh
npx skills add https://github.com/hunxuankai/software-testing-router/tree/v1.0.2/skills/software-testing-router --skill software-testing-router
```

也可从 [GitHub Release](https://github.com/hunxuankai/software-testing-router/releases/tag/v1.0.2) 下载 `software-testing-router-1.0.2.zip`，核对 `.sha256` 后，将其中的 `software-testing-router/` 完整目录放到所用代理支持的技能目录。

独立技能位于 `skills/software-testing-router/`。安装时保留该目录下的全部文件；GitHub 自动生成的整仓 Source code ZIP 与 Release 的独立技能附件范围不同。

## 使用

```text
使用 software-testing-router 为当前软件选择合适的自动化测试方法，
先说明验收要求、实际发现的入口和成功证据，再执行必要验证。
```

有 CLI/API 时检查业务结果与产物；需要证明按钮或表单有效时使用实际 DOM/UIA 交互；Qt 源码行为可用事件循环和信号检查；布局、裁切与外观需要视觉证据。入口详见 [SKILL.md](https://github.com/hunxuankai/software-testing-router/blob/main/skills/software-testing-router/SKILL.md)。

## 运行要求

Windows UIA 助手需要 Windows、Python 3.10+ 标准库、PowerShell 7 和可交互桌面。它核对明确的 PID 与可执行文件，支持限定窗口和控件范围，以及 `set`、`invoke`、`wait` 步骤。硬超时后保留 JSONL 进度，结果未知的动作不会自动重放。

其他测试路线按项目已有工具及技能参考页准备依赖。浏览器、Qt、pytest 与维护解析依赖不随技能包安装，也不是 UIA 助手的 Python 运行依赖。

## 版本与证据

1.0.1 加入 MIT 许可，并对三份历史证据中的本机目录做公开脱敏。1.0.2 将两份纯文本日志改名为 .txt，以保证 GitHub 导入时保留完整证据；日志字节不变。原始步骤、状态、时间与数值保持不变，具体字段和哈希见 [脱敏说明](https://github.com/hunxuankai/software-testing-router/blob/main/skills/software-testing-router/references/evidence/public-redactions-1.0.1.md)。四个运行脚本保持与本地原版相同的字节。

[验证记录](https://github.com/hunxuankai/software-testing-router/blob/main/skills/software-testing-router/references/validation.md) 说明实际样例、历史失败及恢复、环境与未覆盖范围。这些结果不保证任意生产软件适用，也没有证明相对 Computer Use 的通用加速倍数。

本仓库是从唯一维护源码生成的公开快照；本地旧历史及未脱敏原件不进入公开历史。`SOURCE.json` 记录维护来源与技能 tree，Release 的 `.commit.txt` 指向本仓库中可获取的公开来源提交，`.sha256` 对应实际独立 ZIP。

## 许可

[MIT License](https://github.com/hunxuankai/software-testing-router/blob/main/LICENSE) · Copyright (c) 2026 hunxuankai。允许使用、复制、修改、分发和商业使用；分发时保留版权及许可声明。完整许可也包含在每个技能包的 `references/license.txt`。
