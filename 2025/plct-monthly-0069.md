# PLCT 开源进展·第 69 期·2025 年 5 月 1 日（4月汇总）

## 卷首语

工作节奏逐渐开始紧凑的一个月。RuyiSDK 项目再经过了2年的内部迭代整合之后，开始逐步试水向RISC-V开发者推广。PLCT实验室预测我们很快就会迎来超过100万RISC-V开发者，目前业界正处于一个几何级数增长的早期阶段，机会难得。

## 本期亮点

PLCT实验室的亮点产出通过「RuyiSDK」微信公众号（ID：RuyiSDK）和英文网站提供更新服务：

- https://plctlab.org/en/news/

为了减少冗余工作量，已经在微信公众号推送的中文信息默认不再搬运。

## RuyiSDK IDE
1. RuyiSDK IDE 插件开发：
   - 插件工程调整及规范化：
      - 将插件仓库迁移到ruyisdk-eclipse-plugins（https://github.com/ruyisdk/ruyisdk-eclipse-plugins） ，并对工程目录组织进行了重构和优化:https://github.com/ruyisdk/ruyisdk-eclipse-plugins/blob/main/README.md
      - 重构插件组织，基础类调整到 org.ruyisdk.core 包下，作为其它插件的依赖项；并按照XDG规范创建ruyisdkide的本地配置、缓存、数据文件目录；提供基础类，实现XDG规范目录的设置和路径获取；自定义ruyisdk console 用于ruyisdk插件执行中的重要 Info、Error；
      - 重构和优化设备管理，调整配置文件到 ~/.config/ruyisdkide 下，并将UI从继承 viewpart 实现修改为 PreferencePage，实现了通过 Windows > preferences > Device Manage 进行管理，更加符合使用场景。
   - 新增 ruyi 包管理器插件，实现ruyi包管理器基础类，主要包含以下功能：
      - 环境检测：本地是否安装ruyi、版本检查；
      - 安装/更新：安装ruyi、更新ruyi；
      - ruyi配置：安装路径配置、软件源配置、遥测设置与上传等等；
2. RuyiSDK IDE 正在招聘插件开发实习生，详情参考 [J159 RuyiSDK IDE 开发实习生](https://github.com/lazyparser/weloveinterns/blob/master/open-internships.md) ，欢迎有兴趣的小伙伴加入。

## RuyiSDK 包管理器

项目地址：https://github.com/ruyisdk/ruyi

4 月份 RuyiSDK 包管理器发布了 2 个新版本：0.31.0、0.32.0，对应
RuyiSDK 整体的 0.31 与 0.32 两个正式版本。您可移步
[GitHub Releases][GitHub Releases] 或 [ISCAS 镜像源][iscas]下载体验。

[GitHub Releases]: https://github.com/ruyisdk/ruyi/releases/tag/0.32.0
[iscas]: https://mirror.iscas.ac.cn/ruyisdk/ruyi/tags/0.32.0/

本月的主要变更如下：

Release Engineering 与工程化方面：

* 为流程合规，现在要求所有向 RuyiSDK 发起的拉取请求（Pull Requests）都包含开发者原创声明（Developer's Certificate of Origin, DCO）了。
* 新增了贡献指南文档，使社区成员更容易参与项目贡献。
* 为避免 CI 物理机资源的临时下线等原因影响到发版，将 RISC-V 架构的构建任务也迁移至 GitHub Actions 公开免费提供的实例上了。
* 将构建 `ruyi` 的单文件分发版本所用的 Python 版本升级到了 3.13.2。

`ruyi` 包管理器命令行工具方面：

* RuyiSDK 设备安装器在需要安装的软件包不止一个可用版本的时候，会额外允许您选择一个非默认（最新）的版本了。如果您的开发板需要旧版本的系统，该功能对您可能有所帮助。
* 新增了实验模式开关：环境变量 `RUYI_EXPERIMENTAL`，用于启用一些我们暂不承诺兼容性的实验性功能。
* 新增了实验性功能：实体数据库。
    * 该功能有助于 RuyiSDK 为您提供与手头设备关联的有用信息。例如，可用 `ruyi list --related-to-entity device:sipeed-lpi4a` 查询适用于 Sipeed LicheePi 4A 的各种软件包了。
    * 初期支持 CPU 微架构（如香山南湖、玄铁 C910 等等）、CPU 型号（如香山南湖、玄铁 TH1520 等等）、设备型号（如 Sipeed LicheePi 4A 等等）、软件包等四种实体类型。
    * 这些功能为预览版，后续不排除作出不兼容变更以适应需求，因此仅在启用实验模式时才可用。如您有使用场景，请保持您的 `ruyi` 与软件源为最新。

RuyiSDK 软件源方面：

* 完善了设备支持：
    * 新增支持了适用于 SpacemiT K1 设备的特殊刷写策略。
    * 修复了设备安装器调用 `fastboot` 失败的问题。
    * 更新了 Sipeed LicheeRV Nano 的 Buildroot SDK 镜像。
    * 更新了 Sipeed LicheePi 4A 的 RevyOS 镜像版本并修复问题。
    * 更新了 Milk-V Meles 的 RevyOS 镜像。
* 重命名了软件包 `board-image/revyos-sg2042-milkv-pioneer` 为 `board-image/revyos-milkv-pioneer` 以符合当前的软件包命名规范。
* 实体数据库更新：
    * 设备实体定义现已与设备安装器支持范围对齐。补充了 Milk-V、Sipeed、Canaan、StarFive、WCH 等厂商的设备定义。
* 更新了 `toolchain/gnu-plct` 与 `toolchain/gnu-upstream` 两种工具链包到 0.20250401.0。
    * 其中，`gnu-plct` 工具链套件提供的软件版本如下：
       * binutils 2.42，PLCT 维护分支
       * gcc 14.1.0，PLCT 维护分支，含 P 扩展与 RV64ILP32 ABI 支持
       * gdb 16.0，PLCT 维护分支
       * glibc 2.40，PLCT 维护分支
       * linux-headers 6.13
    * `gnu-upstream` 工具链套件提供的软件版本如下：
       * binutils 2.43.1
       * gcc 14.2.0
       * gdb 16.2
       * glibc 2.41
       * linux-headers 6.13

RuyiSDK 服务端组件方面：

* 优化了 ruyisdk.org 官网数据统计页面的性能。
* 集成了 GitHub Releases 渠道的下载量到官网数据统计页面的 RuyiSDK 包管理器下载计数中，使统计数据更准确、全面。
* 将原先位于 RuyiSDK 包管理器仓库、需要手工配置生效的镜像源同步脚本迁移到了服务端。

欢迎试用或来上游围观；您的需求是我们迭代开发的目标和动力。您也可以亲自参与
RuyiSDK 软件的打包与分发工作：目前您可以直接在 GitHub 上查看、修改我们的[部分打包脚本](https://github.com/ruyisdk/ruyici)与[软件源仓库](https://github.com/ruyisdk/packages-index)。今后，按照本年度的开发计划，我们也将支持有权的第三方贡献者通过程序化的方式上传软件包、系统镜像等分发文件，以便利打包工作。

## V8
- 修复了三个特殊情况下崩溃的问题
  - 6486191: [riscv] Fix jumptable segment fault | https://chromium-review.googlesource.com/c/v8/v8/+/6486191
  - 6470016: [riscv] Fix segment fault when jit_allocation is nullptr | https://chromium-review.googlesource.com/c/v8/v8/+/6470016
  - 6458881: [riscv] Fix Segmentation fault in WritableRelocInfo::apply | https://chromium-review.googlesource.com/c/v8/v8/+/6458881
- 优化浮点比较时的指令数量
  - 6440384: [riscv] Optimize RiscvCmpD/S code gen | https://chromium-review.googlesource.com/c/v8/v8/+/6440384
  - 6437206: [riscv] Fix float comparison flags and Implement simulator FP handling | https://chromium-review.googlesource.com/c/v8/v8/+/6437206
- 优化临时寄存器的使用，防止冲突
  - 6418607: [riscv] Replace t2 with t6 in RISC-V scratch registers to avoid Maglev scratch conflicts | https://chromium-review.googlesource.com/c/v8/v8/+/6418607

#### Port Upstream
- 6479719: [riscv][csa] Support indexed loads via kRootRegister | https://chromium-review.googlesource.com/c/v8/v8/+/6479719
- 6452749: [riscv]Reland [contexts] Add ContextCells | https://chromium-review.googlesource.com/c/v8/v8/+/6452749
- 6443151: [riscv][maglev|turbolev] Constant-folding around TypedArrays loads and stores | https://chromium-review.googlesource.com/c/v8/v8/+/6443151
- 6436518: [riscv][wasm][jspi] Remove WasmContinuationObjects | https://chromium-review.googlesource.com/c/v8/v8/+/6436518

  
#### Reviw Patch
- 6439219: [riscv] fix scratch registers usage in OptimizeCodeOrTailCallOptimizedCodeSlot | https://chromium-review.googlesource.com/c/v8/v8/+/6439219
- 6401938: [riscv] Increase usage of the MacroAssembler's wrapper functions | https://chromium-review.googlesource.com/c/v8/v8/+/6401938

## Spidermonkey

## OpenJDK
1. Authored/Co-authored JDK-mainline PRs:
- https://github.com/openjdk/jdk/pull/24660 (8354523: runtime/Monitor/SyncOnValueBasedClassTest.java triggers SIGSEGV)
- https://github.com/openjdk/jdk/pull/23723 (8350480: RISC-V: Relax assertion about registers in C2_MacroAssembler::minmax_fp)
- https://github.com/openjdk/jdk/pull/23879 (8351101: RISC-V: C2: Small improvement to MacroAssembler::revb)

2. Proposed JDK-24u mainline PRs:
- https://github.com/openjdk/jdk24u/pull/90 (8346832: runtime/CompressedOops/CompressedCPUSpecificClassSpaceReservation.java fails on RISC-V)

3. Reviewed JDK-mainline PRs:
- https://github.com/openjdk/jdk/pull/23509 (8349632: RISC-V: Add Zfa fminm/fmaxm)
- https://github.com/openjdk/jdk/pull/24047 (8352022: RISC-V: Support Zfa fminm_h/fmaxm_h for float16)
- https://github.com/openjdk/jdk/pull/23890 (8351140: RISC-V: Intrinsify Unsafe::setMemory)
- https://github.com/openjdk/jdk/pull/23844 (8345298: RISC-V: Add riscv backend for Float16 operations - scalar)
- https://github.com/openjdk/jdk/pull/23614 (8349908: RISC-V: C2 SelectFromTwoVector)
- https://github.com/openjdk/jdk/pull/23580 (8321003: RISC-V: C2 MulReductionVI)
- https://github.com/openjdk/jdk/pull/23171 (8347981: RISC-V: Add Zfa zli imm loads)
- https://github.com/openjdk/jdk/pull/23633 (8350095: RISC-V: Refactor string_compare)
- https://github.com/openjdk/jdk/pull/23793 (8350723: RISC-V: debug.cpp help() is missing riscv line for pns)
- https://github.com/openjdk/jdk/pull/23821 (8350855: RISC-V: print offset by assert of patch_offset_in_conditional_branch)
- https://github.com/openjdk/jdk/pull/23842 (8350940: RISC-V: remove unnecessary assert_different_registers in minmax_fp)
- https://github.com/openjdk/jdk/pull/23839 (8350931: RISC-V: remove unnecessary src register for fp_sqrt_d/f)
- https://github.com/openjdk/jdk/pull/23856 (8351033: RISC-V: TestFloat16ScalarOperations asserts with offset (4210) is too large to be patched in one beq/bge/bgeu/blt/bltu/bne instruction!)
- https://github.com/openjdk/jdk/pull/23903 (8351145: RISC-V: only enable some crypto intrinsic when AvoidUnalignedAccess == false)
- https://github.com/openjdk/jdk/pull/23705 (8350383: Test: add more test case for string compare (UL case))
- https://github.com/openjdk/jdk/pull/23766 (8350642: Interpreter: Upgrade CountBytecodes to 64 bit on 64 bit platforms)

## Go
1. Authored/Co-authored Go-mainline CLs:
- 647596: runtime: unify C -> Go ABI transitions on riscv64 | https://go-review.googlesource.com/c/go/+/647596
- all: add race support for riscv64 | https://github.com/mengzhuo/go/commit/a1b9b0d4faae07a31c599e00ee73aa6b4f882068 
- https://github.com/golang/go/issues/64345
- 659175: cmd/link: generate proper attributes for riscv profile | https://go-review.googlesource.com/c/go/+/659175
- 657036: internal/bytealg: vector implementation of count 1 byte for riscv64 | https://go-review.googlesource.com/c/go/+/657036
- 663778: cmd/asm, cmd/internal/obj: add zvbb/zvbc/zvkb for riscv64 | https://go-review.googlesource.com/c/go/+/663778
- 664155: cmd/asm, cmd/internal/obj: add crypto algorithm suites for riscv64 | https://go-review.googlesource.com/c/go/+/664155
- 664375: cpu: add crypto extensions detection for riscv64 | https://go-review.googlesource.com/c/sys/+/664375
- 663675: cmd/internal/obj: add crypto extension for riscv64 | https://go-review.googlesource.com/c/go/+/663675

3. Reviewed Go-mainline CLs:
- 631937: cmd/internal/obj/riscv: implement vector load/store instructions | https://go-review.googlesource.com/c/go/+/631937 
- 646775: cmd/internal/obj/riscv: add support for vector integer arithmetic instructions | https://go-review.googlesource.com/c/go/+/646775 
- 646736: internal/bytealg: vector implementation of equal for riscv64 | https://go-review.googlesource.com/c/go/+/646736
- 646737: internal/bytealg: vector implementation of compare for riscv64 | https://go-review.googlesource.com/c/go/+/646737
- 646777: cmd/internal/obj/riscv: add support for vector floating-point instructions | https://go-review.googlesource.com/c/go/+/646777
- 646776: cmd/internal/obj/riscv: add support for vector fixed-point arithmetic instructions | https://go-review.googlesource.com/c/go/+/646776 
- 660855: cmd/compile: intrinsify math/bits.Bswap on riscv64 | https://go-review.googlesource.com/c/go/+/660855
- 652717: doc, cmd/internal/obj/riscv: document the riscv64 assembler | https://go-review.googlesource.com/c/go/+/652717 
- 637317: cmd/internal/obj/riscv: fix the encoding for REV8 and ORCB | https://go-review.googlesource.com/c/go/+/637317
- 660856: cmd/compile,internal/cpu,runtime: intrinsify math/bits.OnesCount on riscv64 | https://go-review.googlesource.com/c/go/+/660856 

## OpenCV

## GNU Toolchain

更新了P扩展的草案支持
- https://github.com/ruyisdk/riscv-binutils/commit/19d93c476d210b11060398992b99fd7f9688cc70
- https://github.com/ruyisdk/riscv-binutils/commit/389787869209f415b0628603559cae16e69bca1b

提交了玄铁系列CPU的-mcpu选项的gcc支持，包括c908,c910,c920
- https://patchwork.sourceware.org/project/gcc/patch/20250420155606.924642-1-chenyixuan@iscas.ac.cn/

提交了Sdtrig, Ssstrict, Zama16b扩展的最小支持
- https://patchwork.sourceware.org/project/gcc/patch/20250418084737.653028-1-chendongyan@isrc.iscas.ac.cn/
- https://patchwork.sourceware.org/project/gcc/patch/20250418084627.652743-1-chendongyan@isrc.iscas.ac.cn/

Xsfvcp扩展已合入gcc上游
- https://gcc.gnu.org/git/?p=gcc.git;a=commit;h=37a6fbe652220dbb8aa38afd20443639a97bbd2f
- https://gcc.gnu.org/git/?p=gcc.git;a=commit;h=cc8b8c0b69200ab816a2626e29d91ac995f7438f

更新了TARGET_VERSION的实现
- https://patchwork.sourceware.org/project/gcc/patch/tencent_65DA478EF6B8F1312A21258DA57C07825E08@qq.com/
- https://patchwork.sourceware.org/project/gcc/patch/tencent_5388B68563092E498C7E0848E3089FBCA006@qq.com/

修复了回归测试中发现的一些问题
- https://patchwork.sourceware.org/project/gcc/patch/20250508080102.1340059-1-jiawei@iscas.ac.cn/

## LLVM Team
Upstream 已经合并了的patch:
- [SelectionDAG][X86] Fold sub(x, mul(divrem(x,y)[0], y)) to divrem(x, y)[1] https://github.com/llvm/llvm-project/pull/136565 issue https://github.com/llvm/llvm-project/issues/51823
- [InstCombine] Canonicalize max(min(X, MinC), MaxC) -> min(max(X, MaxC), MinC) https://github.com/llvm/llvm-project/pull/136665 issue https://github.com/llvm/llvm-project/issues/121870
- [ConstraintElim] Fix poison check before adding intrinsic facts https://github.com/llvm/llvm-project/pull/136291
- [CIR] Upstream initial support for union type https://github.com/llvm/llvm-project/pull/137501 issue https://github.com/llvm/llvm-project/issues/136059
- [[DeadArgElim] fix verifier failure when changing musttail's function signature](https://github.com/llvm/llvm-project/pull/127366)
- [[flang][docs] Fix typo in array description](https://github.com/llvm/llvm-project/pull/135908)
- [RISCV][NFC] Delete RISCVAsmParser::parsePseudoQCJumpSymbol https://github.com/llvm/llvm-project/pull/136552
- [RISCV] Add smcntrpmf extension https://github.com/llvm/llvm-project/pull/136556

ruyisdk 中已经合并的patch:
- [[Clang][XTHeadVector] Add more semacheck for xtheadvector](https://github.com/ruyisdk/llvm-project/pull/150)
- [[Rebase 19.1.6] Rebase more changes from 17.1.6 since last update](https://github.com/ruyisdk/llvm-project/pull/151)
- [[Clang][XTHeadVector] Fix vector integer minmax intrinsics and add wrappers](https://github.com/ruyisdk/llvm-project/pull/152)
- [[Clang][XTHeadVector] fix vmulhsu/vmulhu wrappers and tests](https://github.com/ruyisdk/llvm-project/pull/153)

## MLIR

### Buddy Compiler

**buddy-mlir**

**buddy-benchmark**


## CIRCT
### Moore方言

## Box64

- ksco

  - [\[RV64_DYNAREC\] Minor optim to 8 bit TEST opcode](https://github.com/ptitSeb/box64/pull/2583)
  - [\[RV64_DYNAREC\] Small optimization to LEA opcode](https://github.com/ptitSeb/box64/pull/2582)
  - [\[RV64_DYNAREC\] Optimized rv64 printer for pseudo and jump instructions](https://github.com/ptitSeb/box64/pull/2581)
  - [Show Dynarec architecture in version string](https://github.com/ptitSeb/box64/pull/2580)
  - [\[RV64_DYNAREC\] Minor adjustment to dynarec_missing=2](https://github.com/ptitSeb/box64/pull/2578)
  - [\[SIGNAL\] Better signal logging when trace enabled](https://github.com/ptitSeb/box64/pull/2572)
  - [\[RV64_DYNAREC\] Fixed x87 cache swapping](https://github.com/ptitSeb/box64/pull/2571)
  - [\[DYNAREC\] Added ranged Dynablock dump](https://github.com/ptitSeb/box64/pull/2570)
  - [\[ARM64_DYNAREC\] Minor optim to MOVNTDQA](https://github.com/ptitSeb/box64/pull/2568)
  - [Added some missing newlines](https://github.com/ptitSeb/box64/pull/2567)
  - [\[DYNAREC\] Added a x87pc test and some cosmetic changes too](https://github.com/ptitSeb/box64/pull/2561)
  - [\[RV64_DYNAREC\] Better handling of x87double=2](https://github.com/ptitSeb/box64/pull/2560)
  - [\[DYNAREC\] More handling of low precision x87 flag change](https://github.com/ptitSeb/box64/pull/2556)
  - [\[RV64_DYNAREC\][TRACE][COSIM] Improve x87 fiability in dynarec trace and cosim scenario](https://github.com/ptitSeb/box64/pull/2555)
  - [\[ENV\][COSIM] Enable x87double only if it's off](https://github.com/ptitSeb/box64/pull/2554)
  - [\[RV64_DYNAREC\] Added X87DOUBLE=2 support](https://github.com/ptitSeb/box64/pull/2553)
  - [\[WOW64\] Finished skeleton code for PE build](https://github.com/ptitSeb/box64/pull/2542)
  - [\[WOW64\] More tweaks to CMake PE build](https://github.com/ptitSeb/box64/pull/2541)
  - [\[WOW64\] Added PE build to the build system](https://github.com/ptitSeb/box64/pull/2537)
  - [Eliminated many compilation warnings](https://github.com/ptitSeb/box64/pull/2535)
  - [\[WOW64\] Added non-functional PE build](https://github.com/ptitSeb/box64/pull/2532)
  - [\[WOW64\] More tweaks for PE build](https://github.com/ptitSeb/box64/pull/2528)
  - [\[WOW64\] Splitted freq and cleanup functions from x64emu](https://github.com/ptitSeb/box64/pull/2521)
  - [\[WOW64\] More tweaks towards PE build](https://github.com/ptitSeb/box64/pull/2519)
  - [\[WOW64\] More work on the PE wow64 build](https://github.com/ptitSeb/box64/pull/2518)
  - [Made custommem OS-independent](https://github.com/ptitSeb/box64/pull/2517)
  - [Moved emit functions to seperate files from signals.h](https://github.com/ptitSeb/box64/pull/2516)
  - [Added backtrace.h for holding backtrace-related functions](https://github.com/ptitSeb/box64/pull/2515)
  - [\[ARM64_DYNAREC\] Fixed some dangling else warnings](https://github.com/ptitSeb/box64/pull/2514)
  - [\[WOW64\] Add wow64 PE build scaffolding](https://github.com/ptitSeb/box64/pull/2513)
  - [Decoupled alternate functions from bridge](https://github.com/ptitSeb/box64/pull/2500)
  - [\[INTERP\] Fixed a nan propagation issue on RISC-V](https://github.com/ptitSeb/box64/pull/2498)
  - [Moved more functions to os.h](https://github.com/ptitSeb/box64/pull/2497)
  - [\[CI\] Enable cppThreads_32bits in the CI](https://github.com/ptitSeb/box64/pull/2496)
  - [Moved some emit functions to os.h](https://github.com/ptitSeb/box64/pull/2494)
  - [Moved more OS-dependent functions to os.h](https://github.com/ptitSeb/box64/pull/2491)
  - [Introduced box64cpu.h for exported interpreter and dynarec functions](https://github.com/ptitSeb/box64/pull/2490)
  - [Added os.h for future usage](https://github.com/ptitSeb/box64/pull/2488)
  - [Some cosmetic changes to C header files](https://github.com/ptitSeb/box64/pull/2487)

### Box64 RISC-V Guide Website

- rurumuri

  - PRs

    - [Add benchmark document for dav1d](https://github.com/ksco/box64-for-riscv-guide/pull/36)
    - [Add compatibility report document for "Mutazione", "Party Hard", "Tales of MajEyal", "Turok" and "Zombie Night Terror"](https://github.com/ksco/box64-for-riscv-guide/pull/37)
    - [Add compatibility report documents for "Train Fever", "Lila’s Sky Ark" and "Growbot"](https://github.com/ksco/box64-for-riscv-guide/pull/40)
    - [Add compatibility report document for "5D Chess With Multiverse Time Travel", "Faster Than Light", "Phoenotopia: Awakening", "Pizza Tower", "Post Void", "Transport Fever", "Typoman" and "Virtual Circuit Board"](https://github.com/ksco/box64-for-riscv-guide/pull/44)
    - [Add compatibility report documents for "Monster Outbreak", "Neon Abyss", "Rainswept", "Train Valley", "Return of the Obra Dinn", "Swords and Sandals Immortals" and "The Shrouded Isle"](https://github.com/ksco/box64-for-riscv-guide/)

  - Issues

    - ["FarmTogether" corrupted when running](https://github.com/ksco/box64-for-riscv-guide/issues/34)
    - ["Super Collapse 4" unable to launch](https://github.com/ksco/box64-for-riscv-guide/issues/35)
    - ["Star Ruler 2" corrupted when running](https://github.com/ksco/box64-for-riscv-guide/issues/38)
    - ["The Darkside Detective: A Fumble in the Dark" unable to launch](https://github.com/ksco/box64-for-riscv-guide/issues/39)
    - ["Saint Kotar" unable to launch](https://github.com/ksco/box64-for-riscv-guide/issues/41)
    - ["Song of Farca" unable to launch](https://github.com/ksco/box64-for-riscv-guide/issues/42)
    - ["The Colonists" corrupted when running](https://github.com/ksco/box64-for-riscv-guide/issues/43)
    - ["while True learn()" unable to launch](https://github.com/ksco/box64-for-riscv-guide/issues/45)
    - ["down in bermuda" corrupt](https://github.com/ksco/box64-for-riscv-guide/issues/46)
    - ["Untitled Goose Game" corrupt when running](https://github.com/ksco/box64-for-riscv-guide/issues/47)

## DynamoRIO

- ziyao233

  - [i#3544 RV64: Port client.flush to riscv64](https://github.com/DynamoRIO/dynamorio/pull/7438)
  - [i#3544 RV64: Implement relink_special_ibl_xfer()](https://github.com/DynamoRIO/dynamorio/pull/7437)

## coreboot for riscv

本期没有新的进展。

## openocd

本期没有新的进展。

## opensbi

- 更新sbi\_timer，添加xtheadsstc支持。[1](https://lists.infradead.org/pipermail/opensbi/2025-April/008318.html)
- 添加宏优化64位CSR访问，并添加sstateen扩展检测，初始化mstateen/sstateen/hstateen防止一些安全问题。[1](https://lists.infradead.org/pipermail/opensbi/2025-April/008402.html)
- 为了节能，在sbi\_hsm\_hart\_wait中调用hsm\_device\_hart\_stop。[1](https://lists.infradead.org/pipermail/opensbi/2025-April/008339.html)
- 添加cache设备驱动刷新整个缓存。[1](https://lists.infradead.org/pipermail/opensbi/2025-April/008379.html)
- 始终解析每个aplic设备的msi信息。[1](https://lists.infradead.org/pipermail/opensbi/2025-April/008372.html)
- 为了调试添加编译器选项以保留CFI(调用帧信息)。[1](https://lists.infradead.org/pipermail/opensbi/2025-April/008355.html)
- 扩展Smrnmi存在时使能可恢复的不可屏蔽中断。[1](https://lists.infradead.org/pipermail/opensbi/2025-April/008387.html)
- 给fdt\_mpxy\_init添加返回值，返回错误信息。[1](https://lists.infradead.org/pipermail/opensbi/2025-April/008422.html)

## u-boot

本期没有新的进展。

## The Aya Theorem Prover

## Arch Linux

## RevyOS (Debian for Xuantie)

### Debian (Bo YU)

本月主要工作：
1. riscv 方面, 维护 Debian ci 基础设施，及时响应社区关切; 修复一些包在 riscv64 上构建失败的问题
2. review 新包并上传

以下是外部可见links:

- https://tracker.debian.org/news/1636447/accepted-tox-current-env-0016-1-source-into-unstable/ [new upload]
- https://salsa.debian.org/pkg-llvm-team/llvm-toolchain/-/merge_requests/164 [llvm MR]
- https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1102444#10 [sponsored upload selint]
- https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1102540 [ironic: flaky riscv64 autopkgtest]
- https://www.eclipse.org/lists/epp-dev/msg06982.html [test +1 for eclipse]
- https://tracker.debian.org/news/1639516/accepted-ompl-160ds1-3exp1-source-amd64-all-into-experimental/ [new upload]
- https://lists.debian.org/debian-ci/2025/04/msg00002.html [debci issue]
- https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1103395#36 [ananta uploaded]
- https://tracker.debian.org/news/1640833/accepted-ananta-114-1-source-into-unstable/ [ananta new upload]
- https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1103968 [ananta reportbug]
- https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1104037 [iovisor-gobpf patch]
- https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1104063 [mercurial rv64]

### RedleafOS
本月无外部可见更新.

### 桂香伟
### debian package patches
r-cran-rcppparallel: https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1101729  
jsonnet: https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1102655  
ltt-control: https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1103229  
rust-ring: https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1103300  
mayo: https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1104187  
## Nixpkgs

## Fedora

## RT-Thread

RTT upstream：

- [bsp: k230: use rttpkgtool to package kernel][rtt-10236]
- [bsp: k230: support canmv-k230][rtt-10221]
- [bsp: cvitek: riscv: use marco for linker script][rtt-10202]
- [doxygen: fix one typo issue][rtt-10200]
- [doxygen: re-org module groups][rtt-10197]
- [doxygen: cleanup and re-org files][rtt-10183]
- [libcpu: riscv: declare external symbols inside][rtt-10181]
- [libcpu: cleanup undefined rt_hw_mmu_kernel_map_init][rtt-10177]
- [[DM/FDT] Fix garble when booting][rtt-10166]

[rtt-10236]:https://github.com/RT-Thread/rt-thread/pull/10236
[rtt-10221]:https://github.com/RT-Thread/rt-thread/pull/10221
[rtt-10202]:https://github.com/RT-Thread/rt-thread/pull/10202
[rtt-10200]:https://github.com/RT-Thread/rt-thread/pull/10200
[rtt-10197]:https://github.com/RT-Thread/rt-thread/pull/10197
[rtt-10183]:https://github.com/RT-Thread/rt-thread/pull/10183
[rtt-10181]:https://github.com/RT-Thread/rt-thread/pull/10181
[rtt-10177]:https://github.com/RT-Thread/rt-thread/pull/10177
[rtt-10166]:https://github.com/RT-Thread/rt-thread/pull/10166)

RTT Package Tool:

- [Support package kernel for canmv-k230][rttpkgtool-8]

[rttpkgtool-8]:https://github.com/plctlab/rttpkgtool/pull/8

RTT mlibc:

- [fix: correct return value in __mlibc_read function][mlibc-65]
- [enhance : update toolchain variables][mlibc-64]
- [fix : relocate QEMU example binaries to build directory][mlibc-63]

[mlibc-65]:https://github.com/plctlab/mlibc/pull/65
[mlibc-64]:https://github.com/plctlab/mlibc/pull/64
[mlibc-63]:https://github.com/plctlab/mlibc/pull/63

## PLCT罗云翔测试团队

（包含 SAIL 和 ACT 测试部分）

1. 完成RuyiSDK v0.32、v0.31多平台测试，产出测试报告
2. 操作系统支持矩阵新增了HiFive Premier P550、ESP32 RISC-V系列MCU开发板支持，openCloudOS首次进入操作系统支持矩阵
3. 操作系统支持矩阵Web UI功能增强，https://matrix.ruyisdk.org
4. Sail-RISCV提交了Zilsd/Zclsd指令集支持等23个PR
5. 入选2025年RISC-V欧洲峰会的两篇海报已完成修改后提交 

### 1. RuyiSDK

#### 1.1 RuyiSDK测试

- [RuyiSDK 测试策略和测试报告](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/README.md)

  - [RuyiSDK v0.32测试报告](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250422/README.md)

    - [SG2042 Pioneer Fedora 38 测试报告](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250422/RUYI_包管理_Pioneer_Box_Fedora38_riscv64_测试结果.)
    
    - [SG2042 Pioneer openEuler 23.09 测试报告](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250422/RUYI_包管理_Pioneer_Box_openEuler23.09_riscv64_测试结果.md)

    - [SG2042 Pioneer openEuler 24.03 测试报告](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250422/RUYI_包管理_Pioneer_Box_openEuler24.03_riscv64_测试结果.md)

    - [LPi4A openEuler 23.09 测试报告](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250422/RUYI_包管理_LicheePi4A_openEuler23.09_riscv64_测试结果.md)

    - [LPi4A openEuler 24.03 测试报告](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250422/RUYI_包管理_LicheePi4A_openEuler24.03_riscv64_测试结果.md)

    - [LPi4A RevyOS openEuler 23.09 测试报告](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250422/RUYI_包管理_LicheePi4A_RevyOS_riscv64_测试结果.md)
    
    - [Container RevyOS riscv64 测试结果](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250422/RUYI_包管理_Container_RevyOS_riscv64_测试结果.md)
    
    - [Container Archlinux riscv64 测试结果](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250422/RUYI_包管理_Container_Archlinux_riscv64_测试结果.md)
    
    - [Container Archlinux x86\_64 测试结果](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250422/RUYI_包管理_Container_Archlinux_x86_64_测试结果.md)
    
    - [Container Debian sid riscv64 测试结果](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250422/RUYI_包管理_Container_Debiansid_riscv64_测试结果.md)
    
    - [QEMU Fedora39 x86\_64 测试报告](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250422/RUYI_包管理_QEMU_Fedora39_x86_64_测试结果.md)
    
    - [QEMU Fedora39 x86\_64 测试报告](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250422/RUYI_包管理_QEMU_Fedora39_x86_64_测试结果.md)
    
    - [QEMU Fedora39 x86\_64 测试报告](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250422/RUYI_包管理_QEMU_Fedora39_x86_64_测试结果.md)
    
    - [QEMU Fedora38 riscv64 测试结果](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250422/RUYI_包管理_QEMU_Fedora38_riscv64_测试结果.md)
    
    - [QEMU Ubuntu22.04 x86\_64 测试报告](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250422/RUYI_包管理_QEMU_Ubuntu22.04_x86_64_测试结果.md)
    
    - [QEMU Ubuntu22.04 riscv64 测试结果](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250422/RUYI_包管理_QEMU_Ubuntu22.04_riscv64_测试结果.md)
    
    - [QEMU Ubuntu24.04 x86\_64 测试报告](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250422/RUYI_包管理_QEMU_Ubuntu24.04_x86_64_测试结果.md)
    
    - [QEMU Ubuntu24.04 riscv64 测试结果](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250422/RUYI_包管理_QEMU_Ubuntu24.04_riscv64_测试结果.md)
    
    - [QEMU openEuler23.09 riscv64 测试报告](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250422/RUYI_包管理_QEMU_openEuler23.09_riscv64_测试结果.md)
    
    - [QEMU openEuler23.09 x86\_64 测试结果](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250422/RUYI_包管理_QEMU_openEuler23.09_x86_64_测试结果.md)
    
    - [QEMU openEuler24.03 riscv64 测试报告](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250422/RUYI_包管理_QEMU_openEuler24.03_riscv64_测试结果.md)
    
    - [QEMU openEuler24.03 x86\_64 测试结果](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250422/RUYI_包管理_QEMU_openEuler24.03_x86_64_测试结果.md)
    
    - [QEMU Debian12 aarch64 测试结果](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250422/RUYI_包管理_QEMU_Debian12_aarch64_测试结果.md)
    
    - [QEMU Debian12 x86\_64 测试结果](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250422/RUYI_包管理_QEMU_Debian12_x86_64_测试结果.md)
    
    - [QEMU Gentoo Linux x86\_64 测试结果](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250422/RUYI_包管理_QEMU_Gentoo_x86_64_测试结果.md)
  
    - 上一版本依据 packages-index 与 support-matrix 相关测试提出的 issue 回归测试发现还有一部分 packages-index 的 pr 仍在审核中。本版本新增 2 个 issue：

      - [ruyi device provision 调用 fastboot 镜像刷写失败](https://github.com/ruyisdk/ruyi/issues/291)（稳定版中已修复）
      - [[Page Content] Duo FreeRTOS missing test report for duo-buildroot-sdk-v2](https://github.com/ruyisdk/support-matrix/issues/280)
      - Test FreeRTOS on Milk-V Duo 64MB based on Buildroot SDK v2 <https://github.com/ruyisdk/support-matrix/issues/280> (Completed)
        - MailBox demo doesn't work, it seems that there is no relevant Linux driver in the SDK V2.
        - After the Linux kernel starts up, both the IO control and serial port output of the small-core fail to work properly.
        - During the U-Boot stage, the RTOS can still work properly.

    - 资源：
      - [本版本 litester 测试程序](https://github.com/weilinfox/ruyi-litester/tree/bbd4fb24b11802f1079c99680338daa1a09eb1dc)
      - [本版本自动化调度程序](https://github.com/weilinfox/ruyi-reimu/tree/ce05926b26fe13e6a5ce5d20897309a55a211108)
      - 
  - [RuyiSDK v0.31测试报告](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250409/README.md)

    - [SG2042 Pioneer Fedora 38 测试报告](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250409/RUYI_包管理_Pioneer_Box_Fedora38_riscv64_测试结果.)
    
    - [SG2042 Pioneer openEuler 23.09 测试报告](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250409/RUYI_包管理_Pioneer_Box_openEuler23.09_riscv64_测试结果.md)

    - [SG2042 Pioneer openEuler 24.03 测试报告](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250409/RUYI_包管理_Pioneer_Box_openEuler24.03_riscv64_测试结果.md)

    - [LPi4A openEuler 23.09 测试报告](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250409/RUYI_包管理_LicheePi4A_openEuler23.09_riscv64_测试结果.md)

    - [LPi4A openEuler 24.03 测试报告](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250409/RUYI_包管理_LicheePi4A_openEuler24.03_riscv64_测试结果.md)

    - [LPi4A RevyOS openEuler 23.09 测试报告](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250409/RUYI_包管理_LicheePi4A_RevyOS_riscv64_测试结果.md)
    
    - [Container RevyOS riscv64 测试结果](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250409/RUYI_包管理_Container_RevyOS_riscv64_测试结果.md)
    
    - [Container Archlinux riscv64 测试结果](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250409/RUYI_包管理_Container_Archlinux_riscv64_测试结果.md)
    
    - [Container Archlinux x86\_64 测试结果](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250409/RUYI_包管理_Container_Archlinux_x86_64_测试结果.md)
    
    - [Container Debian sid riscv64 测试结果](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250409/RUYI_包管理_Container_Debiansid_riscv64_测试结果.md)
    
    - [QEMU Fedora39 x86\_64 测试报告](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250409/RUYI_包管理_QEMU_Fedora39_x86_64_测试结果.md)
    
    - [QEMU Fedora39 x86\_64 测试报告](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250409/RUYI_包管理_QEMU_Fedora39_x86_64_测试结果.md)
    
    - [QEMU Fedora39 x86\_64 测试报告](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250409/RUYI_包管理_QEMU_Fedora39_x86_64_测试结果.md)
    
    - [QEMU Fedora38 riscv64 测试结果](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250409/RUYI_包管理_QEMU_Fedora38_riscv64_测试结果.md)
    
    - [QEMU Ubuntu22.04 x86\_64 测试报告](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250409/RUYI_包管理_QEMU_Ubuntu22.04_x86_64_测试结果.md)
    
    - [QEMU Ubuntu22.04 riscv64 测试结果](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250409/RUYI_包管理_QEMU_Ubuntu22.04_riscv64_测试结果.md)
    
    - [QEMU Ubuntu24.04 x86\_64 测试报告](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250409/RUYI_包管理_QEMU_Ubuntu24.04_x86_64_测试结果.md)
    
    - [QEMU Ubuntu24.04 riscv64 测试结果](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250409/RUYI_包管理_QEMU_Ubuntu24.04_riscv64_测试结果.md)
    
    - [QEMU openEuler23.09 riscv64 测试报告](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250409/RUYI_包管理_QEMU_openEuler23.09_riscv64_测试结果.md)
    
    - [QEMU openEuler23.09 x86\_64 测试结果](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250409/RUYI_包管理_QEMU_openEuler23.09_x86_64_测试结果.md)
    
    - [QEMU openEuler24.03 riscv64 测试报告](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250409/RUYI_包管理_QEMU_openEuler24.03_riscv64_测试结果.md)
    
    - [QEMU openEuler24.03 x86\_64 测试结果](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250409/RUYI_包管理_QEMU_openEuler24.03_x86_64_测试结果.md)
    
    - [QEMU Debian12 aarch64 测试结果](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250409/RUYI_包管理_QEMU_Debian12_aarch64_测试结果.md)
    
    - [QEMU Debian12 x86\_64 测试结果](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250409/RUYI_包管理_QEMU_Debian12_x86_64_测试结果.md)
    
    - [QEMU Gentoo Linux x86\_64 测试结果](https://gitee.com/yunxiangluo/ruyisdk-test/blob/master/20250409/RUYI_包管理_QEMU_Gentoo_x86_64_测试结果.md)

    - Ruyi v0.32.0-alpha 新增一个 issue（已修复） [#291](https://github.com/ruyisdk/ruyi/issues/291)

    - 本版本 packages-index 与 support-matrix 相关测试所包含的缺陷已经提交，大部分已经修复：
      - [[packages-index] Unmatched Ubuntu image in packages-index out of sync #218](https://github.com/ruyisdk/support-matrix/issues/218)
      - [[packages-index] Unmatched FreeBSD image link in test report is broken #219](https://github.com/ruyisdk/support-matrix/issues/219)
      - [[packages-index] BPI-F3 Bianbu image out of date with upstream and out of sync with packages-index #220](https://github.com/ruyisdk/support-matrix/issues/220)
      - [[packages-index] Unmatched OpenBSD image link in test report is broken #221](https://github.com/ruyisdk/support-matrix/issues/221)
      - [[packages-index] Star64 Armbian image link in test report is broken #222](https://github.com/ruyisdk/support-matrix/issues/222)
      - [[packages-index] LicheePi4A openEuler image missing innovation version reports and out of sync with packages-index #223](https://github.com/ruyisdk/support-matrix/issues/223)
      - [[packages-index] LicheePi4A RevyOS image out of date with upstream and out of sync with packages-index #224](https://github.com/ruyisdk/support-matrix/issues/224)
      - [[packages-index] LicheeConsole4A RevyOS image out of date with upstream #225](https://github.com/ruyisdk/support-matrix/issues/225)
      - [[packages-index] MilkV Meles RevyOS image out of date with upstream and out of sync with packages-index #226](https://github.com/ruyisdk/support-matrix/issues/226)
      - [[packages-index] Pioneer openEuler image missing innovation version reports and out of sync with packages-index #227](https://github.com/ruyisdk/support-matrix/issues/227)
      - [[packages-index] VisionFive2 openEuler image link in test report is broken and status if it may need update #228](https://github.com/ruyisdk/support-matrix/issues/228)
      - [[packages-index] VisionFive openEuler support status may need update #229](https://github.com/ruyisdk/support-matrix/issues/229)
      - [[packages-index] AWOL Nezha D1 openEuler support status may need update #230](https://github.com/ruyisdk/support-matrix/issues/230)
      - [[packages-index] Unmatched openEuler support status may need update #231](https://github.com/ruyisdk/support-matrix/issues/231)
      - [[packages-index] Unmatched openKylin installation guide link broken and support status may need update #232](https://github.com/ruyisdk/support-matrix/issues/232)
      - [[packages-index] OpenWrt Unmatched image out of date with upstream #233](https://github.com/ruyisdk/support-matrix/issues/233)
      - [[packages-index] BuildRoot LicheeRV_Nano image out of date with upstream #234](https://github.com/ruyisdk/support-matrix/issues/234)
      - [[packages-index] MilkV Duo/Duo256m BuildRoot image syncing is a bit wired #236](https://github.com/ruyisdk/support-matrix/issues/236)

    - 资源

      - [本版本 litester 测试程序](https://github.com/weilinfox/ruyi-litester/tree/bbd4fb24b11802f1079c99680338daa1a09eb1dc)

      - [本版本自动化调度程序](https://github.com/weilinfox/ruyi-reimu/tree/ce05926b26fe13e6a5ce5d20897309a55a211108)
  
  - 依据 v0.29.0 的 packages-index 和 support-matrix 附加测试报告提交 issue
    - [[packages-index] Unmatched Ubuntu image in packages-index out of sync #218](https://github.com/ruyisdk/support-matrix/issues/218)
    - [[packages-index] Unmatched FreeBSD image link in test report is broken #219](https://github.com/ruyisdk/support-matrix/issues/219)
    - [[packages-index] BPI-F3 Bianbu image out of date with upstream and out of sync with packages-index #220](https://github.com/ruyisdk/support-matrix/issues/220)
    - [[packages-index] Unmatched OpenBSD image link in test report is broken #221](https://github.com/ruyisdk/support-matrix/issues/221)
    - [[packages-index] Star64 Armbian image link in test report is broken #222](https://github.com/ruyisdk/support-matrix/issues/222)
    - [[packages-index] LicheePi4A openEuler image missing innovation version reports and out of sync with packages-index #223](https://github.com/ruyisdk/support-matrix/issues/223)
    - [[packages-index] LicheePi4A RevyOS image out of date with upstream and out of sync with packages-index #224](https://github.com/ruyisdk/support-matrix/issues/224)
    - [[packages-index] LicheeConsole4A RevyOS image out of date with upstream #225](https://github.com/ruyisdk/support-matrix/issues/225)
    - [[packages-index] MilkV Meles RevyOS image out of date with upstream and out of sync with packages-index #226](https://github.com/ruyisdk/support-matrix/issues/226)
    - [[packages-index] Pioneer openEuler image missing innovation version reports and out of sync with packages-index #227](https://github.com/ruyisdk/support-matrix/issues/227)
    - [[packages-index] VisionFive2 openEuler image link in test report is broken and status if it may need update #228](https://github.com/ruyisdk/support-matrix/issues/228)
    - [[packages-index] VisionFive openEuler support status may need update #229](https://github.com/ruyisdk/support-matrix/issues/229)
    - [[packages-index] AWOL Nezha D1 openEuler support status may need update #230](https://github.com/ruyisdk/support-matrix/issues/230)
    - [[packages-index] Unmatched openEuler support status may need update #231](https://github.com/ruyisdk/support-matrix/issues/231)
    - [[packages-index] Unmatched openKylin installation guide link broken and support status may need update #232](https://github.com/ruyisdk/support-matrix/issues/232)
    - [[packages-index] OpenWrt Unmatched image out of date with upstream #233](https://github.com/ruyisdk/support-matrix/issues/233)
    - [[packages-index] BuildRoot LicheeRV_Nano image out of date with upstream #234](https://github.com/ruyisdk/support-matrix/issues/234)
    - [[packages-index] MilkV Duo/Duo256m BuildRoot image syncing is a bit wired #236](https://github.com/ruyisdk/support-matrix/issues/236)
    - [#280](https://github.com/ruyisdk/support-matrix/issues/280): Tested FreeRTOS @ Duo (v2)
  - [packages-index 测试报告](https://gitee.com/weilinfox/ruyisdk-test/pulls/4)
    > packages-index 到上游的检查：  
    > packages-index 的最后 commit: 767236: 2025-04-10T15:24:31，0.3 天前  
    > 有 11 个包没有实现检查更新，需要人工开发；  
    > 有 12 个包需要更新；  
    > 有 5 个包出现404错误；  
    > 有 0 个包出现403错误；  
    > 有 12 个包已经最新；  
    > 有 15 个包无需自动检查更新。

    > packages-index 到 support-matrix 的检查  
    > support-matrix 的最后 commit: 503fab: 2025-04-10T22:11:51，0.0 天前  
    > 有 29 个包版本不匹配；  
    > 有 3 个包没有找到对应的文件夹；  
    > 有 7 个包没有填写版本号；  
    > 有 19 个包版本相同。

    针对报告提交了相应的 Issue，检查了目前的 packages-index Issues 有没有被解决 [详情](https://github.com/Cyl18/plct_working/blob/main/month5/2.md)

    - 更新 issue [packages-index/#37](https://github.com/ruyisdk/packages-index/issues/37) 有一部分包无法下载

#### 1.2 RuyiSDK 测试工具开发

- ruyi-packaging 目前能覆盖 board-image 70% 的内容并输出上游版本号

  - [ci: remove make-source workflow #3](https://github.com/ruyisdk/ruyi-packaging/pull/3)

  - [misc: update file structure #4](https://github.com/ruyisdk/ruyi-packaging/pull/4)
  
  - [Initial commit of database #1](https://github.com/ruyisdk/ruyi-packaging/pull/1)
  
  - [Initial commit of package checker #2](https://github.com/ruyisdk/ruyi-packaging/pull/2)
  
  - [Update check_info.json #5](https://github.com/ruyisdk/ruyi-packaging/pull/5)
  
  - [Add openwrt & openeuler_lpi4a checker #6](https://github.com/ruyisdk/ruyi-packaging/pull/6)
  
  - [fix error in lpi4a, update gen_check_info and add revyos_checker #7](https://github.com/ruyisdk/ruyi-packaging/pull/7)
  
  - [Fix error in lpi4a #8](https://github.com/ruyisdk/ruyi-packaging/pull/8)
  
  - [Update gen_check_info and add revyos_checker #9](https://github.com/ruyisdk/ruyi-packaging/pull/9)
  
  - [checker: add revyos checker #10](https://github.com/ruyisdk/ruyi-packaging/pull/10)
  
  - [Update check_info.json manually #11](https://github.com/ruyisdk/ruyi-packaging/pull/11)
  
  - [自动更新脚本初步编写，编写了总体框架，github_checker 以及 GitHub Actions workflow](https://github.com/ruyisdk/ruyi-packaging/pull/2) 
  
  - [添加了  openwrt & openeuler_lpi4a checker](https://github.com/ruyisdk/ruyi-packaging/pull/6) 
  
  - [修复 lpi4a checker 的问题（导致 workflow 不能正常运行）](https://github.com/ruyisdk/ruyi-packaging/pull/8) 
  
  - [增加 revyos_checker，编写脚本 gen_check_info 来生成 check_info.json](https://github.com/ruyisdk/ruyi-packaging/pull/9) 
  
  - [补全 #9 没有覆盖到 revyos_checker 的地方](https://github.com/ruyisdk/ruyi-packaging/pull/10) 
  
  - [更新 db 的 check_info.json 包含所有 board-image](https://github.com/ruyisdk/ruyi-packaging/pull/11) 
  
  - [db 更新](https://github.com/ruyisdk/ruyi-packaging/pull/1) [#5](https://github.com/ruyisdk/ruyi-packaging/pull/5)

- 编写 duo-buildroot-sdk-v2 FreeRTOS 相关[测试代码](https://github.com/weilinfox/duo-buildroot-sdk-v2/blob/33e432779e749724d698bb5b4e88ff755dae2959/freertos/cvitek/task/radio/main_radio.c)，测试发现其支持存在问题，向 support-matrix 提出 issue [[Page Content] Duo FreeRTOS missing test report for duo-buildroot-sdk-v2 #280](https://github.com/ruyisdk/support-matrix/issues/280)

- ruyi-litester
  - [ruyi-litester/#10](https://github.com/weilinfox/ruyi-litester/pull/10)增加了对ruyi list/ruyi admin/ruyi install三个命令的子命令的测试

  - [ruyi-litester/#11](https://github.com/weilinfox/ruyi-litester/pull/11)增加了对ruyi venv命令的子命令的测试

  - [ruyi-litester/#1](https://github.com/weilinfox/ruyi-litester/pull/9)根据0.29.0特性删除了脚本中的json部分

#### 1.3 RuyiSDK网站

- [Add support matrix link to index #99](https://github.com/ruyisdk/ruyisdk-website/pull/99)

- [docs: update English translation #103](https://github.com/ruyisdk/ruyisdk-website/pull/103)

- [biweekly: update to the latest 42 #104](https://github.com/ruyisdk/ruyisdk-website/pull/104)

- [docs: fix typos #107](https://github.com/ruyisdk/ruyisdk-website/pull/107)

- [pages: remove old static page background and fix docs rollback #108](https://github.com/ruyisdk/ruyisdk-website/pull/108)

- [pages: update translations on index #110](https://github.com/ruyisdk/ruyisdk-website/pull/110)

- [pages: fix homepage button on mobile browser #111](https://github.com/ruyisdk/ruyisdk-website/pull/111)

- [Update blog and biweekly to the latest #112](https://github.com/ruyisdk/ruyisdk-website/pull/112)

- [Fix button text wrap on de page #113](https://github.com/ruyisdk/ruyisdk-website/pull/113)

- [pages: really fix homepage css #114](https://github.com/ruyisdk/ruyisdk-website/pull/114)

- [docs: update English version #116](https://github.com/ruyisdk/ruyisdk-website/pull/116)

- [fix the style of statistic page and QRcode #98](https://github.com/ruyisdk/ruyisdk-website/pull/98)

- [Enforce DCO compliance & fix contribution guidelines #101](https://github.com/ruyisdk/ruyisdk-website/pull/101)

- [update en docs #102](https://github.com/ruyisdk/ruyisdk-website/pull/102)

- [news: update ruyi news to latest version 0.31 #105](https://github.com/ruyisdk/ruyisdk-website/pull/105)

- [docs: update English version #106](https://github.com/ruyisdk/ruyisdk-website/pull/106)

- [docs: update English version #116](https://github.com/ruyisdk/ruyisdk-website/pull/116)

- ruyisdk-website 新增 2 个 issue
    - [官网新闻与数据统计页面图片更新 #93](https://github.com/ruyisdk/ruyisdk-website/issues/93)
    - [数据统计页面前端优化 #94](https://github.com/ruyisdk/ruyisdk-website/issues/94)
  
#### 1.4 RuyiSDK文档

- [Add official website update #143](https://github.com/ruyisdk/wechat-articles/pull/143)

- [docs: fix typos #82](https://github.com/ruyisdk/docs/pull/82)

#### 1.5 RuyiSDK技术分享

- 扩充 RISC-V Summit Europe 扩展摘要 [V4](https://gitlab.inuyasha.love/weilinfox/plct-working/-/blob/master/Note/ruyisdk/RuyiSDK%20-%20An%20Integrated%20and%20Customizable%20Toolkit%20for%20RISC-V%20Software%20Development_V4.docx)

- 扩充 RISC-V Summit Europe 使用用例图，修改 Methodology [V4.5](https://gitlab.inuyasha.love/weilinfox/plct-working/-/blob/master/Note/ruyisdk/RuyiSDK%20-%20An%20Integrated%20and%20Customizable%20Toolkit%20for%20RISC-V%20Software%20Development_V4.5.docx)

- 完成和提交 RISC-V Summit Europe 的 [Extended Abstract](https://gitlab.inuyasha.love/weilinfox/plct-working/-/blob/master/Note/ruyisdk/F4_Abstract_RuyiSDK%20-%20An%20Integrated%20and%20Customizable%20Toolkit%20for%20RISC-V%20Software%20Development_2P.pdf) 和 [Poster](https://gitlab.inuyasha.love/weilinfox/plct-working/-/blob/master/Note/ruyisdk/F4_Abstract_RuyiSDK%20-%20An%20Integrated%20and%20Customizable%20Toolkit%20for%20RISC-V%20Software%20Development_2P.pdf)

### 2. 操作系统支持矩阵

#### 2.1 开发板操作系统支持测试
- [BPI-F3/openHarmony: Fix date](https://github.com/ruyisdk/support-matrix/pull/212)
- [Duo/OpenWrt: new test report](https://github.com/ruyisdk/support-matrix/pull/213)
- [Duo/OpenWrt: remove others.yml](https://github.com/ruyisdk/support-matrix/pull/214)
- [Add Fedora and update Debian test reports for Mars; fix some typos.](https://github.com/ruyisdk/support-matrix/pull/216)
- [Star64/Armbian: update link](https://github.com/ruyisdk/support-matrix/pull/235)
- [LicheePi4A: add openEuler 25.03](https://github.com/ruyisdk/support-matrix/pull/237)
- [Sync reports and address recent issues](https://github.com/ruyisdk/support-matrix/pull/238)
- [LicheePi4A/RevyOS: Bump to 20250323](https://github.com/ruyisdk/support-matrix/pull/239)
- [Bit-Birck K1: update to biandu 2.1.1,add alpine](https://github.com/ruyisdk/support-matrix/pull/240)
- [fix typo](https://github.com/ruyisdk/support-matrix/pull/241)
- [duo256/licheervnano: Update Debian/Fedora](https://github.com/ruyisdk/support-matrix/pull/242)
- [BPI-F3: Bianbu Update to v2.1.1](https://github.com/ruyisdk/support-matrix/pull/243)
- [CONTRIBUTING: change/add some definitions in metadata](https://github.com/ruyisdk/support-matrix/pull/245)
- [Pioneer/openEuler: bump to 25.03](https://github.com/ruyisdk/support-matrix/pull/246)
- [LicheePi4A_8-32G: Add RevyOS Test Report](https://github.com/ruyisdk/support-matrix/pull/247)
- [LicheePi4A_8-32GB: Add OpenEuler Innovation Test Report](https://github.com/ruyisdk/support-matrix/pull/248)
- [fix typo](https://github.com/ruyisdk/support-matrix/pull/249)
- [VisionFive2: Add Guix System test reports.](https://github.com/ruyisdk/support-matrix/pull/250)
- [Add ram information for the boards](https://github.com/ruyisdk/support-matrix/pull/251)
- [Megrez: Add Guix System test reports.](https://github.com/ruyisdk/support-matrix/pull/252)
- [Add a new board HiFive Premier P550 and it's Ubuntu, Yocto and Debian test reports (CFT).](https://github.com/ruyisdk/support-matrix/pull/253)
- [visionfive2: Add Guix](https://github.com/ruyisdk/support-matrix/pull/258)
- [Add Guix and Ubuntu test reports for Mars. Add Guix to metadata.](https://github.com/ruyisdk/support-matrix/pull/260)
- [HiFive Premier P550: fix date and board matadata](https://github.com/ruyisdk/support-matrix/pull/261)
- [Unmatched: fix frontmatter](https://github.com/ruyisdk/support-matrix/pull/262)
- [Unmatched: update Armbian](https://github.com/ruyisdk/support-matrix/pull/263)
- [Enforce and document DCO compliance](https://github.com/ruyisdk/support-matrix/pull/264)
- [Fix RAM information for some boards](https://github.com/ruyisdk/support-matrix/pull/265)
- [duos: Add/Update Debian/BuildRoot/NixOS](https://github.com/ruyisdk/support-matrix/pull/266)
- [LicheeConsole4A/RevyOS: Bump to 20240720](https://github.com/ruyisdk/support-matrix/pull/269)
- [Pioneer/openCloudOS: Add openCloudOs Stream 23](https://github.com/ruyisdk/support-matrix/pull/270)
- [Tool: Add metadata checker ](https://github.com/ruyisdk/support-matrix/pull/271)
- [NeZha-D1s/Ubuntu: Bump to Ubuntu 25.04 LTS](https://github.com/ruyisdk/support-matrix/pull/272)
- [PIC64GX/Ubuntu: Bump to Ubuntu 25.04](https://github.com/ruyisdk/support-matrix/pull/273)
- [Icicle/Ubuntu: Bump to Ubuntu 25.04](https://github.com/ruyisdk/support-matrix/pull/274)
- [Star64/Ubuntu: Add Ubuntu 25.04](https://github.com/ruyisdk/support-matrix/pull/275)
- [Add NixOS test reports for Mars.](https://github.com/ruyisdk/support-matrix/pull/276)
- [dongshanpi/mangopi/vf2 updates](https://github.com/ruyisdk/support-matrix/pull/277)
- [LicheePi4A(8-32): Add test report for Armbian ](https://github.com/ruyisdk/support-matrix/pull/278)
- [Update Ubuntu test reports to 25.04 for Mars.](https://github.com/ruyisdk/support-matrix/pull/279)
- [Unmatched: update Debian to trixie/sid](https://github.com/ruyisdk/support-matrix/pull/254)
- [Unmatched: update Ubuntu/OpenWrt/FreeBSD/OpenBSD](https://github.com/ruyisdk/support-matrix/pull/255)
- [oERV: add notes](https://github.com/ruyisdk/support-matrix/pull/256)
- [meles: update revyos 20250323](https://github.com/ruyisdk/support-matrix/pull/259)
- BPi-F3/Bianbu: 更新至v2.1.1 版本，包含 minimal desktop 版本 [#243](https://github.com/ruyisdk/support-matrix/pull/243)
- Pioneer/open Euler：更新至 25.03 版本 [#246](https://github.com/ruyisdk/support-matrix/pull/246)
- Pioneer/openCloudOS: 添加了 openCloud OS Stream的测试报告 [#270](https://github.com/ruyisdk/support-matrix/pull/270)
- LicheeConsole 4A/RevyOS：更新包好到最新版本 [#269](https://github.com/ruyisdk/support-matrix/pull/269)
- 更新部分文档 [#245](https://github.com/ruyisdk/support-matrix/pull/245)
- [Sync reports and address recent issues](https://github.com/ruyisdk/support-matrix/pull/238)
- [duo256/licheervnano: Update Debian/Fedora](https://github.com/ruyisdk/support-matrix/pull/242)
- [visionfive2: Add Guix](https://github.com/ruyisdk/support-matrix/pull/258)
- [duos: Add/Update Debian/BuildRoot/NixOS](https://github.com/ruyisdk/support-matrix/pull/266)
- [Add xv6](https://github.com/ruyisdk/support-matrix/pull/268)
- [dongshanpi/mangopi/vf2 updates](https://github.com/ruyisdk/support-matrix/pull/277)
- [Unmatched: fix frontmatter](https://github.com/ruyisdk/support-matrix/pull/262): 修复 frontmatter
- [HiFive Premier P550: fix date and board matadata](https://github.com/ruyisdk/support-matrix/pull/261): 修复 metadata
- [fix typo](https://github.com/ruyisdk/support-matrix/pull/249)
- [fix typo](https://github.com/ruyisdk/support-matrix/pull/241)
- [Bit-Birck K1: update to biandu 2.1.1,add alpine](https://github.com/ruyisdk/support-matrix/pull/240): 测试并更新 Bit-Brick_K1 @ bianbu, Bit-Brick_K1 @ alpine
- [LicheePi4A: add openEuler 25.03](https://github.com/ruyisdk/support-matrix/pull/237): 测试并更新 LicheePi4A @ openEuler
- [Bit-Brick_K1: add fedora 42 minimal](https://github.com/ruyisdk/support-matrix/pull/211): 测试并更新 Bit-Brick_K1 @ fedora
- lp4a revyos 20250323：https://github.com/ruyisdk/support-matrix/pull/239
- Star64 Ubuntu 25.04：https://github.com/ruyisdk/support-matrix/pull/275
- Iccle Ubuntu 25.04：https://github.com/ruyisdk/support-matrix/pull/274
- PIC64GX Ubuntu 25.04：https://github.com/ruyisdk/support-matrix/pull/273
- Nezha Ubuntu 25.04：https://github.com/ruyisdk/support-matrix/pull/272
- lp4a revyos AIC8800关机后不断电开机，无线网卡也会报错：https://github.com/revyos/revyos/issues/123#issuecomment-2798746008
- lp4a openeuler ruyi ide测试
- [https://github.com/ruyisdk/support-matrix/pull/216](https://github.com/ruyisdk/support-matrix/pull/216)
  - Add Fedora test reports for Mars.
  - Update Debian test reports for Mars.
  - Fix some typos.
- [https://github.com/ruyisdk/support-matrix/pull/260](https://github.com/ruyisdk/support-matrix/pull/260)
  - Add Guix test reports for Mars
  - Update Ubuntu test reports for Mars.
  - Add Guix to metadata.
- [https://github.com/ruyisdk/support-matrix/pull/253](https://github.com/ruyisdk/support-matrix/pull/253)
  - Add a new board HiFive Premier P550.
  - Add Ubuntu, Yocto and Debian (RockOS) test reports for P550 (CFT version).
- [https://github.com/ruyisdk/support-matrix/pull/276](https://github.com/ruyisdk/support-matrix/pull/276)
  - Add NixOS test reports for Mars.
- [https://github.com/ruyisdk/support-matrix/pull/279](https://github.com/ruyisdk/support-matrix/pull/279)
  - Update Ubuntu test reports to 25.04 for Mars.
- [https://github.com/ruyisdk/support-matrix/pull/281](https://github.com/ruyisdk/support-matrix/pull/281)
  - Add OpenWRT and test reports for Mars.
  - Add NuttX test reports for Mars.
  - Fix some typos.
- 补充支持矩阵ESP32 RISC-V 系列mcu，现上传c3，c2 的FreeRTOS(ESP-IDF)和Zephyr。[link](https://github.com/stc15f104w/support-matrix/commit/e6ebe408c7ce4afc1cb2ff723d9a78ced40eff35)
- LPi4A 8-32 oE & RevyOS
  - https://github.com/ruyisdk/support-matrix/pull/248
  - https://github.com/ruyisdk/support-matrix/pull/247
- Metadata RAM info
  - https://github.com/ruyisdk/support-matrix/pull/251
  - https://github.com/ruyisdk/support-matrix/pull/265
- Armbian for LiP4A 8-32G
  - https://github.com/ruyisdk/support-matrix/pull/278
- FreeRTOS for DuoS
  - https://github.com/ruyisdk/support-matrix/pull/283
- [K1: update GCC 14 report](https://github.com/QA-Team-lo/ruyisdk-gnu-tests/commit/4a1deac29c83954de947052af523e3d754ee99ea)

#### 2.2 操作系统支持矩阵Web UI

- [x] 表格支持 others.yml 中的数据
- [x] 支持 github markdown grammer 
- [x] support-matrix: 首行/列固定
- [x] 使用统一主题字体
- [x] Header:项目地址 & ruyi logo
- [x] 修复首页 title
- [x] 更新 copyright
- [x] 搜索栏 i18n
- [x] 首页/board 页 RAM 字段展示
- [x] 首页多种排序方式,默认排序按照 RuyiSDK 支持的流行度排序
- [x] 排序下拉菜单样式优化
- [x] 排序菜单与 i18n
- [x] 统一开发板卡片高度
- [x] 添加 ruyisdk 支持的开发板图标
- [x] matrix: 添加 tab 切换表格
- [x] report: i18n
- [x] 使用 yaml 库
- [x] report: 样式优化
- [x] matrix: 表格渲染重构
- [x] CI: 每日自动更新submodule

- 前端页面 Cloudflare Pages 部署
  - Link: https://matrix.ruyisdk.org
  - Repo: https://github.com/QA-Team-lo/support-matrix-frontend
  
#### 2.3 操作系统支持矩阵测试开发

- 增加对Metadata进行检查的工具及CI： [#271](https://github.com/ruyisdk/support-matrix/pull/271)
- ruyisdk support-matrix 同步
  - 允许config分布在每个文件夹中，增强可维护性 [commit](https://github.com/wychlw/support-matrix/commit/323055859cac89c16e5ff5d418fa41e502f09a52)
  - 添加了 star64/unmatched 的同步配置，更新了多个其余系统配置 [branch](https://github.com/wychlw/support-matrix/commits/update_cfgs/)
  - 对齐ruyi新定义字段，修复一些存在的问题
  - 检查到packages-index的同步产物并提交pr https://github.com/ruyisdk/packages-index/pull/43

#### 2.4 会议和技术分享

- [RuyiSDK 双周报/支持矩阵部分](https://github.com/ruyisdk/wechat-articles/pull/141)

- [RuyiSDK 双周报/支持矩阵部分](https://github.com/ruyisdk/wechat-articles/pull/146)

#### 2.5 操作系统支持矩阵设备维护

- 内部 Infra 日常维护
    - 电源更换
    - 上架 K230，为 https://github.com/ruyisdk/ruyisdk/issues/2 做准备
    - https://radiata.kevinmx.top   
  - 
- 其它线下事务处理
  
#### 2.6 实习生考核

- 甲辰计划联合招聘

### 3. SAIL和ACT

#### 3.1 SAIL和ACT开发

- SAIL/ACT

  - [PR] <https://github.com/riscv/sail-riscv/pull/829> Remove unused cli opts
  - [PR] <https://github.com/riscv-non-isa/riscv-arch-test/pull/636> Add Zilsd/Zclsd support
  - [PR] <https://github.com/riscv/sail-riscv/pull/863> Add URL_HASH to GMP download
  - [PR] <https://github.com/riscv/sail-riscv/pull/857> Add overload bits_of for virtaddr_bits and physaddr_bits
  - [PR] <https://github.com/riscv/sail-riscv/pull/875> Use result type for TR_Result instead of union
  - [PR] <https://github.com/riscv/sail-riscv/pull/872> Use result type instead of PTW_Result
  - [PR] <https://github.com/riscv/sail-riscv/pull/892> Disable htif when tohost symbol not found
  - [PR](https://github.com/riscv/sail-riscv/pull/871)
    - 修复Sail-RISCV在支持34bit物理地址后日志输出上无法按16进制格式化非被四整除位数的bitvector的问题
    - 添加帮助函数`hex_bits_str`以支持对任意位数bitvector的日志输出
  - [PR](https://github.com/riscv/sail-riscv/pull/904) : 提交PR, 使得Sail-RISCV支持可匹配的Zicsr扩展支持
  - [PR](https://github.com/riscv/sail-riscv/pull/807)
    - 规范了satp.MODE在只读情况下mstatus.SUM也只读的情况
    - 对函数名称等具体规范以及逻辑判断条件进行优化，使代码更规范统一
    - 目前已经进入等待合并状态
  - [PR](https://github.com/riscv/sail-riscv/pull/752)
    - 由于同为向量加密集的zvknab已经合并，故进行rebase重构保证代码规范
    - 目前已解决所有提出review的人的问题
  - [PR](https://github.com/riscv-software-src/riscv-tests/commit/4bbcf2e7def4c0b37e893baaa5ddaf31e1144e33) 已合并，修复了 autoupdate
  - [PR](https://github.com/riscv-software-src/riscv-tests/commit/8edbedf776b4d7b9af1061058a816a9a2a44e9ca)已合并，更新了 env 环境
  - [PR](https://github.com/riscv-software-src/riscv-tests/pull/606) ELF Build
  - [PR](https://github.com/riscv/sail-riscv/pull/839) 已合并，重命名
  - [PR](https://github.com/riscv/sail-riscv/pull/882)已合并，fix gmp
  - [PR](https://github.com/riscv/sail-riscv/pull/876)fix build script
  - [PR](https://github.com/riscv/sail-riscv/pull/890)fix wrong CI condition
  - [PR](https://github.com/riscv/sail-riscv/pull/889)Move to new os-boot
  - [PR](https://github.com/riscv-non-isa/riscv-arch-test/pull/635)已合并，提交拆分zfh测试用例
  - [PR](https://github.com/riscv-non-isa/riscv-arch-test/pull/628)已合并，提交拆分zfinx测试用例

    | 测试用例名 | 所属拓展 | 地址                                                         |
    | ---------- | -------- | ------------------------------------------------------------ |
    | fmadd-b15  | zfinx    | [github](https://github.com/riscv-non-isa/riscv-arch-test/tree/dev/riscv-test-suite/rv64i_m/Zhinx/src) |
    | fmsub-b15  | zfinx    | [github](https://github.com/riscv-non-isa/riscv-arch-test/tree/dev/riscv-test-suite/rv64i_m/Zhinx/src) |
    | fnmadd-b15 | zfinx    | [github](https://github.com/riscv-non-isa/riscv-arch-test/tree/dev/riscv-test-suite/rv64i_m/Zhinx/src) |
    | fnmsub-b15 | zfinx    | [github](https://github.com/riscv-non-isa/riscv-arch-test/tree/dev/riscv-test-suite/rv64i_m/Zhinx/src) |
    | fmadd-b15  | zfh      | [github](https://github.com/riscv-non-isa/riscv-arch-test/tree/dev/riscv-test-suite/rv32i_m/Zfh/src) |
    | fmsub-b15  | zfh      | [github](https://github.com/riscv-non-isa/riscv-arch-test/tree/dev/riscv-test-suite/rv32i_m/Zfh/src) |
    | fnmadd-b15 | zfh      | [github](https://github.com/riscv-non-isa/riscv-arch-test/tree/dev/riscv-test-suite/rv32i_m/Zfh/src) |
    | fnmsub-b15 | zfh      | [github](https://github.com/riscv-non-isa/riscv-arch-test/tree/dev/riscv-test-suite/rv32i_m/Zfh/src) |
    | fmadd-b1   | zfh      | [github](https://github.com/riscv-non-isa/riscv-arch-test/tree/dev/riscv-test-suite/rv32i_m/Zfh/src) |
    | fmsub-b1   | zfh      | [github](https://github.com/riscv-non-isa/riscv-arch-test/tree/dev/riscv-test-suite/rv32i_m/Zfh/src) |
    | fnmadd-b1  | zfh      | [github](https://github.com/riscv-non-isa/riscv-arch-test/tree/dev/riscv-test-suite/rv32i_m/Zfh/src) |
    | fnmsub-b1  | zfh      | [github](https://github.com/riscv-non-isa/riscv-arch-test/tree/dev/riscv-test-suite/rv32i_m/Zfh/src) |

  - [PR](https://github.com/riscv-non-isa/riscv-arch-test/pull/638)为ACT添加缺失的RV32 Zfinx共117个测试用例
  - [PR](https://github.com/riscv-non-isa/riscv-arch-test/pull/642)为ACT添加缺失的rv32 Zdinx fmadd/fmsub共580个测试用例

    | 测试指令名 | 所属拓展 | 地址                                                         |
    | ---------- | -------- | ------------------------------------------------------------ |
    | fadd       | Zfinx    | [github](https://github.com/riscv-non-isa/riscv-arch-test/tree/dev/riscv-test-suite/rv32i_m/Zfinx/src) |
    | fclass     | Zfinx    | [github](https://github.com/riscv-non-isa/riscv-arch-test/tree/dev/riscv-test-suite/rv32i_m/Zfinx/src) |
    | fdiv       | Zfinx    | [github](https://github.com/riscv-non-isa/riscv-arch-test/tree/dev/riscv-test-suite/rv32i_m/Zfinx/src) |
    | feq        | Zfinx    | [github](https://github.com/riscv-non-isa/riscv-arch-test/tree/dev/riscv-test-suite/rv32i_m/Zfinx/src) |
    | fle        | Zfinx    | [github](https://github.com/riscv-non-isa/riscv-arch-test/tree/dev/riscv-test-suite/rv32i_m/Zfinx/src) |
    | flt        | Zfinx    | [github](https://github.com/riscv-non-isa/riscv-arch-test/tree/dev/riscv-test-suite/rv32i_m/Zfinx/src) |
    | fmadd      | Zfinx    | [github](https://github.com/riscv-non-isa/riscv-arch-test/tree/dev/riscv-test-suite/rv32i_m/Zfinx/src) |
    | fmax       | Zfinx    | [github](https://github.com/riscv-non-isa/riscv-arch-test/tree/dev/riscv-test-suite/rv32i_m/Zfinx/src) |
    | fmin       | Zfinx    | [github](https://github.com/riscv-non-isa/riscv-arch-test/tree/dev/riscv-test-suite/rv32i_m/Zfinx/src) |
    | fmsub      | Zfinx    | [github](https://github.com/riscv-non-isa/riscv-arch-test/tree/dev/riscv-test-suite/rv32i_m/Zfinx/src) |
    | fmul       | Zfinx    | [github](https://github.com/riscv-non-isa/riscv-arch-test/tree/dev/riscv-test-suite/rv32i_m/Zfinx/src) |
    | fnmadd     | Zfinx    | [github](https://github.com/riscv-non-isa/riscv-arch-test/tree/dev/riscv-test-suite/rv32i_m/Zfinx/src) |
    | fnmsub     | Zfinx    | [github](https://github.com/riscv-non-isa/riscv-arch-test/tree/dev/riscv-test-suite/rv32i_m/Zfinx/src) |
    | fsgnj      | Zfinx    | [github](https://github.com/riscv-non-isa/riscv-arch-test/tree/dev/riscv-test-suite/rv32i_m/Zfinx/src) |
    | fsgnjn     | Zfinx    | [github](https://github.com/riscv-non-isa/riscv-arch-test/tree/dev/riscv-test-suite/rv32i_m/Zfinx/src) |
    | fsgnjx     | Zfinx    | [github](https://github.com/riscv-non-isa/riscv-arch-test/tree/dev/riscv-test-suite/rv32i_m/Zfinx/src) |
    | fsqrt      | Zfinx    | [github](https://github.com/riscv-non-isa/riscv-arch-test/tree/dev/riscv-test-suite/rv32i_m/Zfinx/src) |
    | fsub       | Zfinx    | [github](https://github.com/riscv-non-isa/riscv-arch-test/tree/dev/riscv-test-suite/rv32i_m/Zfinx/src) |
    | fmadd      | Zdinx    | [github](https://github.com/riscv-non-isa/riscv-arch-test/tree/dev/riscv-test-suite/rv32i_m/Zdinx/src) |
    | fnmadd     | Zdinx    | [github](https://github.com/riscv-non-isa/riscv-arch-test/tree/dev/riscv-test-suite/rv32i_m/Zdinx/src) |
    | fmsub      | Zdinx    | [github](https://github.com/riscv-non-isa/riscv-arch-test/tree/dev/riscv-test-suite/rv32i_m/Zdinx/src) |
    | fnmsub     | Zdinx    | [github](https://github.com/riscv-non-isa/riscv-arch-test/tree/dev/riscv-test-suite/rv32i_m/Zdinx/src) |

- Issues
  - [#758](https://github.com/riscv/sail-riscv/discussions/834): `ram_size` doesn't `<< 20` by default since
  - [#851](https://github.com/riscv/sail-riscv/issues/851): 解决Issue提出问题

#### 3.2 SAIL技术分享
  
- [sail code gen](https://github.com/rez5427/plct/blob/main/Report/sailcodegen.pptx)

#### 3.3 SAIL会议

- tech-golden-model meeting [`04.07`, `04.14`, `04.21`, `04.28`](https://docs.google.com/document/d/11f9ihMT8vcmgijmvebMiHttwSbw9eY_MKkR9ea3CNFCg)
- 参加了4月7日的Architectural Compatible Test SIG Meeting，并记录了会议文档[#MD](https://github.com/Pagerd/PLCT/blob/main/Report/week/week80/ACT.md)
- 参加了4月22日的Architectural Compatible Test SIG Meeting，并记录了会议文档[#MD](https://github.com/Pagerd/PLCT/blob/main/Report/week/week82/ACT.md)
  
### 4. 独立测试项目/应用软件生态观测

#### 4.1 RISC-V B-EXT Clang Performance Test

共测试以下三款芯片：
- JH7110
- K1
- P550

分别对：`rv64gc` `rv64gc_zba_zbb` `rv64gc_zba_zbb_zbc_zbs` 三个扩展测试

包含以下测试：
- Coremark
- Coremark Pro
- csibe
- Unic Bench

报告见：[bext_test_clang](ttps://github.com/wychlw/plct/tree/main/doc/bext_test_clang)

#### 4.2 GNU工具链测试

- SG2042 TH1520两个芯片的gnu-upstream gnu-plct均更新至14 [ruyisdk-gnu-tests](https://github.com/QA-Team-lo/ruyisdk-gnu-tests/)

#### 4.3 oscompare

其中部分为自年初 oscompare 任务移植而来并增加英文翻译。

新增 19 篇：

- Deepin @ BPI-F3
- openEuler @ BPI-F3
- openKylin @ Jupiter
- Deepin @ LicheePi4A (8-32)
- Fedora @ LicheePi4A (8-32)
- OpenWRT @ LicheePi4A (8-32)
- openEuler @ LicheePi4A (8-32)
- openKylin @ LicheePi4A (8-32)
- Deepin @ Meles
- openEuler @ Meles
- openEuler @ Meles
- Guix @ VisionFive2
- xv6 @ Milk-V Duo
- xv6 @ Milk-V Duo 256M (CFH)
- xv6 @ Milk-V Duo S (CFH)
- xv6 @ MangoPi MQ-Pro
- xv6 @ DongshanPi Nezha-STU
- Deepin @ DongshanPi-Nezha STU (Basic)
- Deepin @ MangoPi MQ-Pro (Good)

更新 14 篇：
- openEuler @ LicheePi4A
- openKylin @ LicheePi4A
- Deepin @ Pioneer
- openEuler @ Pioneer
- Deepin @ Unmatched
- openEuler @ Unmatched
- openKylin @ Unmatched
- BuildRoot @ Duo 256M (分 v1/v2)
- BuildRoot @ LicheeRV Nano
- Debian @ Duo 256M
- Debian @ LicheeRV Nano
- Ubuntu @ VisionFive2 (25.04)
- ArchLinux @ DongshanPi-Nezha STU (Basic)
- ArchLinux @ MangoPi MQ-Pro (Basic)

#### 4.4 ruyisdk-gnu-tests

- 更新 K230 上 ruyisdk gnu-upstream GCC 14.2.0 的测试: https://github.com/QA-Team-lo/ruyisdk-gnu-tests/commit/c3ecaef4e15edeab70dd08551e0ba27f39fbef2f

- [https://github.com/QA-Team-lo/ruyisdk-gnu-tests/commit/9e87128d587a38412b7aca41651d050a3050e34d](https://github.com/QA-Team-lo/ruyisdk-gnu-tests/commit/9e87128d587a38412b7aca41651d050a3050e34d)
  - JH7110 (Milk-V Mars): Native, Cross both w/ B, w/o B.
  - CV1800B (Milk-V Duo): Cross.

#### 4.5 OpenGauss test

- [Test OpenGauss v7.0.0-RC1 on LicheePi 4A based on oERV 24.04 LTS (WIP)](https://github.com/QA-Team-lo/Team-Planning/issues/100)

- GCC14 @ SG2000
  - https://github.com/QA-Team-lo/ruyisdk-gnu-tests/pull/5

- [测试 gcc15](https://github.com/riscv-collab/riscv-gnu-toolchain/pull/1710) 

#### 4.6 Vulkan CTS Test
- [test for Vulkan CTS 1.3.8.1 on MilkV Megrez](https://github.com/QA-Team-lo/VK-GL-CTS/commit/495b404362b9151784b9dc618faa693330c707a7)

#### 4.7 ROS1/2 测试和Demo原型开发

- Lichee Pi 4A 环境搭建
    - 完成 Lichee Pi 4A 的镜像刷写与 ROS 环境安装。
    - 编译 YOLOX 测试用例并通过 CPU 成功运行。

- NPU 功能测试
    - 针对 Lichee Pi 4A 使用 NPU（TH1520）运行 yolov5n 时遇到的问题进行排查，并成功解决 [Issue revyos/revyos#126](https://github.com/revyos/revyos/issues/126)。

- 文档
    - 提交 PR [sipeed/sipeed_wiki#797](https://github.com/sipeed/sipeed_wiki/pull/797) 添加导出 `onnx` 模型时的必要参数，该 PR 已被合并。

- 项目部署与验证
    - 采用多机通信的方式，成功在 Lichee Pi 4A 与其他设备间跑通项目，记录多机通信部署的相关文档：[multiple_mechine.md](https://github.com/lalafua/sim_llm/blob/main/docs/multiple_mechine.md)。
    - 录制多机通信运行效果的演示视频：[demo_multiple_mechine.webm](https://github.com/lalafua/sim_llm/blob/main/assets/demo_multiple_mechine.webm)

## Milkv duo YOLO12

- [PR 已合并](https://github.com/milk-v/milkv.io/pull/130)

### 5. RISC-V教育和短视频

#### 5.1 短视频课程设计

- 项目迭代计划

https://github.com/DuoQilai/PLCT-Works/tree/main/RISC-V_short_video/Plan_Document

- 项目迭代回溯

https://github.com/DuoQilai/PLCT-Works/tree/main/RISC-V_short_video/Review_Document

- [Milk-V Duo YOLO实战课程项目方案](https://github.com/DuoQilai/PLCT-Works/blob/main/RISC-V_short_video/Project_proposal_yolo11_new.md)
  
#### 5.2 短视频设计和实现

- [在 Duo S 上使用 Pico-8SEG-LED 数码管](https://www.bilibili.com/video/BV1jpLyzgE3T)

- [YOLO FFmpeg（下）](https://www.bilibili.com/video/BV1aNdXY1Esa)

- [Milk-V Duo基于 YOLOv5 的目标检测](https://www.bilibili.com/video/BV1F85xzfEJw)

- [在 Duo S 上使用 Pico-8SEG-LED 数码管](https://www.bilibili.com/video/BV1jpLyzgE3T)

- [X-AnyLabeling的安装与使用](https://www.bilibili.com/video/BV1SgdXYGEV1)  

- [X-AnyLabeling的安装与使用（下）](https://www.bilibili.com/video/BV1kwLbzBEja)

- [设置模型参数](https://www.bilibili.com/video/BV1aHLbzRErM)  

#### 5.3 短视频剧本和文档、素材

- [Milk-V Duo基于 YOLOv5 的目标检测](https://github.com/jason-hue/plct/blob/main/%E6%B5%8B%E8%AF%95%E6%96%87%E6%A1%A3/Milk-V%20Duo%E5%9F%BA%E4%BA%8E%20YOLOv5%20%E7%9A%84%E7%9B%AE%E6%A0%87%E6%A3%80%E6%B5%8B.md)

- [Milk-V Duo新手入门（上）](https://github.com/jason-hue/plct/blob/main/%E6%B5%8B%E8%AF%95%E6%96%87%E6%A1%A3/Milk-V%20Duo%E6%96%B0%E6%89%8B%E5%85%A5%E9%97%A8%EF%BC%88%E4%B8%8A%EF%BC%89.md)

- [Milk-V Duo新手入门（下）-待补充完整](https://github.com/jason-hue/plct/blob/main/%E6%B5%8B%E8%AF%95%E6%96%87%E6%A1%A3/Milk-V%20Duo%E6%96%B0%E6%89%8B%E5%85%A5%E9%97%A8%EF%BC%88%E4%B8%8B%EF%BC%89.md)

- [在 Duo S 上使用 Pico-8SEG-LED 数码管](https://github.com/jason-hue/plct/blob/main/%E6%B5%8B%E8%AF%95%E6%96%87%E6%A1%A3/%E5%9C%A8%20Duo%20S%20%E4%B8%8A%E4%BD%BF%E7%94%A8%20Pico-8SEG-LED%20%E6%95%B0%E7%A0%81%E7%AE%A1.md)

- [X-AnyLabeling的安装与使用](https://github.com/LizzyLoong/Practice/blob/main/General/X-AngLabeling%E7%9A%84%E5%AE%89%E8%A3%85%E4%B8%8E%E4%BD%BF%E7%94%A8%E6%96%87%E6%A1%A3.md)
- [X-AnyLabeling的安装与使用视频脚本](https://github.com/LizzyLoong/Practice/blob/main/yolo/2.3X-AnyLabeling%E7%9A%84%E5%AE%89%E8%A3%85%E4%B8%8E%E4%BD%BF%E7%94%A8/%E8%A7%86%E9%A2%91%E8%84%9A%E6%9C%AC.md)     
- [X-AnyLabeling的安装与使用（下）视频脚本](https://github.com/LizzyLoong/Practice/blob/main/%E5%91%A8%E6%8A%A5/4%E6%9C%88%E7%AC%AC2%E5%91%A8%E4%BA%A7%E5%87%BA.md)
- [设置模型参数视频脚本](https://github.com/LizzyLoong/Practice/blob/main/yolo/3.1%E8%AE%BE%E7%BD%AE%E6%A8%A1%E5%9E%8B%E5%8F%82%E6%95%B0/%E8%A7%86%E9%A2%91%E8%84%9A%E6%9C%AC.md)    
- [设置模型参数文档](https://github.com/LizzyLoong/Practice/blob/main/yolo/3.1%E8%AE%BE%E7%BD%AE%E6%A8%A1%E5%9E%8B%E5%8F%82%E6%95%B0/%E8%AE%BE%E7%BD%AE%E6%A8%A1%E5%9E%8B%E5%8F%82%E6%95%B0.md)    

#### 5.5 短视频技术分享

- 2025年 RISC-V 欧洲峰会

  - [Abstract修改](https://github.com/DuoQilai/PLCT-Works/blob/main/Notes/RVDay/F3_Abstract_Building%20the%20RISC-V%20Education%20Ecosystem%20A%20Systematic%20Educational%20Contents%20Design%2C%20Remote%20Laboratories%2C%20and%20Community-Driven%20Learning.docx)

  - [Poster提交](https://github.com/DuoQilai/PLCT-Works/blob/main/Notes/RVDay/F1_Poster_Building%20the%20RISC-V%20Education%20Ecosystem.pptx)

  - [Slides提交](https://github.com/DuoQilai/PLCT-Works/blob/main/Notes/RVDay/Slides_Europe.pptx)
 
### 6. 职工

#### 6.1 蔡玮霖

- Ruyi v0.31.0 测试完成 [!67](https://gitee.com/yunxiangluo/ruyisdk-test/pulls/67)

- Ruyi v0.32.0-alpha.20250409 测试完成，新增一个 issue（已修复） [#291](https://github.com/ruyisdk/ruyi/issues/291)

- ruyisdk-website 提交 11 个 pr
  - [Add support matrix link to index #99](https://github.com/ruyisdk/ruyisdk-website/pull/99)
  - [docs: update English translation #103](https://github.com/ruyisdk/ruyisdk-website/pull/103)
  - [biweekly: update to the latest 42 #104](https://github.com/ruyisdk/ruyisdk-website/pull/104)
  - [docs: fix typos #107](https://github.com/ruyisdk/ruyisdk-website/pull/107)
  - [pages: remove old static page background and fix docs rollback #108](https://github.com/ruyisdk/ruyisdk-website/pull/108)
  - [pages: update translations on index #110](https://github.com/ruyisdk/ruyisdk-website/pull/110)
  - [pages: fix homepage button on mobile browser #111](https://github.com/ruyisdk/ruyisdk-website/pull/111)
  - [Update blog and biweekly to the latest #112](https://github.com/ruyisdk/ruyisdk-website/pull/112)
  - [Fix button text wrap on de page #113](https://github.com/ruyisdk/ruyisdk-website/pull/113)
  - [pages: really fix homepage css #114](https://github.com/ruyisdk/ruyisdk-website/pull/114)
  - [docs: update English version #116](https://github.com/ruyisdk/ruyisdk-website/pull/116)
- ruyisdk-website 审核 6 个 pr
    - [fix the style of statistic page and QRcode #98](https://github.com/ruyisdk/ruyisdk-website/pull/98)
    - [Enforce DCO compliance & fix contribution guidelines #101](https://github.com/ruyisdk/ruyisdk-website/pull/101)
    - [update en docs #102](https://github.com/ruyisdk/ruyisdk-website/pull/102)
    - [news: update ruyi news to latest version 0.31 #105](https://github.com/ruyisdk/ruyisdk-website/pull/105)
    - [docs: update English version #106](https://github.com/ruyisdk/ruyisdk-website/pull/106)
    - [docs: update English version #116](https://github.com/ruyisdk/ruyisdk-website/pull/116)
- ruyisdk-website 新增 2 个 issue
    - [官网新闻与数据统计页面图片更新 #93](https://github.com/ruyisdk/ruyisdk-website/issues/93)
    - [数据统计页面前端优化 #94](https://github.com/ruyisdk/ruyisdk-website/issues/94)
- wechat-articles 提交 1 个 pr
    - [Add official website update #143](https://github.com/ruyisdk/wechat-articles/pull/143)
- docs 提交 1 个 pr
    - [docs: fix typos #82]https://github.com/ruyisdk/docs/pull/82
- ruyi-packaging 提交 2 个 pr
    - [ci: remove make-source workflow #3](https://github.com/ruyisdk/ruyi-packaging/pull/3)
    - [misc: update file structure #4](https://github.com/ruyisdk/ruyi-packaging/pull/4)
- ruyi-packaging 审核 9 个 pr
    - [Initial commit of database #1](https://github.com/ruyisdk/ruyi-packaging/pull/1)
    - [Initial commit of package checker #2](https://github.com/ruyisdk/ruyi-packaging/pull/2)
    - [Update check_info.json #5](https://github.com/ruyisdk/ruyi-packaging/pull/5)
    - [Add openwrt & openeuler_lpi4a checker #6](https://github.com/ruyisdk/ruyi-packaging/pull/6)
    - [fix error in lpi4a, update gen_check_info and add revyos_checker #7](https://github.com/ruyisdk/ruyi-packaging/pull/7)
    - [Fix error in lpi4a #8](https://github.com/ruyisdk/ruyi-packaging/pull/8)
    - [Update gen_check_info and add revyos_checker #9](https://github.com/ruyisdk/ruyi-packaging/pull/9)
    - [checker: add revyos checker #10](https://github.com/ruyisdk/ruyi-packaging/pull/10)
    - [Update check_info.json manually #11](https://github.com/ruyisdk/ruyi-packaging/pull/11)
- 审核实习生 pr
    - [!3 change v0.29.0](https://gitee.com/weilinfox/ruyisdk-test/pulls/3)
    - [delete json #9](https://github.com/weilinfox/ruyi-litester/pull/9)
    - [ruyi list extend #10 (暂未合并)](https://github.com/weilinfox/ruyi-litester/pull/10)
    - [Ruyi venv change #11 (暂未合并)](https://github.com/weilinfox/ruyi-litester/pull/11)
- 依据 v0.29.0 的 packages-index 和 support-matrix 附加测试报告提交 issue
    - [[packages-index] Unmatched Ubuntu image in packages-index out of sync #218](https://github.com/ruyisdk/support-matrix/issues/218)
    - [[packages-index] Unmatched FreeBSD image link in test report is broken #219](https://github.com/ruyisdk/support-matrix/issues/219)
    - [[packages-index] BPI-F3 Bianbu image out of date with upstream and out of sync with packages-index #220](https://github.com/ruyisdk/support-matrix/issues/220)
    - [[packages-index] Unmatched OpenBSD image link in test report is broken #221](https://github.com/ruyisdk/support-matrix/issues/221)
    - [[packages-index] Star64 Armbian image link in test report is broken #222](https://github.com/ruyisdk/support-matrix/issues/222)
    - [[packages-index] LicheePi4A openEuler image missing innovation version reports and out of sync with packages-index #223](https://github.com/ruyisdk/support-matrix/issues/223)
    - [[packages-index] LicheePi4A RevyOS image out of date with upstream and out of sync with packages-index #224](https://github.com/ruyisdk/support-matrix/issues/224)
    - [[packages-index] LicheeConsole4A RevyOS image out of date with upstream #225](https://github.com/ruyisdk/support-matrix/issues/225)
    - [[packages-index] MilkV Meles RevyOS image out of date with upstream and out of sync with packages-index #226](https://github.com/ruyisdk/support-matrix/issues/226)
    - [[packages-index] Pioneer openEuler image missing innovation version reports and out of sync with packages-index #227](https://github.com/ruyisdk/support-matrix/issues/227)
    - [[packages-index] VisionFive2 openEuler image link in test report is broken and status if it may need update #228](https://github.com/ruyisdk/support-matrix/issues/228)
    - [[packages-index] VisionFive openEuler support status may need update #229](https://github.com/ruyisdk/support-matrix/issues/229)
    - [[packages-index] AWOL Nezha D1 openEuler support status may need update #230](https://github.com/ruyisdk/support-matrix/issues/230)
    - [[packages-index] Unmatched openEuler support status may need update #231](https://github.com/ruyisdk/support-matrix/issues/231)
    - [[packages-index] Unmatched openKylin installation guide link broken and support status may need update #232](https://github.com/ruyisdk/support-matrix/issues/232)
    - [[packages-index] OpenWrt Unmatched image out of date with upstream #233](https://github.com/ruyisdk/support-matrix/issues/233)
    - [[packages-index] BuildRoot LicheeRV_Nano image out of date with upstream #234](https://github.com/ruyisdk/support-matrix/issues/234)
    - [[packages-index] MilkV Duo/Duo256m BuildRoot image syncing is a bit wired #236](https://github.com/ruyisdk/support-matrix/issues/236)
- 编写 duo-buildroot-sdk-v2 FreeRTOS 相关[测试代码](https://github.com/weilinfox/duo-buildroot-sdk-v2/blob/33e432779e749724d698bb5b4e88ff755dae2959/freertos/cvitek/task/radio/main_radio.c)，测试发现其支持存在问题，向 support-matrix 提出 issue [[Page Content] Duo FreeRTOS missing test report for duo-buildroot-sdk-v2 #280](https://github.com/ruyisdk/support-matrix/issues/280)
- 扩充 RISC-V Summit Europe 扩展摘要到 3 页半 [V4](https://gitlab.inuyasha.love/weilinfox/plct-working/-/blob/master/Note/ruyisdk/RuyiSDK%20-%20An%20Integrated%20and%20Customizable%20Toolkit%20for%20RISC-V%20Software%20Development_V4.docx)
- 扩充 RISC-V Summit Europe 使用用例图，修改 Methodology [V4.5](https://gitlab.inuyasha.love/weilinfox/plct-working/-/blob/master/Note/ruyisdk/RuyiSDK%20-%20An%20Integrated%20and%20Customizable%20Toolkit%20for%20RISC-V%20Software%20Development_V4.5.docx)
- 完成和提交 RISC-V Summit Europe 的 [Extended Abstract](https://gitlab.inuyasha.love/weilinfox/plct-working/-/blob/master/Note/ruyisdk/F4_Abstract_RuyiSDK%20-%20An%20Integrated%20and%20Customizable%20Toolkit%20for%20RISC-V%20Software%20Development_2P.pdf) 和 [Poster](https://gitlab.inuyasha.love/weilinfox/plct-working/-/blob/master/Note/ruyisdk/F4_Abstract_RuyiSDK%20-%20An%20Integrated%20and%20Customizable%20Toolkit%20for%20RISC-V%20Software%20Development_2P.pdf)

#### 6.2 郑景坤

##### 6.2.1 操作系统支持矩阵

- PR Review (对测试团队操作系统支持矩阵产出的review审核，以下内容同测试团队产出，此处为review)
    - [BPI-F3/openHarmony: Fix date](https://github.com/ruyisdk/support-matrix/pull/212)
    - [Duo/OpenWrt: new test report](https://github.com/ruyisdk/support-matrix/pull/213)
    - [Duo/OpenWrt: remove others.yml](https://github.com/ruyisdk/support-matrix/pull/214)
    - [Add Fedora and update Debian test reports for Mars; fix some typos.](https://github.com/ruyisdk/support-matrix/pull/216)
    - [Star64/Armbian: update link](https://github.com/ruyisdk/support-matrix/pull/235)
    - [LicheePi4A: add openEuler 25.03](https://github.com/ruyisdk/support-matrix/pull/237)
    - [Sync reports and address recent issues](https://github.com/ruyisdk/support-matrix/pull/238)
    - [LicheePi4A/RevyOS: Bump to 20250323](https://github.com/ruyisdk/support-matrix/pull/239)
    - [Bit-Birck K1: update to biandu 2.1.1,add alpine](https://github.com/ruyisdk/support-matrix/pull/240)
    - [fix typo](https://github.com/ruyisdk/support-matrix/pull/241)
    - [duo256/licheervnano: Update Debian/Fedora](https://github.com/ruyisdk/support-matrix/pull/242)
    - [BPI-F3: Bianbu Update to v2.1.1](https://github.com/ruyisdk/support-matrix/pull/243)
    - [CONTRIBUTING: change/add some definitions in metadata](https://github.com/ruyisdk/support-matrix/pull/245)
    - [Pioneer/openEuler: bump to 25.03](https://github.com/ruyisdk/support-matrix/pull/246)
    - [LicheePi4A_8+32G: Add RevyOS Test Report](https://github.com/ruyisdk/support-matrix/pull/247)
    - [LicheePi4A_8+32GB: Add OpenEuler Innovation Test Report](https://github.com/ruyisdk/support-matrix/pull/248)
    - [fix typo](https://github.com/ruyisdk/support-matrix/pull/249)
    - [VisionFive2: Add Guix System test reports.](https://github.com/ruyisdk/support-matrix/pull/250)
    - [Add ram information for the boards](https://github.com/ruyisdk/support-matrix/pull/251)
    - [Megrez: Add Guix System test reports.](https://github.com/ruyisdk/support-matrix/pull/252)
    - [Add a new board HiFive Premier P550 and it's Ubuntu, Yocto and Debian test reports (CFT).](https://github.com/ruyisdk/support-matrix/pull/253)
    - [visionfive2: Add Guix](https://github.com/ruyisdk/support-matrix/pull/258)
    - [Add Guix and Ubuntu test reports for Mars. Add Guix to metadata.](https://github.com/ruyisdk/support-matrix/pull/260)
    - [HiFive Premier P550: fix date and board matadata](https://github.com/ruyisdk/support-matrix/pull/261)
    - [Unmatched: fix frontmatter](https://github.com/ruyisdk/support-matrix/pull/262)
    - [Unmatched: update Armbian](https://github.com/ruyisdk/support-matrix/pull/263)
    - [Enforce and document DCO compliance](https://github.com/ruyisdk/support-matrix/pull/264)
    - [Fix RAM information for some boards](https://github.com/ruyisdk/support-matrix/pull/265)
    - [duos: Add/Update Debian/BuildRoot/NixOS](https://github.com/ruyisdk/support-matrix/pull/266)
    - [LicheeConsole4A/RevyOS: Bump to 20240720](https://github.com/ruyisdk/support-matrix/pull/269)
    - [Pioneer/openCloudOS: Add openCloudOs Stream 23](https://github.com/ruyisdk/support-matrix/pull/270)
    - [Tool: Add metadata checker ](https://github.com/ruyisdk/support-matrix/pull/271)
    - [NeZha-D1s/Ubuntu: Bump to Ubuntu 25.04 LTS](https://github.com/ruyisdk/support-matrix/pull/272)
    - [PIC64GX/Ubuntu: Bump to Ubuntu 25.04](https://github.com/ruyisdk/support-matrix/pull/273)
    - [Icicle/Ubuntu: Bump to Ubuntu 25.04](https://github.com/ruyisdk/support-matrix/pull/274)
    - [Star64/Ubuntu: Add Ubuntu 25.04](https://github.com/ruyisdk/support-matrix/pull/275)
    - [Add NixOS test reports for Mars.](https://github.com/ruyisdk/support-matrix/pull/276)
    - [dongshanpi/mangopi/vf2 updates](https://github.com/ruyisdk/support-matrix/pull/277)
    - [LicheePi4A(8+32): Add test report for Armbian ](https://github.com/ruyisdk/support-matrix/pull/278)
    - [Update Ubuntu test reports to 25.04 for Mars.](https://github.com/ruyisdk/support-matrix/pull/279)
- New PRs
    - [Unmatched: update Debian to trixie/sid](https://github.com/ruyisdk/support-matrix/pull/254)
    - [Unmatched: update Ubuntu/OpenWrt/FreeBSD/OpenBSD](https://github.com/ruyisdk/support-matrix/pull/255)
    - [oERV: add notes](https://github.com/ruyisdk/support-matrix/pull/256)
    - [meles: update revyos 20250323](https://github.com/ruyisdk/support-matrix/pull/259)
- 前端页面 Cloudflare Pages 部署
    - Link: https://matrix.ruyisdk.org
    - Repo: https://github.com/QA-Team-lo/support-matrix-frontend

##### 6.2.2 RuyiSDK 双周报/支持矩阵部分

- https://github.com/ruyisdk/wechat-articles/pull/141
- https://github.com/ruyisdk/wechat-articles/pull/146

##### 6.2.3 RevyOS 小队

- PR Review
    - [Bump lpi4a and meles to 20250323](https://github.com/revyos/docs/pull/39)
- [东亚双周会](https://docs.google.com/presentation/d/1S_p_7RZdbtw0lO5PST44AatPu3ObaIGyAnWhOsMnH0M/edit#slide=id.g327cde8f41c_0_161) & [Open Hours](https://docs.google.com/presentation/d/1LkiRpb2B5AGF1XEv9e1qQwzZvsmV86Mvopoecl-NtB8/edit#slide=id.g2c2a5d80f05_5_0) 宣发
- `revyos-keyring` 过期及解决方案公告
    - https://github.com/orgs/revyos/discussions/127
    - https://github.com/revyos/revyos/issues/125
- Issue tracking
    - https://github.com/revyos/revyos/issues/104
- Docs additions
    - [docs: installation: add MCU fw upgrade guide](https://github.com/revyos/docs/pull/43)

#### 6.3 阎明铸

##### 6.3.1 sail/act

- \[PR\] <https://github.com/riscv/sail-riscv/pull/829> Remove unused cli opts
- \[PR\] <https://github.com/riscv-non-isa/riscv-arch-test/pull/636> Add Zilsd/Zclsd support
- \[PR\] <https://github.com/riscv/sail-riscv/pull/863> Add URL_HASH to GMP download
- \[PR\] <https://github.com/riscv/sail-riscv/pull/857> Add overload bits_of for virtaddr_bits and physaddr_bits
- \[PR\] <https://github.com/riscv/sail-riscv/pull/875> Use result type for TR_Result instead of union
- \[PR\] <https://github.com/riscv/sail-riscv/pull/872> Use result type instead of PTW_Result
- \[PR\] <https://github.com/riscv/sail-riscv/pull/892> Disable htif when tohost symbol not found
- \[讨论\] <https://github.com/riscv/sail-riscv/discussions/834> `ram_size` doesn't `<< 20` by default since #758

##### 6.3.2 会议/技术分享

- tech-golden-model meeting [`04.07`, `04.14`, `04.21`, `04.28`](https://docs.google.com/document/d/11f9ihMT8vcmgijmvebMiHttwSbw9eY_MKkR9ea3CNFCg)

- tech-golden-model meeting [`03.03`, `03.10`, `03.10`, `03.10`](https://docs.google.com/document/d/1f9ihMT8vcmgijmvebMiHttwSbw9eY_MKkR9ea3CNFCg)
  
- 第 9 (3.31) 次 RISC-V 软硬件前沿及开源基础设施研讨会：基于 Sail 的指令集模拟器加速技术研究 [PPT](https://github.com/trdthg/plct/blob/main/outcome/202503/Pydrofoil.pdf)

#### 6.4 张馥媛

##### 6.4.1 Milk-V Duo

- 发布Milk-V Duo YOLO实战课程更新到第10集
[项目方案](https://github.com/DuoQilai/PLCT-Works/blob/main/RISC-V_short_video/Project_proposal_yolo11_new.md)
  - 2.3 FFmpeg的安装与使用(下)
  - 2.3 X-AnyLabeling的安装与使用（上）
  - 2.3 X-AnyLabeling的安装与使用（下）
  - 3.1 设置模型参数

##### 6.4.2 视频AI配音

- [【Milk-V Duo S】在 Duo S 上使用 Pico-8SEG-LED 数码管](https://www.bilibili.com/video/BV1jpLyzgE3T/?spm_id_from=333.1387.homepage.video_card.click&vd_source=417238cd96b1b549d14bcb35a9da3cf0)
- 
##### 6.4.3 RISC-V short video项目
建议和修改同学们提交的视频，发布视频和控制进度

- 项目迭代计划：

https://github.com/DuoQilai/PLCT-Works/tree/main/RISC-V_short_video/Plan_Document

- 项目迭代回溯：

https://github.com/DuoQilai/PLCT-Works/tree/main/RISC-V_short_video/Review_Document

##### 6.4.4 欧洲峰会

提交扩展摘要、海报和演示文稿

- [Abstract](https://github.com/DuoQilai/PLCT-Works/blob/main/Notes/RVDay/F3_Abstract_Building%20the%20RISC-V%20Education%20Ecosystem%20A%20Systematic%20Educational%20Contents%20Design%2C%20Remote%20Laboratories%2C%20and%20Community-Driven%20Learning.docx)
- [Poster](https://github.com/DuoQilai/PLCT-Works/blob/main/Notes/RVDay/F1_Poster_Building%20the%20RISC-V%20Education%20Ecosystem.pptx)
- [Slides](https://github.com/DuoQilai/PLCT-Works/blob/main/Notes/RVDay/Slides_Europe.pptx)

#### 6.5 朱旭昌

- PR

  - 更新了修复ACT仓库中FD测试用例的nan_boxed为16进制的pr并成功合并[#614](https://github.com/riscv-non-isa/riscv-arch-test/pull/614)

  修复列表：

  | 测试用例指令名      | 所属拓展 | 链接                                                         |
  | --------------- | -------- | ------------------------------------------------------------ |
  | fadd.d-01.S  | D        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fclass.d_b1-01.S  | D        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fcvt.d.s_b1-01.S  | D        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fcvt.d.wu_b25-01.S  | D        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fcvt.d.w_b25-01.S  | D        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fcvt.s.d_b1-01.S  | D        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fcvt.w.d_b1-01.S  | D        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fcvt.wu.d_b1-01.S  | D        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fdiv.d_b1-01.S  | D        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | feq.d_b1-01.S  | D        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fld-align-01.S  | D        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fle.d_b1-01.S  | D        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | flt.d_b1-01.S  | D        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fmadd.d_b1-01.S  | D        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fmax.d_b1-01.S  | D        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fmin.d_b1-01.S  | D        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fmin.d_b19-01.S  | D        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fmsub.d_b1-01.S  | D        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fmul.d_b1-01.S  | D        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fnmadd.d_b1-01.S  | D        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fnmsub.d_b1-01.S  | D        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fsd-align-01.S  | D        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fsgnj.d_b1-01.S  | D        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fsgnjn.d_b1-01.S  | D        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fsgnjx.d_b1-01.S  | D        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fsqrt.d_b1-01.S  | D        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fadd_b1-01.S  | F        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fclass_b1-01.S  | F        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fcvt.s.wu_b25-01.S  | F        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fcvt.s.w_b25-01.S  | F        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fcvt.w.s_b1-01.S  | F        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fcvt.wu.s_b1-01.S  | F        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fdiv_b1-01.S  | F        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | feq_b1-01.S  | F        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fle_b1-01.S  | F        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | flt_b1-01.S  | F        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | flw-align-01.S  | F        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fmadd_b1-01.S  | F        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fmax_b1-01.S  | F        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fmin_b1-01.S  | F        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fmsub_b1-01.S  | F        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fmsub_b2-01.S  | F        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fmul_b1-01.S  | F        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fmv.w.x_b25-01.S  | F        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fmv.x.w_b1-01.S  | F        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fnmadd_b1-01.S  | F        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fnmsub_b1-01.S  | F        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fsgnjn_b1-01.S  | F        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fsgnjx_b1-01.S  | F        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fsgnj_b1-01.S  | F        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fsub_b1-01.S  | F        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fsub_b2-01.S  | F        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fsw-align-01.S  | F        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | c.fld-01.S  | D_Zcd        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | c.fldsp-01.S  | D_Zcd        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | c.fsd-01.S  | D_Zcd        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | c.fsdsp-01.S  | D_Zcd        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fcvtmod.w.d_b1-01.S  | D_Zfa        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fleq.d_b1-01.S  | D_Zfa        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fleq.d_b19-01.S  | D_Zfa        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fleq_b1-01.S  | D_Zfa        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fli.d-01.S  | D_Zfa        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fltq.d_b1-01.S  | D_Zfa        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fltq_b1-01.S  | D_Zfa        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fmaxm.d_b1-01.S  | D_Zfa        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fmaxm_b1-01.S  | D_Zfa        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fminm.d_b1-01.S  | D_Zfa        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fminm_b1-01.S  | D_Zfa        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fmvh.x.d_b1-01.S  | D_Zfa        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | froundnx.d_b1-01.S  | D_Zfa        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | froundnx_b1-01.S  | D_Zfa        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fround_b1-01.S  | D_Zfa        | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | c.flw-01.S  | F_Zcf       | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | c.flwsp-01.S  | F_Zcf       | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | c.fsw-01.S  | F_Zcf       | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | c.fswsp-01.S  | F_Zcf       | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fleq_b1-01.S  | F_Zfa       | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fli.s-01.S  | F_Zfa       | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fltq_b1-01.S  | F_Zfa       | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fmaxm_b19-01.S  | F_Zfa       | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fminm_b19-01.S  | F_Zfa       | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | froundnx_b1-01.S  | F_Zfa       | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |
  | fround_b1-01.S  | F_Zfa       | [Github](https://github.com/riscv-non-isa/riscv-arch-test/blob/dev/riscv-test-suite/rv32i_m/D/src/) |

  - 提交了一个更新act的readme的pr[#620](https://github.com/riscv-non-isa/riscv-arch-test/pull/620)
  
  - 提交了修复pmp test name的bug[#619](https://github.com/riscv-non-isa/riscv-arch-test/pull/620)

- Issues

  - 调研了压缩指令缺失测试条件的测试用例[#482](https://github.com/riscv-non-isa/riscv-arch-test/issues/482)
  - 在无法运行RISCOF issue下进行评论并跟进帮助[#616](https://github.com/riscv-non-isa/riscv-arch-test/issues/616)
  - 调研了fmadd_b15-01.S过大的问题[#625](https://github.com/riscv-non-isa/riscv-arch-test/issues/625)

- 技术分享和会议
  
  - 调研了cvw-arch-verif并在技术分享上分享了[cvw-arch-verif_为wally的ACT测试用例集](https://github.com/Pagerd/PLCT/blob/main/Report/25-Mar/week79/cvw-arch-verif_为wally的ACT测试用例集.pptx)
  
  - 参加了3月11日的ACT会议，并编写了会议纪要[MD](https://github.com/Pagerd/PLCT/blob/main/Report/25-Mar/week78/ACT.md)
  
  - 主持了3月20日第99届东亚时区双周会[Link](https://community.riscv.org/e/m9tw5p/)

## SG2042 Upstream

- [[PATCH v4 0/2] Add basic SPI support for SG2042 SoC][sg2042-lk-1]: SPI 控制器补丁 第 4 版
- [[PATCH v5 0/3] Add basic SPI support for SOPHGO SG2042 SoC][sg2042-lk-2]: SPI 控制器补丁 第 5 版
- [[PATCH v6 0/3] Add basic SPI support for SOPHGO SG2042 SoC][sg2042-lk-3]: SPI 控制器补丁 第 6 版。Applied by for-next, 有望进入 v6.16。

[sg2042-lk-1]:https://lore.kernel.org/linux-riscv/20250407-sfg-spi-v4-0-30ac949a1e35@gmail.com/
[sg2042-lk-2]:https://lore.kernel.org/linux-riscv/20250422-sfg-spi-v5-0-c7f6554a94a0@gmail.com/
[sg2042-lk-3]:https://lore.kernel.org/linux-riscv/20250425-sfg-spi-v6-0-2dbe7bb46013@gmail.com/

## Milk-V Duo Upstream

- [[PATCH v3 0/3] riscv: sophgo: add mailbox support for CV18XX series SoC][cv18xx-1k-1]: Mailbox 驱动，第 3 版。

[cv18xx-1k-1]: https://lore.kernel.org/linux-riscv/20250428-cv18xx-mbox-v3-0-ed18dfd836d1@pigmoral.tech/

## RVI Collaborations

## RustSBI 小队

## TPU-MLIR （甲辰计划 J123）

### 吴欣宇

- 代码[添加BM1822相关测试](https://github.com/sophgo/cvikernel/pull/7)
- 文章[算能Cvikernel-BM1822支持介绍](https://zhuanlan.zhihu.com/p/1900232577236857857)
- 文章[算能Cvikernel中的CV1880V2](https://zhuanlan.zhihu.com/p/1900523854977308234)
- 文章[算能CviKernel项目技术介绍](https://zhuanlan.zhihu.com/p/1900613968386582054)


## LuaJIT

## gem5

受到2023年6月PLCT第一次大裁员影响，尚未招募到新的负责人。

## Spike

受到2023年6月PLCT第一次大裁员影响，尚未招募到新的负责人。

## QEMU

受到2023年6月PLCT第一次大裁员影响，尚未招募到新的负责人。

## Songsong Zhang

1. Add initial RISC-V support for CasaOS
- https://github.com/IceWhaleTech/github/pull/3
- https://github.com/IceWhaleTech/CasaOS-Gateway/pull/54
- https://github.com/IceWhaleTech/CasaOS-AppManagement/pull/211
- https://github.com/IceWhaleTech/CasaOS-LocalStorage/pull/62
- https://github.com/IceWhaleTech/CasaOS-UserService/pull/46
- https://github.com/IceWhaleTech/CasaOS-CLI/pull/39
- https://github.com/IceWhaleTech/get/pull/16
- https://github.com/IceWhaleTech/CasaOS-MessageBus/pull/62
- https://github.com/IceWhaleTech/CasaOS/pull/2206

## 参考链接

- [PLCT 实验室的开放职位（实习生）](https://github.com/plctlab/weloveinterns/blob/master/open-internships.md)
- [PLCT 开源进展 (PLCT Weekly)](https://github.com/plctlab/PLCT-Weekly)
- [PLCT 公开报告幻灯片（选集）](https://github.com/plctlab/PLCT-Open-Reports)
