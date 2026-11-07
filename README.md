# software-testing-router

按验收目标与软件实际可观察能力，选择 CLI/API、DOM、Windows UIA、Qt 内部测试或视觉证据。附带具有硬超时和进度日志的 Windows UIA 助手。

Choose the test interface that can prove the required behavior: CLI/API, DOM, Windows UI Automation, Qt tests, or visual evidence.

## 安装

当前版本：**1.0.1**。

```sh
npx skills add https://github.com/hunxuankai/software-testing-router/tree/v1.0.1/skills/software-testing-router --skill software-testing-router
```

也可从 [GitHub Release](https://github.com/hunxuankai/software-testing-router/releases/tag/v1.0.1) 下载 `software-testing-router-1.0.1.zip`，核对 `.sha256` 后，将其中的 `software-testing-router/` 完整目录放到所用代理支持的技能目录。

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

本次发布加入 MIT 许可，并对三份历史证据中的本机目录做公开脱敏。原始步骤、状态、时间与数值保持不变，具体字段和哈希见 [脱敏说明](https://github.com/hunxuankai/software-testing-router/blob/main/skills/software-testing-router/references/evidence/public-redactions-1.0.1.md)。四个运行脚本保持与本地原版相同的字节。

[验证记录](https://github.com/hunxuankai/software-testing-router/blob/main/skills/software-testing-router/references/validation.md) 说明实际样例、历史失败及恢复、环境与未覆盖范围。这些结果不保证任意生产软件适用，也没有证明相对 Computer Use 的通用加速倍数。

本仓库是从唯一维护源码生成的公开快照；本地旧历史及未脱敏原件不进入公开历史。`SOURCE.json` 记录维护来源与技能 tree，Release 的 `.commit.txt` 指向本仓库中可获取的公开来源提交，`.sha256` 对应实际独立 ZIP。

## 许可

[MIT License](https://github.com/hunxuankai/software-testing-router/blob/main/LICENSE) · Copyright (c) 2026 hunxuankai。允许使用、复制、修改、分发和商业使用；分发时保留版权及许可声明。完整许可也包含在每个技能包的 `references/license.txt`。
