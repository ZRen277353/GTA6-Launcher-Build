# GTA6启动器编译 / GTA6 Launcher Builds

[Run a build / 运行编译](../../actions/workflows/build.yml) · [Releases / 下载](../../releases) · [PortWatch VI](https://portwatch-vi.xyz/)

A workflow-only repository for building the latest public upstream Windows x64 launchers. It contains **only this README and two GitHub Actions workflows**: no launcher implementation, game files, console SDKs or keys. Upstream runtime sources are not patched.

本项目只有 **README 和两个工作流文件**，用于从上游最新公开源码编译 Windows x64 启动器。不存放启动器实现、游戏文件、主机 SDK 或密钥，也不修改上游运行时代码。

## Projects / 编译列表

| Upstream / 原仓库 | Notes / 说明 |
| --- | --- |
| [stellarr-studio/kelvaroq](https://github.com/stellarr-studio/kelvaroq) | Full official portable layout: GUI/CLI, patched native backend, embedded Python/reference runtime and notices. / 完整便携包，包含图形界面、命令行、配套后端及运行库。 |
| [trustno1-coder/VIctory-NVIDIA](https://github.com/trustno1-coder/VIctory-NVIDIA) | Optimized Release; exact-source NVIDIA settings with the documented Series S / 540p own-upscaler path and offline precompile helpers. / NVIDIA专用预设与官方540p自带升频路径，附纯离线预编译脚本。 |
| [LooperSalty/AnyXboxX](https://github.com/LooperSalty/AnyXboxX) | Optimized GUI + CLI + exact vendored VIctory; official Windows/NVIDIA profile and offline shader/pipeline script. / GUI、CLI及配套后端，使用上游NVIDIA预设，附离线预编译脚本；diagnostics为上游必需特性。 |
| [carlm125/cool_fox](https://github.com/carlm125/cool_fox) | Optimized Release with its exact upstream RTX pareto-residency / Series S preset; includes shader and recorded-pipeline precompile scripts. / 最高优化编译，使用上游RTX性能预设，附着色器及已记录管线的一键预编译。 |
| [avatarmanz/VIctory-7900XTX](https://github.com/avatarmanz/VIctory-7900XTX) | Added from PortWatch's public race; uses the exact source's official fast launch preset. AMD tuning; 64 GB RAM recommended upstream. / 新增榜单项目，提取官方性能启动预设，主要针对AMD。 |
| [DexNom/VIctory](https://github.com/DexNom/VIctory) | Added from PortWatch's public race; verifies format tables, builds all three executables. Default BAT uses the README no-RT path. / 新增榜单项目，含三个程序及格式表校验，默认关闭光追。 |
| [Belobug/VIctory-Belobug-AMD](https://github.com/Belobug/VIctory-Belobug-AMD) | AMD-oriented fork / 面向AMD的分支 |
| [uiliannn/VItoriaBr](https://github.com/uiliannn/VItoriaBr) | VIctory variant / VIctory分支 |
| [fxceless/victory-fork](https://github.com/fxceless/victory-fork) | Documented run-279 runtime settings plus the measured run-293 lookup cache / 已测性能预设及查找缓存 |
| [Marvisak/VIctory](https://github.com/Marvisak/VIctory) | Original VIctory / 原项目 |
| [xcl-dev/xcl](https://github.com/xcl-dev/xcl) | XCL, with checksum-verified DXC and Agility runtime files / 附校验过的微软运行库 |
| [Zettrox/ztrx](https://github.com/Zettrox/ztrx) | Kept in the selector; HTTP 404 is reported explicitly, without preventing other builds / 保留选项，404会明确记录并跳过 |

The public, active main-race repositories in the PortWatch data checked on **2026-11-26** are covered. Private builds and projects without downloadable public code cannot be compiled here. The list is deliberately reviewed rather than executing arbitrary repositories added by a website; future additions require updating both workflow allowlists.

已覆盖 **2026年11月26日** 检查时 PortWatch 主榜中可用的公开代码项目。私有版本和未开放源码的版本无法编译。名单经过审核，网站未来出现新项目时需更新两个工作流中的允许列表。

## Fork and build / Fork后自行编译

1. **Fork this repository** into your own GitHub account. / 点击本仓库右上角 **Fork**，复制到自己的账号。
2. Open **Actions** in your Fork and enable workflows if prompted. / 在Fork中进入 **Actions**，如有提示则启用工作流。
3. Choose **Build launchers / 编译启动器**, then **Run workflow**. / 选择编译工作流，点击 **Run workflow**。
4. Select **all** for one combined batch, or a single upstream. Choose the CPU target, then start. / 选择全部或单个项目，选择CPU指令集后开始。
5. Wait for the selected builds and the final **Publish one combined Release / 统一发布** job. Open **Releases** in your own Fork and download the desired ZIP. / 等待编译和统一发布完成，到自己Fork的 **Releases** 下载所需ZIP。

The upstream default-branch HEAD is resolved at build time and checked out by its exact full SHA. The upstream Rust toolchain and Cargo.lock are respected. Dependencies intentionally pinned or vendored by a project remain pinned/vendored: Kelvaroq builds its prescribed VIctory revision plus its ordered patches, and AnyXboxX builds its included third_party/victory and checks out its TomBox submodule by the committed SHA, not an unrelated fork HEAD. The submodule source is included as sources/tombox.zip. Upstream changes do not trigger automatic builds: manually run again whenever you want an update.

每个项目在构建时解析上游默认分支HEAD，并按完整SHA精确检出，使用上游指定的Rust工具链及Cargo.lock。项目有意固定或内置的依赖不会擅自升级：Kelvaroq使用其指定的VIctory提交及补丁，AnyXboxX使用自己的内置VIctory；两者后端详情写入清单和包内构建记录。想更新时再次手动触发即可，上游更新不会自动消耗编译额度。

Build jobs have `contents: read`; only the final publisher has `contents: write`. Account or organization Actions policies must permit these workflows. No personal token or additional secret is required in a normal Fork.

编译任务只有读取仓库权限，统一发布任务才有写入Release权限。普通Fork无需添加个人令牌或额外Secret；账号或组织需允许Actions及工作流写权限。

## Compiler optimization / 编译优化

Every successful package uses **Release, opt-level=3, fat LTO, codegen-units=1**, with **static MSVC CRT in the native game engines**, with incremental compilation disabled. Diagnostics is disabled except for **AnyXboxX** (upstream compile requirement, including queue::presented gating) and **VIctory-NVIDIA** (upstream runtime controls: old ray-pass filtering / new settings menu). Both remain optimized release-profile builds, not the separate diag profile, and retain their upstream light-DRED settings. The publisher validates each exception and its recorded reason; no other project can silently enable it. Line-table debug information is retained for crash analysis. Kelvaroq's official packagers set encoded rustflags; the build adapter appends the CPU/CRT flags while preserving their reproducible path flags, recording both native and frontend invocations in COMPILER-RECEIPT.json. Its native engine remains static-CRT, but the frontend uses dynamic CRT because upstream src/simd.rs explicitly imports ucrt; forcing static CRT there produces duplicate-symbol link errors. CPU/LTO/optimization levels are unchanged. The packaging adapter also normalizes the official compiled WPF companion location for the current Cargo build-directory layout without editing upstream runtime sources. Included non-Rust dependencies retain their own runtime requirements. The actual Rust version, flags and profile are saved in `BUILD-INFO.json`, project metadata and the unified manifest.

所有项目使用 **Release、最高常规优化等级3、fat LTO、单代码生成单元**，原生游戏后端使用 **静态CRT**，关闭增量编译。仅 **AnyXboxX** 因上游编译要求、**VIctory-NVIDIA** 因上游运行控制需要保留diagnostics特性；其余关闭。两者仍使用优化Release而非diag配置，保留上游轻量DRED，发布前校验例外原因。保留便于定位崩溃的行号调试信息。Kelvaroq前端SIMD库按上游设计显式调用动态UCRT，因此前端保留动态CRT，后端仍为静态CRT；不牺牲CPU/LTO优化，也不以忽略重复符号的危险链接选项强行通过。实际参数和Rust版本记录在构建信息及清单中。

- **x86-64-v3 (default):** optimized for modern CPUs with AVX2, FMA, BMI/BMI2 and related v3 instructions. / 默认适合支持这些指令的现代Intel/AMD处理器。
- **x86-64:** the CPU compatibility option for older machines. / 旧CPU使用兼容选项。

The build never uses `target-cpu=native` on a GitHub runner: that would target the runner's CPU rather than yours. Compiler optimization cannot guarantee better FPS in every scene or fix unimplemented rendering, GPU synchronization bugs or upstream crashes. These packages are not gameplay-benchmarked and do **not** promise stable 30 FPS.

不会把GitHub编译机的CPU当作你的CPU使用。编译优化不保证每个场景都更快，也无法自动修复未实现的渲染、GPU同步问题或全部崩溃；本项目不承诺稳定30帧。

## One Release, 404 fallback and version comparison / 统一发布、404兜底和版本对比

**One workflow run produces one Release after all selected projects have a verified result.** Successful projects provide ZIPs; an upstream repository/commit HTTP 404 provides an explicit **“project 404, cannot download source, cannot compile”** record. A 404 project does not block other projects and never reuses an older binary. Other failures, including compiler errors, rate limits, permissions and broken dependency downloads, still fail the batch and prevent publication.

**每次运行只统一发布一个Release。** 所有选择项目都有确定结果后才发布：成功项目提供ZIP，上游仓库或提交404则写明 **“项目404，无法下载源码，无法编译”**，不会阻塞其他项目，也不会混入旧程序。真实编译错误、权限、限流和依赖下载错误仍使整批失败，不会被当成404隐藏。

Even if every selected project is 404, one status-only Release is published, with no executable and no invented compile time. Interrupted builds or missing/corrupt artifacts never count as 404.

全部项目均404时发布一个仅含状态的Release，不附EXE，不伪造编译完成时间。中断任务或缺失、损坏的产物不会冒充404。

The Release title shows the compile completion time in **Beijing time (UTC+08:00)**. The source-version table shows each original repository, **this run's 12-character Git SHA**, the **last published all-project batch's 12-character SHA**, changed/unchanged/new/404 status, and compile time. Commit links and comparisons open the exact full SHAs. A more recent single-project release is not mistaken for the previous all-project batch. Legacy multi-project manifests are recognized.

Release标题显示 **北京时间UTC+08:00的编译完成时间**。表格对比原仓库、本次Git短版本、上次全项目统一构建的Git短版本、变化状态及编译时间。页面仅显示 **12位** 提交号，点击可查看完整提交或差异；较新的单项目构建不会覆盖上次全项目构建基准，旧版多项目清单也能识别。

Full SHAs remain in `BUILD-MANIFEST.json` and uniquely named per-project metadata, including unavailable status and previous version. Packages, checksums and metadata are uploaded to one draft Release, verified against remote asset sizes and SHA-256 digests, and then published together. Workflow runs are serialized to keep the previous-batch comparison unambiguous.

完整SHA及404状态保存在统一清单和各项目metadata中。附件全部上传到同一个草稿Release，核对远端大小和SHA-256后一次性公开。工作流串行发布，便于确定上次构建基准。

## Extract and run / 解压使用

Extract the entire ZIP into a writable folder. Its layout is **original-owner/original-repository/**. Run **Launch.bat** inside that folder or drag the game's EXE onto it. VIctory asks for the game executable; XCL opens its own game picker. VIctory saves and caches normally use `data/` and `cache/` beside the launcher. GUI projects can also store settings in AppData according to upstream; portable here means ready to extract/run without building, not that every setting stays in the ZIP folder.

完整解压到可写目录，在 **原作者/仓库名/** 内运行 **Launch.bat**，或将游戏主程序拖到BAT。VIctory存档和缓存一般位于旁边的 `data/`、`cache/`；GUI项目还可能按上游设计写入AppData。“便携”指解压可用、无需自己编译，不代表所有用户设置都在解压目录。

Packages include the executable, BAT, upstream README/license notices, exact `source.zip`, `BUILD-INFO.json` and file checksums. Projects with runtime presets also include `PERFORMANCE.md` and `RUNTIME-PRESET.json`; `Launch.bat --print-runtime-preset` prints the applied settings without starting the game. Runtime variables take effect when launching, not merely while compiling.

包内含EXE、BAT、原项目说明和许可证、对应源码及校验文件。有预设的包另附参数说明和JSON；`Launch.bat --print-runtime-preset` 可检查参数而不启动游戏。运行参数需在启动游戏前设置，仅在编译时设置不会自动生效。

- **fxceless:** retains the documented run-279 settings, shader opt 63 and upstream wave defaults; adds **`VICTORY_LOOKUP_CACHE=1`**, which upstream run 293 measured at about 14 ms/frame saved at equal draws. Unmeasured experiments are not enabled indiscriminately.
- **AvatarManz:** extracts the official `scripts/play.ps1` default fast settings from the exact compiled SHA, including threaded recording, joined submissions and upload-ahead. AMD-focused, upstream recommends 64 GB RAM; NVIDIA performance is not established. Optional ray tracing stays off. The original script is included as a reference and is not the portable entry point.
- **DexNom:** bundles **victory.exe, victory-play.exe and victory-warm.exe** and initializes checksum-pinned format tables before building. The BAT uses the README no-RT path, which is also upstream's suggested NVIDIA starting point. Select optional ray tracing/FSR explicitly with victory-play.exe.
- **XCL:** bundles checksum-pinned official DXC/Agility files and their licenses; **Windows Gaming Services / XGameRuntime must still be installed**.

No Rust or full Windows SDK is required to use the packages. You still need the upstream-supported Windows/GPU/driver features and your own unpacked game files. Releases persist independently of the seven-day intermediate Actions artifacts. GitHub's automatic “Source code” downloads are this workflow repository; **the actual launcher source is the `source.zip` inside each successful package**.

运行包无需Rust或完整Windows SDK，但仍需满足上游系统和显卡要求，并自行提供已解包游戏文件。Release附件独立于7天保留期的中转产物。GitHub自动附加的Source code是本工作流仓库；**实际启动器源码在各包的source.zip里**。

## Attribution / 致谢

All launcher code belongs to its original authors and retains its upstream license. VIctory variants and Kelvaroq use GPL-3.0-or-later; Kelvaroq also retains separate licenses/source notices for its reference CPU and embedded dependencies. XCL uses GPL-2.0-only. AnyXboxX preserves its own and its vendored backend notices. Microsoft runtime files retain their notices. PortWatch rankings and author reports help choose projects; they are not a performance guarantee by this builder.

所有启动器代码属于原作者并遵循上游许可证，微软运行库保留独立声明。榜单与作者实测用于选项目，不代表本编译仓库保证相同帧率或稳定性。

## cool_fox shader precompile / 着色器预编译

Extract the complete `carlm125/cool_fox/` folder. Run **Precompile-Shaders.bat**, or drag your game EXE onto it. It applies the same preset and local `cache/` as Launch.bat, translates all shaders discovered in `shaders.rpf` and the game EXE using `victory shaders`, then runs `victory warm` for **all recorded pipelines**. The executable queries your actual GPU; no old-fork wave or descriptor limits are hardcoded. Shader workers scale up to the logical CPU count with a free-RAM estimate; pipeline processes have a more conservative memory-aware cap of 8. This is a throughput-oriented default, not a measured proof of globally optimal settings. Logs are saved in `logs/`. Exit code 2 indicates partial shader/pipeline failure; exact grouped errors remain visible.

完整解压后，双击 **Precompile-Shaders.bat** 或把游戏EXE拖到它上面。脚本沿用 Launch.bat 的性能预设和本地缓存，先遍历 `shaders.rpf` 及游戏EXE中的着色器，再预热全部已记录GPU管线。程序自动探测显卡，不硬编码旧分支的GPU参数。按CPU线程数和可用内存估算并行度；GPU管线进程更保守、最多8个，避免盲目占满内存。日志保存在 `logs/`，部分失败返回2并保留具体原因。

GPU pipelines require state recorded during actual gameplay: if none exists, launch once through **Record-Pipelines.bat**, enter the desired scenes, exit normally, and rerun Precompile-Shaders.bat. Normal Launch.bat does not force this extra recording overhead. Never use someone else's game-derived caches. Driver/translator changes can require another warmup; compilation cannot generate an unobserved pipeline, fix unsupported shaders, or guarantee 30 FPS. This fork's upstream AMD baseline recommends 64 GB RAM; using an RTX-oriented preset does not guarantee it fits 32 GB in every scene.

GPU管线需要游戏实际记录：首次提示没有记录时，先用 **Record-Pipelines.bat** 进入所需场景并正常退出，再执行预编译。正常 Launch.bat 不强制增加管线记录开销。不要使用他人游戏衍生缓存。更换驱动或翻译器后可再次执行；无法提前生成从未记录的管线，不能修复未支持的着色器或保证30帧。上游AMD基线建议64GB内存，RTX预设也不代表所有场景都能在32GB内存下正常运行。

Optional / 可选: `Precompile-Shaders.bat -Jobs 20 -PipelineJobs 4`, `-SkipPipelines`, or `-DryRun -NoPause` (validation only, no cache changes). No game assets or GPU-specific caches are built or published by CI.

## Kelvaroq and AnyXboxX / 新增完整启动器包

**Kelvaroq:** keep the entire `stellarr-studio/kelvaroq/` directory. `Launch.bat` opens the official GUI; select your game folder and use its automatic hardware profile. Dropping a game path on the BAT invokes `kelvaroq-cli.exe play`. The official portable packager supplies the native engine, SIMD DLL, embedded Python/Capstone/Unicorn reference runtime, notices and exact patched backend sources. It also runs its isolated package smoke tests. Windows .NET Framework 4.8 and compatible drivers/hardware are still required. Do not force `--experimental-render`: upstream does not show that it is faster. Shader/pipeline preloading is integrated into native play, which then opens the game; no standalone frontend precompile command is invented here. The manifest distinguishes the frontend SHA from the upstream-prescribed backend SHA and patched tree.

**Kelvaroq：** 完整保留 `stellarr-studio/kelvaroq/` 中所有文件，双击 `Launch.bat` 打开官方GUI选择游戏目录，默认使用硬件自动预设；拖入游戏路径则走CLI。后端、SIMD库、内嵌Python及依赖都已打包，无需自行装Rust/Python；仍需Windows .NET Framework 4.8和兼容驱动。使用上游固定后端及补丁，不强制未证实更快的experimental-render。预加载集成在正式启动流程中，完成后会进入游戏，不冒充纯离线预编译。构建记录区分前端提交、后端提交和补丁后的源码树。

**AnyXboxX:** keep all of `LooperSalty/AnyXboxX/`. `Launch.bat` asks for the game EXE/folder (or accepts a dropped path) and uses the upstream Windows/NVIDIA tuned profile with the included `victory.exe` through `--victory-exe`; no Cargo invocation or source checkout is needed at runtime. `Open-GUI.bat` optionally opens the included graphical launcher with that same prebuilt backend. The GUI can keep its own settings in AppData. **Shader versus pipeline preload:** behavior follows the exact selected upstream revision. Recent frontends use plain `warm` to prebuild recorded GPU pipelines; that is not CPU translation of every shader, even if the wrapper calls it `--precompile`. Some earlier revisions instead passed unsupported archive arguments to `warm`. Our standalone **Precompile-Shaders.bat** calls the backend's **shaders** command for all inputs first, then **warm** for recorded GPU pipelines, independently of those changing frontend wrappers. It does not patch upstream runtime sources. The standalone script uses the exact source's NVIDIA environment, logical-CPU/free-memory-aware concurrency, local cache and logs; it does **not** start the game. `Record-Pipelines.bat` starts the game only when you need to collect missing scene states. These scripts use the same options and limitations described below for cool_fox. They enumerate shaders.rpf and the game EXE; shaders in other optional DLLs are not claimed as precompiled.

**AnyXboxX：** 包内包含CLI、GUI及同版本内置后端。用 `Launch.bat` 输入/拖入游戏EXE或目录，直接使用官方Windows/NVIDIA性能预设，不在本机重新编译；可选 `Open-GUI.bat` 打开GUI。**注意区分着色器翻译和GPU管线预热：** 上游新版前端使用不带路径的warm预热已记录管线，并不等于翻译全部着色器；更早版本曾传入不支持的路径参数。本包 **Precompile-Shaders.bat** 独立调用后端shaders再调用warm，纯离线处理，不依赖前端封装，也不会打开游戏；之后用 **Launch.bat** 启动即可。脚本与启动预设和缓存一致，按CPU/可用内存控制并发，保留日志；只遍历shaders.rpf和游戏EXE，不声称处理其他可选DLL。缺少GPU管线历史时才用 `Record-Pipelines.bat` 玩相应场景并正常退出。源码因条件编译要求保留diagnostics特性，但编译优化不降级、不会偷偷替换别的后端。

## VIctory-NVIDIA portable preset / NVIDIA专用便携包

Keep the complete **trustno1-coder/VIctory-NVIDIA/** directory. **Launch.bat** prompts for the game EXE, or accepts a dropped EXE/folder, and applies exact-source `scripts/play-nvidia.ps1` speed settings with the documented **-Resolution 540 / Series S** path: **960×540 internal rendering, the title’s temporal upscaler, dynamic descriptor slots and GPU memory comparisons**. Old source revisions retain ray-walk misses and their official ray-pass filter. The renderer update at `63957271cdd1` changed upstream’s default to native-720p DLSS/stretched output and removed that filter: this portable package deliberately selects the still-documented 540p own-upscaler path instead. **The optional dlss feature is not compiled, and separately licensed NVIDIA SDK/runtime files are not included.** New upstream DLSS FPS measurements do not apply to this configuration. Diagnostics retains source-generation-specific runtime controls, without reducing compiler optimization. Its long hash list is verified and loaded inside PowerShell, not through a command exceeding cmd.exe’s line limit. Physical RAM below 24 GiB gets the upstream 3072 MiB CPU-mapped VRAM window cap; larger PCs leave it off.

完整保留 **trustno1-coder/VIctory-NVIDIA/**，双击 **Launch.bat** 输入游戏EXE路径，或把EXE/目录拖上去。提取同一源码的NVIDIA速度参数，并明确选择官方仍支持的 **-Resolution 540 / Series S自带升频** 路径。旧版保留官方光追过滤；`63957271cdd1` 起上游默认改为原生720p+DLSS/拉伸，本便携包仍选540p路径，**不编译dlss可选特性、不打包另行授权的NVIDIA SDK/DLL**，不能套用上游DLSS帧率数据。低于24 GiB物理内存时沿用上游3072 MiB窗口上限；更大内存保持关闭。哈希列表在PowerShell子进程里加载，避免BAT长命令被截断。

**Image-quality tradeoff:** the upstream speed preset can still show stretched/jumping bodies or rainbow skin. `Launch.bat -Quality` selects the exact source’s source’s slower quality overrides. Other options: `-Resolution 480`, `-Resolution 720`, `-Adapter "5070"`, `-WindowsBarMB 0`, `-NoSmoothMotion`. The default `victory_sm.exe` alias is copied/refreshed only at real launch, like upstream. **It does not automatically enable Smooth Motion**; configure that profile in NVIDIA App yourself, or disable the alias. Generated frames are not the game’s native rendering FPS. `Launch.bat --print-runtime-preset` and `Launch.bat -DryRun -NoPause` only inspect settings; neither opens the game.

**画质取舍：** 速度模式仍可能有人体拉伸/跳动、彩虹皮肤等上游问题；可用 `Launch.bat -Quality` 应用官方较慢的画质修正。可选480p/720p、显卡选择或内存窗口覆盖。默认别名 `victory_sm.exe` 仅在真正启动时生成；**改名本身不会开启Smooth Motion**，需自行配置NVIDIA App，或加 `-NoSmoothMotion`。插帧不等于原生帧率。本项目未实测游戏表现，不保证稳定30帧、无渲染错误或无崩溃。

**Precompile-Shaders.bat** is offline: CPU-translate shaders found in `shaders.rpf` and the game EXE, then GPU-warm all recorded pipelines, using the same default NVIDIA settings and local cache as Launch.bat. It does not start the game. Workers scale with CPU count and free RAM (GPU workers capped at 8). No history yet? Run **Record-Pipelines.bat** deliberately, visit desired scenes, exit normally, then rerun precompile. Exit 2 means partial shader/pipeline failure, not complete success. Existing shader and pipeline limitations still apply; no game-derived caches are published. If you change driver, translator or runtime options, warm the matching configuration again.

**Precompile-Shaders.bat** 只做离线编译：先处理shaders.rpf和游戏EXE里的着色器，再预热已记录GPU管线，**不会打开游戏**。与默认启动使用同一NVIDIA预设和缓存，按CPU/可用内存控制并发。没有管线历史时才使用 **Record-Pipelines.bat** 进入需要的场景、正常退出后再预编译。部分失败返回2并保留日志；无法提前生成未记录的管线，也不分发游戏衍生缓存。更换驱动、翻译器或运行配置后请重新预热对应配置。
