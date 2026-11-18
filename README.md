<p align="center">
  <img src="assets/logo.png" alt="rk-serial 图标" width="96">
</p>

<h1 align="center">rk-serial · 串口调试助手</h1>

<p align="center">面向人与 AI Agent 协作的桌面串口调试工具</p>

<p align="center">
  <a href="#下载与安装">下载</a> ·
  <a href="README.en.md">English</a>
</p>

## 解决了什么痛点

rk-serial 来自 Rockchip 同事在板卡开发与联调中的实际需求。它把工程师和 Agent 接入同一个可观察的串口工作台，解决三个常见问题。

- **多个 Agent 调试同一个串口，命令容易交叉。** 多个 Agent 可以同时读取日志，各自保留读取位置。需要控制串口时，按端口排队，每次只授予一个控制者。多步调试可以在同一次控制权内完成。
- **工程师和 Agent 同时操作，容易抢占或误发。** GUI 统一持有串口。Agent 占用时，人工发送会收到端口忙提示，可以在控制权释放后继续操作。不同串口独立协调。
- **Agent 接管后，工程师难以知道发生了什么。** 人和 Agent 共用同一个可见终端，工程师可以继续查看收发记录和日志。「Agent 连接」还能查看已登记的调试任务、当前控制者和等待状态。

## 下载与安装

以下链接可直接下载 **v3.22.5**。Ubuntu 用户推荐安装与系统架构匹配的 `.deb` 包，可运行 `dpkg --print-architecture` 确认架构。

| 系统 | 推荐下载 | 其他格式 |
| --- | --- | --- |
| Windows 10 / 11（x64） | [安装包（EXE）][windows-exe] | [MSI 安装包][windows-msi] · [绿色版（ZIP）][windows-zip] |
| Ubuntu 22.04 / 24.04 / 26.04 LTS（amd64 / x86_64） | [amd64 安装包（DEB）][ubuntu-amd64-deb] | [amd64 便携版（AppImage）][ubuntu-amd64-appimage] |
| Ubuntu 22.04 / 24.04 / 26.04 LTS（arm64 / aarch64） | [arm64 安装包（DEB）][ubuntu-arm64-deb] | [arm64 便携版（AppImage）][ubuntu-arm64-appimage] |

- **Windows**：运行 EXE 安装包，按向导完成安装，安装程序可引导安装 WebView2。使用绿色版时，解压后运行 `rk-serial-desktop.exe`，并将 `rk-serial-bridge.exe` 保留在同一目录。
- **Ubuntu**：用 `sudo apt install ./<文件名>.deb` 安装。使用 AppImage 时，赋予文件执行权限后运行。没有 FUSE 时，可加上 `--appimage-extract-and-run`。

已安装用户可通过「关于 → 检查更新」获取新版。旧安装包匹配时，应用会优先尝试经过校验的差量更新，必要时回退到完整安装包。[查看所有版本](https://github.com/LiWeny16/rk-serial-releases/releases)

## 日常调试能力

| 场景 | rk-serial 的做法 |
| --- | --- |
| **在 Linux 和 RT-Thread（RTT）之间切换** | 按串口识别系统类型，也支持手动标记。命令大全提供 Linux、RT-Thread 和通用命令，可搜索用法、一键加入共享快捷列表并拖动排序。 |
| **快速发送和重复测试** | 从发送历史联想命令，支持快捷命令、定时循环发送、HEX 和可配置行尾。多个串口各自保留发送草稿。 |
| **录制串口和复盘** | 使用 **串口 Record** 按串口录制，用时长或容量控制任务，也能按设定循环发送。录制可在 GUI 查看，支持检索和导出，便于复盘偶发问题。 |
| **让 Agent 执行完整调试流程** | 内置 Agent bridge 覆盖串口配置与连接、读写、快捷命令、录制、归档检索和导出、批处理与验收用例。Agent 与工程师在同一 GUI 中操作和观察串口。 |

## 界面预览

![rk-serial 主工作台，包含串口标签、交互终端、发送区与常用命令](assets/workspace.png)

| Linux / RT-Thread 命令大全 | Agent 连接 |
| :---: | :---: |
| <a href="assets/command-catalog.png"><img src="assets/command-catalog.png" alt="命令大全的系统筛选与快捷操作" width="420"></a> | <a href="assets/agent-connection.png"><img src="assets/agent-connection.png" alt="Agent 调试任务及 Skill 安装面板" width="420"></a> |

<p align="center">
  <a href="assets/serial-record.png"><img src="assets/serial-record.png" alt="串口 Record 的录制设置" width="640"></a>
  <br>串口录制
</p>

## 接入 Agent

在 GUI 打开「Agent 连接」，选择平台并安装随软件提供的 **rk-serial skill**。安装完成后，在 Agent 中说明端口和任务，例如「使用 rk-serial skill 查看 COM10 的启动日志」。

可选平台包括 Codex、Claude Code、Pi、OpenCode、Gemini CLI、GitHub Copilot CLI、Cursor 和兼容 Agent。

## 致谢

rk-serial 在 **@吴镜** 提供的 Skills 基础上开发。感谢 **@周潼**、**@伟鹏** 提出改进建议并参与测试，也感谢 Rockchip 同事在日常板卡开发与联调中分享的经验和反馈。

本仓库提供公开下载与版本说明。

[windows-exe]: https://github.com/LiWeny16/rk-serial-releases/releases/download/v3.22.5/rk-serial_3.22.5_x64-setup.exe
[windows-msi]: https://github.com/LiWeny16/rk-serial-releases/releases/download/v3.22.5/rk-serial_3.22.5_x64_en-US.msi
[windows-zip]: https://github.com/LiWeny16/rk-serial-releases/releases/download/v3.22.5/rk-serial-v3.22.5-portable.zip
[ubuntu-amd64-deb]: https://github.com/LiWeny16/rk-serial-releases/releases/download/v3.22.5/rk-serial_3.22.5_amd64.deb
[ubuntu-arm64-deb]: https://github.com/LiWeny16/rk-serial-releases/releases/download/v3.22.5/rk-serial_3.22.5_arm64.deb
[ubuntu-amd64-appimage]: https://github.com/LiWeny16/rk-serial-releases/releases/download/v3.22.5/rk-serial_3.22.5_amd64.AppImage
[ubuntu-arm64-appimage]: https://github.com/LiWeny16/rk-serial-releases/releases/download/v3.22.5/rk-serial_3.22.5_arm64.AppImage
