# rk-serial 串口调试助手

Rockchip RT-Thread 板卡多串口调试助手（Tauri 桌面 GUI + Python 串口服务端）。

本仓库为 **公开发布仓库**：仅提供编译好的程序与下载说明，**不含源码**。

## 下载

最新版本：[Releases](https://github.com/LiWeny16/rk-serial-releases/releases/latest)

| 文件 | 说明 |
| --- | --- |
| `rk-serial-vX.Y.Z-portable.zip` | 绿色版：解压后 `rk-serial-desktop.exe` 与 `rk_serial_service.exe` 放同一目录，双击即用 |
| `rk-serial_X.Y.Z_x64-setup.exe` | NSIS 安装包（推荐）：自动安装并覆盖旧版本，自带 WebView2 引导 |
| `rk-serial_X.Y.Z_x64_en-US.msi` | MSI 安装包 |

## 安装与更新

- **安装包**：运行 `x64-setup.exe` 按向导安装；下次有新版时，应用内“关于 → 检查更新 → 下载并安装更新”会自动下载安装包并启动覆盖安装。
- **绿色版**：解压 zip 即可；升级时下载新 zip 覆盖同目录两个 exe 即可（数据默认保存在程序目录 `runtime\`）。
- 应用启动时会自动检查新版本，状态栏“关于”按钮出现红点时表示有更新。

## 系统要求

- Windows 10 / 11 x64（需 WebView2 运行时，Win10/11 一般自带；安装包会自动引导安装）
- USB 转串口驱动（如 CH340/CP210x，按设备实际型号安装）

## 常见问题

- **日志出现行内乱码 / 二进制字符**（如 `sai1[HALDSD[`）：多为设备端把文本日志与中断/二进制数据并发输出到同一 UART，非本工具解码问题；可在界面勾选 HEX 查看原始字节确认。
- **中文乱码**：在“高级设置”中切换解码编码（UTF-8 / GBK / GB18030 / Big5 等），选择会被记忆。
- 需要帮助或反馈，请联系作者。
