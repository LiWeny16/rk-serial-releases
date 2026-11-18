<p align="center">
  <img src="assets/logo.png" alt="rk-serial logo" width="96">
</p>

<h1 align="center">rk-serial · 串口调试助手</h1>

<p align="center">面向人与 AI Agent 协作的桌面串口调试工具<br>Desktop serial debugging for engineers and AI agents</p>

<p align="center">
  <a href="https://github.com/LiWeny16/rk-serial-releases/releases/latest">下载最新版 · Latest release</a> ·
  <a href="#解决了什么痛点">简体中文</a> ·
  <a href="#what-rk-serial-solves">English</a>
</p>

## 解决了什么痛点

rk-serial 来自 Rockchip 同事在板卡开发与联调中的实际需求。它把工程师和 Agent 接入同一个可观察的串口工作台，解决三个常见问题。

- **多个 Agent 调试同一个串口，命令容易交叉。** 多个 Agent 可以同时读取日志，各自保留读取位置。需要控制串口时，按端口排队，每次只授予一个控制者。多步调试可以在同一次控制权内完成。
- **工程师和 Agent 同时操作，容易抢占或误发。** GUI 统一持有串口。Agent 占用时，人工发送会收到端口忙提示，可以在控制权释放后继续操作。不同串口独立协调。
- **Agent 接管后，工程师难以知道发生了什么。** 人和 Agent 共用同一个可见终端，工程师可以继续查看收发记录和日志。「Agent 连接」还能查看已登记的调试任务、当前控制者和等待状态。

![rk-serial 主工作台，包含串口标签、交互终端、发送区与常用命令](assets/workspace.png)

## 日常调试能力

| 场景 | rk-serial 的做法 |
| --- | --- |
| **在 Linux 和 RT-Thread（RTT）之间切换** | 按串口识别系统类型，也支持手动标记。命令大全提供 Linux、RT-Thread 和通用命令，可搜索用法、一键加入共享快捷列表并拖动排序。 |
| **快速发送和重复测试** | 从发送历史联想命令，支持快捷命令、定时循环发送、HEX 和可配置行尾。多个串口各自保留发送草稿。 |
| **录制串口和复盘** | 使用 **串口 Record** 按串口录制，用时长或容量控制任务，也能按设定循环发送。录制可在 GUI 查看，支持检索和导出，便于复盘偶发问题。 |
| **让 Agent 执行完整调试流程** | 内置 Agent bridge 覆盖串口配置与连接、读写、快捷命令、录制、归档检索和导出、批处理与验收用例。Agent 与工程师在同一 GUI 中操作和观察串口。 |

## What rk-serial solves

rk-serial grew out of board bring-up and debugging at Rockchip. It gives engineers and AI agents a shared desktop workspace for serial debugging.

- **Commands from multiple agents can interleave on one port.** Agents can read the same log concurrently with independent cursors. Control requests queue per port, so only one participant controls a port at a time. An agent can also hold control across a sequence of operations.
- **An engineer and an agent can compete for the same port.** The GUI owns the serial connection and reports when an agent is holding control. An engineer can resume sending once control is released. Each port is coordinated independently.
- **Agent activity can be difficult to observe.** Engineers can follow traffic and logs in the shared terminal during an agent task. The Agent Connection panel shows registered tasks, who holds control, and which requests are waiting.

## Debugging features

| Workflow | What you can do |
| --- | --- |
| **Move between Linux and RT-Thread (RTT)** | Detect or set the system type per port. Browse the Linux, RT-Thread, and general command catalog, then add and reorder shortcuts in a shared list. |
| **Send commands and repeat tests** | Get suggestions from send history and use quick commands, timed loops, HEX, or configurable line endings. Each port keeps its own draft. |
| **Capture traffic and investigate intermittent issues** | Start a per-port **Serial Record**, set time or capacity limits, and optionally run a command loop. Review, search, and export the recording for later analysis. |
| **Run a complete workflow with an agent** | The bundled Agent bridge supports port configuration, connections, reads and writes, quick commands, recording, archive search and export, batches, and reusable checks. Engineers and agents operate and observe serial ports through the same GUI. |

## 界面预览 · Screenshots

| Linux / RT-Thread 命令大全 · Command catalog | Agent 连接 · Agent Connection |
| :---: | :---: |
| <a href="assets/command-catalog.png"><img src="assets/command-catalog.png" alt="命令大全的系统筛选与快捷操作 / Command catalog with system filters and shortcut actions" width="420"></a> | <a href="assets/agent-connection.png"><img src="assets/agent-connection.png" alt="Agent 调试任务及 Skill 安装面板 / Registered agent tasks and skill installation" width="420"></a> |

<p align="center">
  <a href="assets/serial-record.png"><img src="assets/serial-record.png" alt="串口 Record 的录制设置 / Serial Record configuration" width="640"></a>
  <br>串口 Record · Serial Record
</p>

## 下载与安装 · Download and install

请按系统和架构选择安装包。Choose the package for your operating system and architecture. [GitHub Releases](https://github.com/LiWeny16/rk-serial-releases/releases/latest)

| 系统 · Platform | 文件 · Package |
| --- | --- |
| **Windows 10 / 11 · x64** | `*_x64-setup.exe`（推荐 / recommended）、`*_x64_en-US.msi`、`*-portable.zip` |
| **Ubuntu 22.04 / 24.04 / 26.04 LTS · amd64 / arm64** | `*_amd64.deb`、`*_arm64.deb`、`*_amd64.AppImage`、`*_arm64.AppImage` |

**中文** · Windows 绿色版解压后运行 `rk-serial-desktop.exe`，并将 `rk-serial-bridge.exe` 保留在同一目录。Windows 安装包提供 WebView2 引导。Ubuntu 可用 `sudo apt install ./<文件名>.deb` 安装，或赋予 AppImage 执行权限后运行。没有 FUSE 时可使用 `--appimage-extract-and-run`。已安装用户可在应用的「关于 → 检查更新」获取新版。

**English** · For Windows portable, extract the ZIP and run `rk-serial-desktop.exe` with `rk-serial-bridge.exe` in the same directory. The Windows installer can bootstrap WebView2. On Ubuntu, install the Debian package with `sudo apt install ./<filename>.deb`, or make the AppImage executable and run it. Use `--appimage-extract-and-run` if FUSE is unavailable. Existing users can check for updates from the About dialog.

应用会在旧安装包匹配时优先尝试经过校验的差量更新，必要时回退到完整安装包。The app tries a verified delta update when a matching previous package is available and falls back to the full package when needed.

## 接入 Agent · Connect an agent

在 GUI 打开「Agent 连接」，选择平台并安装随软件提供的 **rk-serial skill**。安装完成后，在 Agent 中说明端口和任务，例如「使用 rk-serial skill 查看 COM10 的启动日志」。

Open **Agent Connection** in the GUI, select your platform, and install the bundled **rk-serial skill**. Then give your agent a port and a task, for example, “Use the rk-serial skill to inspect the boot log on COM10.”

可选平台包括 Codex、Claude Code、Pi、OpenCode、Gemini CLI、GitHub Copilot CLI、Cursor 和兼容 Agent。

Supported platforms include Codex, Claude Code, Pi, OpenCode, Gemini CLI, GitHub Copilot CLI, Cursor, and compatible agents.

## 致谢 · Acknowledgments

rk-serial 在 **@吴镜** 提供的 Skills 基础上开发。感谢 **@周潼**、**@伟鹏** 提出改进建议并参与测试，也感谢 Rockchip 同事在日常板卡开发与联调中分享的经验和反馈。

rk-serial builds on the skills contributed by **@吴镜**. Thanks to **@周潼** and **@伟鹏** for their suggestions and testing, and to our Rockchip colleagues for sharing their experience and feedback from day-to-day board development and debugging.

本仓库提供公开下载与版本说明。 · This repository hosts public downloads and release notes.
