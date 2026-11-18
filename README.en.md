<p align="center">
  <img src="assets/logo.png" alt="rk-serial logo" width="96">
</p>

<h1 align="center">rk-serial</h1>

<p align="center">Desktop serial debugging for engineers and AI agents</p>

<p align="center">
  <a href="#download-and-install">Download</a> ·
  <a href="#agent-control-demo">Demo</a> ·
  <a href="#architecture-and-log-layers">Architecture</a> ·
  <a href="README.md">简体中文</a>
</p>

## What rk-serial solves

rk-serial grew out of board bring-up and debugging at Rockchip. It gives engineers and AI agents a shared desktop workspace for serial debugging.

| Workflow | What can go wrong with a typical setup | How rk-serial helps |
| --- | --- | --- |
| Multiple agents share a port | Separate agents compete to open the device. Shared scripts can interleave commands and replies. | Agents read concurrently with independent cursors. Writes queue in order per port, and an agent can hold control for a sequence of operations. |
| An engineer and an agent work on the same port | A command entered in the terminal can interrupt an agent procedure. | The desktop owns the connection. It reports the port as busy while an agent holds control, then lets the engineer resume sending. Each port is coordinated independently. |
| An engineer follows agent activity | When an agent uses a separate script, the engineer usually has to wait for its report. | The terminal keeps showing traffic and logs. The port tab shows AI activity. Agent Connection shows registered tasks, the current holder, and the queue. |
| A rare change is buried in a long log | Sending the whole log or repeatedly reading its tail fills the context with duplicate output. | L0 keeps the original evidence, L1 indexes templates and parameters, and L2 returns counts, changes, and references. Agents can expand the evidence that matters. |
| An intermittent issue needs a recording or a repeatable check | Copying terminal output loses context. Ad hoc scripts need their own recording, polling, and retry logic. | Serial Record can run by duration, loop count, or capacity and export evidence. Saved batches and checks use request IDs to avoid resending on retries. |

## Agent control demo

COM10 stays connected and displays real RT-Thread output. The capture shows the agent claiming control, reading the log, and releasing control.

![Live capture of an agent claiming COM10, reading the log, and releasing control](assets/agent-demo-en.gif)

## Download and install

The links below download **v3.22.5** directly. On Ubuntu, we recommend the `.deb` package for your architecture. Run `dpkg --print-architecture` to check which package you need.

| Platform | Recommended download | Other formats |
| --- | --- | --- |
| Windows 10 / 11 (x64) | [EXE installer][windows-exe] | [MSI installer][windows-msi] · [Portable ZIP][windows-zip] |
| Ubuntu 22.04 / 24.04 / 26.04 LTS (amd64 / x86_64) | [amd64 Debian package][ubuntu-amd64-deb] | [amd64 AppImage][ubuntu-amd64-appimage] |
| Ubuntu 22.04 / 24.04 / 26.04 LTS (arm64 / aarch64) | [arm64 Debian package][ubuntu-arm64-deb] | [arm64 AppImage][ubuntu-arm64-appimage] |

- **Windows.** Run the EXE installer and follow the prompts. The installer can set up WebView2. For the portable ZIP, extract the archive and run `rk-serial-desktop.exe` with `rk-serial-bridge.exe` in the same directory.
- **Ubuntu.** Install the package with `sudo apt install ./<filename>.deb`. To use an AppImage, make it executable and run it. If FUSE is unavailable, pass `--appimage-extract-and-run`.

Check for updates from the About dialog. The app tries a verified delta update when a matching previous package is available and falls back to the full package when needed. [All releases](https://github.com/LiWeny16/rk-serial-releases/releases)

## Architecture and log layers

Engineers use the GUI and agents use the bridge to work through the same desktop app. The serial core handles I/O, while the app coordinates access per port. The terminal continues to show traffic as agents query the log layers.

![Architecture diagram showing the desktop app coordinating UART access for the GUI and agent bridge, and L0, L1, and L2 serving log queries](assets/architecture-en.png)

| Layer | What it stores or returns |
| --- | --- |
| **L0 · Original traffic** | Keeps raw bytes, decoded text, direction, and timestamps per port. Evidence references identify text or bytes to retrieve. |
| **L1 · Rebuildable compressed index** | Groups repeated structures by template and keeps parameter values, counts, and L0 locations. Completed Record segments can persist a compressed index. Queries can scan L0 when an index is unavailable. |
| **L2 · Agent queries and evidence** | `log-query` returns match counts, groups or changes, and evidence references within a byte budget. Responses report completeness and pagination. `log-expand` retrieves the requested L0 evidence. |

### Token comparison

The table compares passing log text directly to an agent with the responses from rk-serial for the same sample. Both sides were counted using `cl100k_base`.

| Measured task | Send log text to the agent | rk-serial responses | Fewer tokens |
| --- | ---: | ---: | ---: |
| **Find one rare change among 100,001 error lines** (v3.22.5) | All matching lines · **1,000,010** | Query and evidence expansion · **367** | **99.96%** |
| **Search about 50 MiB of archived logs** (v3.13 benchmark) | Entire log · **13,516,805** | 29 compact query pages · **6,665** | **99.95%** |

Both benchmarks used synthetic samples. In the first, 100,000 matching lines repeated the same error and one had a different code. The query searched for `ERROR` and expanded the rare result to verify its original bytes. It used the L0 scan fallback. Actual token usage depends on the log, query, and tokenizer.

## Debugging features

| Workflow | What you can do |
| --- | --- |
| **Move between Linux and RT-Thread (RTT)** | Detect or set the system type per port. Browse the Linux, RT-Thread, and general command catalog, then add and reorder shortcuts in a shared list. |
| **Watch multiple ports at once** | Each port has its own terminal and connection status. Use ANSI, HEX, timestamps, search, pause, log export, and baud-rate probing. |
| **Send commands and repeat tests** | Get suggestions from send history and use quick commands, timed line-by-line loops, HEX, or configurable line endings. Each port keeps its own draft. |
| **Capture traffic and investigate intermittent issues** | Start a per-port **Serial Record**, set time, loop count, or capacity rules, and optionally send commands in a loop. Review, search, and export recordings. |
| **Run a complete workflow with an agent** | The bundled bridge supports port configuration, connections, reads and writes, incremental queries, quick commands, recording, archive search and export, batches, and reusable checks. It can also coordinate an explicit handoff to an external flashing tool. |

## Screenshots

![rk-serial workspace showing live RT-Thread output on COM10 with the AI badge on its tab](assets/workspace.png)

| Linux / RT-Thread command catalog | Agent Connection |
| :---: | :---: |
| <a href="assets/command-catalog.png"><img src="assets/command-catalog.png" alt="Command catalog with system filters and shortcut actions" width="420"></a> | <a href="assets/agent-connection.png"><img src="assets/agent-connection.png" alt="Registered agent tasks and skill installation" width="420"></a> |

<p align="center">
  <a href="assets/serial-record.png"><img src="assets/serial-record.png" alt="Serial Record configuration" width="640"></a>
  <br>Serial Record
</p>

## Connect an agent

Open **Agent Connection** in the GUI, select your platform, and install the bundled **rk-serial skill**. Then give your agent a port and a task, for example, “Use the rk-serial skill to inspect the boot log on COM10.”

Supported platforms include Codex, Claude Code, Pi, OpenCode, Gemini CLI, GitHub Copilot CLI, Cursor, and compatible agents.

## Acknowledgments

rk-serial builds on the skills contributed by **@吴镜**. Thanks to **@周潼** and **@伟鹏** for their suggestions and testing, and to our Rockchip colleagues for sharing their experience and feedback from day-to-day board development and debugging.

This repository hosts public downloads and release notes.

[windows-exe]: https://github.com/LiWeny16/rk-serial-releases/releases/download/v3.22.5/rk-serial_3.22.5_x64-setup.exe
[windows-msi]: https://github.com/LiWeny16/rk-serial-releases/releases/download/v3.22.5/rk-serial_3.22.5_x64_en-US.msi
[windows-zip]: https://github.com/LiWeny16/rk-serial-releases/releases/download/v3.22.5/rk-serial-v3.22.5-portable.zip
[ubuntu-amd64-deb]: https://github.com/LiWeny16/rk-serial-releases/releases/download/v3.22.5/rk-serial_3.22.5_amd64.deb
[ubuntu-arm64-deb]: https://github.com/LiWeny16/rk-serial-releases/releases/download/v3.22.5/rk-serial_3.22.5_arm64.deb
[ubuntu-amd64-appimage]: https://github.com/LiWeny16/rk-serial-releases/releases/download/v3.22.5/rk-serial_3.22.5_amd64.AppImage
[ubuntu-arm64-appimage]: https://github.com/LiWeny16/rk-serial-releases/releases/download/v3.22.5/rk-serial_3.22.5_arm64.AppImage
