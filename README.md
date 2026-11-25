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

## English

### Fork and build it yourself

1. Sign in to GitHub, click **Fork** at the top of this repository, and create a copy under your own account.
2. Open **Actions** in your fork. If GitHub asks, enable workflows by clicking **I understand my workflows, go ahead and enable them**.
3. Select **Build launchers / 编译启动器**, click **Run workflow**, select your default branch and choose **all** or one upstream project.
4. Run the workflow. Each selected project resolves its upstream default branch at build time, records the complete commit SHA, and checks out that exact commit. You do not need to fork or synchronize the five upstream projects.
5. Once a project's build and ZIP checks succeed, open **Releases** in **your fork** and download its Windows x64 ZIP. Each selected project gets a separate release. If one fails, the other projects can still publish.

No personal access token or repository secret is needed. The built-in GITHUB_TOKEN reads upstream metadata and publishes to your fork. The build job has read-only contents permission; the separate publisher has contents: write. If your organization or account restricts Actions or write tokens, allow this workflow in its settings. Upstream updates do not trigger builds automatically: run it manually whenever you need a new build.

### Download and run

Extract the entire ZIP into a writable folder. Open **Launch.bat** inside **original-owner/original-repository/**, or drag your game's EXE onto it. VIctory variants prompt for the full game EXE path; XCL opens its own chooser. Saves and caches are kept beside the launcher in data/ and cache/.

Packages contain the original executable, launcher, upstream README and license notices, the exact source.zip, BUILD-INFO.json and SHA256SUMS.txt. XCL additionally bundles checksum-verified official DXC and D3D12 Agility runtimes with their separate license notices. Windows Gaming Services / XGameRuntime must already be installed for XCL. Rust and the full Windows SDK are not needed to use the binaries.

Each release lists the **original repository URL, complete source Git commit, source branch, commit date, Git client version, Rust version, build run, and archive SHA-256**. The build checks release compilation, CLI --help and extracted file hashes. It does **not** establish gameplay, rendering correctness or performance. Supported Windows updates, GPU features/drivers and your own unpacked games are still required. Read each original project's requirements. No decryption, game downloading or game data is provided.

Release assets remain until deleted; intermediate Actions transfer artifacts expire after 7 days. GitHub's automatically generated “Source code” links refer to this workflow repository: the exact launcher sources are in **source.zip inside the portable package**.

## 中文

### Fork 后自己编译

1. 登录 GitHub，在本仓库页面点击 **Fork**，复制到自己的账号下。
2. 打开自己仓库的 **Actions**。如果 GitHub 提示未启用工作流，点击确认启用。
3. 选择 **Build launchers / 编译启动器**，点击 **Run workflow**，选择默认分支，并选择 **all（全部）** 或某个项目。
4. 点击运行。每个项目会在构建时查询原仓库默认分支的最新提交，记录完整 SHA，再检出该提交编译。无需另外 Fork 或手动同步五个上游仓库。
5. 某个项目编译和压缩包验证成功后，在**你自己仓库的 Releases** 中下载其 Windows x64 ZIP。各项目独立发布 Release；单个项目失败不会阻止其他项目发布。

无需填写个人密钥、PAT 或仓库 Secret，使用 GitHub 自带的 GITHUB_TOKEN。构建阶段仅有 contents: read；独立发布阶段拥有 contents: write。如组织或账号限制 Actions 或写权限，需在相应设置中允许该工作流。上游更新不会自动触发，想更新时手动运行即可。

### 解压使用

将 ZIP **完整解压**到可写目录，在**原作者/仓库名/**目录内双击 **Launch.bat**，也可以把游戏主程序拖到 BAT 上。VIctory 系列会提示输入游戏 EXE 完整路径；XCL 打开自带的选择窗口。存档和缓存放在启动器旁的 data/、cache/ 中。

压缩包包含原程序、启动器、上游说明与许可证、对应源码 source.zip、BUILD-INFO.json 和 SHA256SUMS.txt。XCL 额外附带经过校验的微软官方 DXC、D3D12 Agility 运行库及各自许可证，但 Gaming Services / XGameRuntime 仍需系统安装。使用这些程序不需要 Rust 或完整 Windows SDK。

每个 Release 都标明**原仓库地址、编译源码完整 Git SHA、分支、提交时间、Git 工具版本、Rust 版本、构建记录和压缩包 SHA-256**。工作流验证 Release 编译、命令行帮助及解压文件校验，**不代表游戏已可玩，也不验证画面正确性或帧率**。仍需符合上游的 Windows 更新、显卡功能和驱动要求，并自行准备已解包的游戏文件。

Releases 附件不会随中转构建产物的 7 天保留期失效，除非手动删除。GitHub 自动生成的“Source code”附件是本工作流仓库源码；**实际启动器源码在便携包内的 source.zip 中**。

## Attribution / 致谢

All launcher code belongs to its original authors and retains its upstream license. VIctory and its variants use GPL-3.0-or-later; XCL uses GPL-2.0-only. Microsoft runtime files carry separate notices. This repository builds upstream code without patching its runtime sources.

启动器代码属于原作者并遵循其上游许可证：VIctory 及其分支为 GPL-3.0-or-later，XCL 为 GPL-2.0-only；微软运行库保留独立许可证。本仓库不修改上游运行时代码。
