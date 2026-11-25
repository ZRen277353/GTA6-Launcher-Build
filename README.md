# GTA6启动器编译 / GTA6 Launcher Builds

Workflow-only Windows x64 portable builds from the latest upstream sources. This independent repository contains only this README and GitHub Actions workflow files: no launcher source code, game files, keys or console SDK components are committed here. This is an unofficial build service, not an official PC game release.

本仓库仅用于从最新上游源码编译 Windows x64 便携包，是独立仓库，仅包含 README 和 GitHub Actions 工作流，不存放启动器源码、游戏文件、密钥或主机 SDK。本项目提供非官方构建，并非游戏官方 PC 版本。

| Original project / 原项目 | Executable / 程序 | ZIP directory / 解压目录 |
| --- | --- | --- |
| [Marvisak/VIctory](https://github.com/Marvisak/VIctory) | victory.exe | Marvisak/VIctory/ |
| [xcl-dev/xcl](https://github.com/xcl-dev/xcl) | xcl.exe | xcl-dev/xcl/ |
| [fxceless/victory-fork](https://github.com/fxceless/victory-fork) | victory.exe | fxceless/victory-fork/ |
| [Zettrox/ztrx](https://github.com/Zettrox/ztrx) | victory.exe | Zettrox/ztrx/ |
| [uiliannn/VItoriaBr](https://github.com/uiliannn/VItoriaBr) | victory.exe | uiliannn/VItoriaBr/ |
| [Belobug/VIctory-Belobug-AMD](https://github.com/Belobug/VIctory-Belobug-AMD) | victory.exe | Belobug/VIctory-Belobug-AMD/ |

## English

### Fork and build it yourself

1. Sign in to GitHub, click **Fork** at the top of this repository, and create a copy under your own account.
2. Open **Actions** in your fork. If GitHub asks, enable workflows by clicking **I understand my workflows, go ahead and enable them**.
3. Select **Build launchers / 编译启动器**, click **Run workflow**, select your default branch and choose **all** or one upstream project.
4. Run the workflow. Each selected project resolves its upstream default branch at build time, records the complete commit SHA, and checks out that exact commit. You do not need to fork or synchronize the six upstream projects.
5. Wait until **all selected projects** compile and pass ZIP checks. The workflow then publishes **one combined Release per run**, containing a separate Windows x64 ZIP for each selected project. Open **Releases** in **your fork** to download the ZIP you want. If any selected project fails, **no Release is published**; fix the failure and rerun the failed jobs.

No personal access token or repository secret is needed. The built-in GITHUB_TOKEN reads upstream metadata and publishes to your fork. The build job has read-only contents permission; the separate publisher has contents: write. If your organization or account restricts Actions or write tokens, allow this workflow in its settings. Upstream updates do not trigger builds automatically: run it manually whenever you need a new build.

### Download and run

Extract the entire ZIP into a writable folder. Open **Launch.bat** inside **original-owner/original-repository/**, or drag your game's EXE onto it. VIctory variants prompt for the full game EXE path; XCL opens its own chooser. Saves and caches are kept beside the launcher in data/ and cache/.

Packages contain the original executable, launcher, upstream README and license notices, the exact source.zip, BUILD-INFO.json and SHA256SUMS.txt. XCL additionally bundles checksum-verified official DXC and D3D12 Agility runtimes with their separate license notices. Windows Gaming Services / XGameRuntime must already be installed for XCL. Rust and the full Windows SDK are not needed to use the binaries.

Each release title includes the **compile completion time in Beijing time (UTC+08:00)**. The notes show the batch compile window and each project’s own compile completion time, together with the **original repository URL, complete source Git commit, source branch, commit date, Git client version, Rust version, build run, and archive SHA-256**. Each Release includes BUILD-MANIFEST.json and SHA256SUMS.txt, plus uniquely named per-project metadata and checksum files. All assets are uploaded and checked in a draft before the single Release becomes public. The build checks release compilation, CLI --help and extracted file hashes. It does **not** establish gameplay, rendering correctness or performance. Supported Windows updates, GPU features/drivers and your own unpacked games are still required. Read each original project's requirements. No decryption, game downloading or game data is provided.

Release assets remain until deleted; intermediate Actions transfer artifacts expire after 7 days. GitHub's automatically generated “Source code” links refer to this workflow repository: the exact launcher sources are in **source.zip inside the portable package**.

## 中文

### Fork 后自己编译

1. 登录 GitHub，在本仓库页面点击 **Fork**，复制到自己的账号下。
2. 打开自己仓库的 **Actions**。如果 GitHub 提示未启用工作流，点击确认启用。
3. 选择 **Build launchers / 编译启动器**，点击 **Run workflow**，选择默认分支，并选择 **all（全部）** 或某个项目。
4. 点击运行。每个项目会在构建时查询原仓库默认分支的最新提交，记录完整 SHA，再检出该提交编译。无需另外 Fork 或手动同步六个上游仓库。
5. 等待**所有所选项目**编译成功并通过 ZIP 校验。每次运行**只统一发布一个 Release**，其中分别附上各项目的 Windows x64 ZIP。在**你自己仓库的 Releases** 下载所需项目。任一所选项目失败则**不发布 Release**，修复后可重新运行失败的任务。

无需填写个人密钥、PAT 或仓库 Secret，使用 GitHub 自带的 GITHUB_TOKEN。构建阶段仅有 contents: read；独立发布阶段拥有 contents: write。如组织或账号限制 Actions 或写权限，需在相应设置中允许该工作流。上游更新不会自动触发，想更新时手动运行即可。

### 解压使用

将 ZIP **完整解压**到可写目录，在**原作者/仓库名/**目录内双击 **Launch.bat**，也可以把游戏主程序拖到 BAT 上。VIctory 系列会提示输入游戏 EXE 完整路径；XCL 打开自带的选择窗口。存档和缓存放在启动器旁的 data/、cache/ 中。

压缩包包含原程序、启动器、上游说明与许可证、对应源码 source.zip、BUILD-INFO.json 和 SHA256SUMS.txt。XCL 额外附带经过校验的微软官方 DXC、D3D12 Agility 运行库及各自许可证，但 Gaming Services / XGameRuntime 仍需系统安装。使用这些程序不需要 Rust 或完整 Windows SDK。

每个 Release 的标题直接标明**编译完成时间（北京时间 UTC+08:00）**，正文显示本批次编译起止时间以及每个项目各自的编译完成时间，并标明**原仓库地址、编译源码完整 Git SHA、分支、源码提交时间、Git 工具版本、Rust 版本、构建记录和压缩包 SHA-256**。同时提供统一的 BUILD-MANIFEST.json、SHA256SUMS.txt，以及名称不冲突的各项目 metadata 和校验文件。所有附件先上传并校验，最后才将这一个 Release 从草稿转为正式发布。工作流验证 Release 编译、命令行帮助及解压文件校验，**不代表游戏已可玩，也不验证画面正确性或帧率**。仍需符合上游的 Windows 更新、显卡功能和驱动要求，并自行准备已解包的游戏文件。

Releases 附件不会随中转构建产物的 7 天保留期失效，除非手动删除。GitHub 自动生成的“Source code”附件是本工作流仓库源码；**实际启动器源码在便携包内的 source.zip 中**。

## fxceless/victory-fork performance preset / 性能预设

The fxceless package's **Launch.bat** now enables the README run-253 switches used for its reported 8-10 FPS, plus the author's documented run-279 additions (2048 MiB card windows, 1280 MiB buffer budget with 64 KiB minimum, texture copy-back, shorter buffer hashes). It uses Series X mode, keeps shader optimization at 63 and wave64 at upstream defaults, and clears timing/debug instrumentation. These are **runtime environment variables**, not Rust compile flags; setting them only while compiling does not enable them when playing. The executable is still an unmodified **Release** build without the diagnostics feature. Start via Launch.bat to apply the preset; starting victory.exe alone does not.

fxceless 包内的 **Launch.bat** 已开启 README 中用于实测 8-10 帧的 run 253 开关，并加入作者 Run 279 推荐项：2048 MiB 显存窗口、1280 MiB 缓冲预算（最小 64 KiB）、纹理回写与短哈希。使用 Series X 模式，着色器优化保持 63，wave64 保留上游默认设置，并清除计时和调试变量。它们是**运行时环境变量**，不是 Rust 编译参数；只在编译时设置并不会影响实际游玩。程序仍是未修改上游源码的 **Release** 构建，不启用 diagnostics。请通过 Launch.bat 启动，单独运行 victory.exe 不会自动应用预设。

Upstream reported **8-10 FPS in the first rooms and 5-8.5 in the opening cutscene** on Windows 11, i7-14700KF, RTX 4070 12GB and 32GB RAM. This is not a frame-rate guarantee or a gameplay test of our package. Card window/buffer budgets are additional partial VRAM budgets, not total requirements. See **PERFORMANCE.md** and **RUNTIME-PRESET.json** inside the package for every applied switch, the exact reference SHA and instructions. 

上游实测为 Win11、i7-14700KF、RTX 4070 12GB、32GB 内存：**首个房间 8-10 帧，开场过场 5-8.5 帧**；不保证你的硬件达到该帧率，也不代表本包已经游戏实测。显存窗口与缓冲只代表部分预算，不是总显存要求。包内 **PERFORMANCE.md** 与 **RUNTIME-PRESET.json** 记录全部开关、参数来源的完整 SHA 及用法。

Preset reference / 预设来源：[README](https://github.com/fxceless/victory-fork/blob/6d20eed6ae8afe37c52f1911b46537b0ea8bcd0d/README.md) · [diagnostics/README.md](https://github.com/fxceless/victory-fork/blob/6d20eed6ae8afe37c52f1911b46537b0ea8bcd0d/diagnostics/README.md). Packages always record their own compiled source SHA separately.

## Attribution / 致谢

All launcher code belongs to its original authors and retains its upstream license. VIctory and its variants use GPL-3.0-or-later; XCL uses GPL-2.0-only. Microsoft runtime files carry separate notices. This repository builds upstream code without patching its runtime sources.

启动器代码属于原作者并遵循其上游许可证：VIctory 及其分支为 GPL-3.0-or-later，XCL 为 GPL-2.0-only；微软运行库保留独立许可证。本仓库不修改上游运行时代码。
