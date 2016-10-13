# Swift 3.0 发布进程

[阅读原文>>](https://swift.org/blog/swift-3-0-release-process/)

2016 年 5 月 6 日&nbsp;&nbsp;&nbsp;&nbsp;[Ted Kremenek](https://github.com/tkremenek/)

这篇文章描述了 Swift 3.0 的目标、发布过程以及预估的时间表。

Swift 3.0 是一个与 Swift 2.2 不兼容的主要版本。它包含对于语言和 Swift 标准库的根本改变。有关在 Swift 3.0 中已经实现的更改的完整列表，请参阅 [Swift 演进站点](https://github.com/apple/swift-evolution#implemented-proposals-for-swift-3)。

Swift 3.0 也是第一个包含 [Swift 软件包管理器](https://swift.org/package-manager/)的版本。虽然 Swift 软件包管理器仍处于早期开发阶段，但它支持跨平台 Swift 软件包的开发与分发。Swift 软件包管理器将在 Darwin 和 Linux 上都可用。

对于 Linux，Swift 3 也将是第一个包含 [Swift 核心库](https://swift.org/core-libraries/)的版本。

Swift 3.0 预计将在 2016 年下半年发布。除了 Swift.org 的发布版，Swift 3.0 也将在未来版本的 Xcode 中提供。

## 开发者预览

* Swift 3.0 将有一系列的开发者预览（即“种子”或“beta版”），提供合格及融合的 Swift 3 构建版本。其目标是为用户提供更稳定且更合格的 Swift 二进制文件，他们可以[下载](https://swift.org/download)和试用 （并对此报告 bug 信息），而不是仅仅抓住 `master` 分支最新快照。

* 开发者预览的发布节奏可能并不规则，但很可能是每 4-6 周。发布间隔将部分地由进入 `master` 分支的代码变化量的多少以及使开发者预览分支变得稳定所需要时间的多少来决定。

* Swift 3.0 将从最后一个开发者预览分支开始被公告为“GM”。

* 进入开发者预览的内容将由相应的发布经理管理（见下文）。

## 获取 Swift 3.0 的更改

#### 分支
* __master__：Swift 3.0 的开发在 `master` 中产生。在 `master` 中的所有更改将成为 Swift 3.0 最终版本的一部分，一直到最后一个开发者预览分支被创建为止。在那时，`master`  将被用来追踪未来 Swift 版本的开发。

* __swift-3.0-preview-\<X\>-branch__：开发者预览的分支将从 `master` 来创建。对这些分支的所有更改都需要通过启动持续集成测试的拉取请求来进行提交。指定代码仓库的发布经理将批准把拉取请求合并到开发者预览分支中。

* __swift-3.0-branch__：从 `master` 分支创建的最后一个开发者预览分支将被称为 `swift-3.0-branch`。这将是最终的“发布分支”。

#### 把更改带入 Swift 3.0 的理念

* Swift 3.0 只会考虑与那些与发布的核心目标一致的更改进行汇集。

* 那些对语言的破坏性的更改将根据具体情况予以考虑。

* Swift 3.0 中的所有语言和 API 方面的更改都将通过 [Swift 演进](https://github.com/apple/swift-evolution)过程。

* 发布经理设置的标准——接受更改的限制将随着发行版汇集过程的时间变化而变得越来越严格。相同的策略也适用于本质上是迷你发布版本的开发者预览分支。

### 时间表

* 第一个开发者预览分支 `swift-3.0-preview-1-branch` 将从 5 月 12 日的 `master` 分支创建。它将在 4-6 周后被释放。

* 创建最后一个开发者预览分支——`swift-3.0-branch`——的日期尚未确立。当该日期确定后，计划将在 [swift-dev](https://lists.swift.org/mailman/listinfo/swift-dev) 中传达并且在这篇文章内更新。

### 受影响的代码仓库

以下代码仓库将使用 `swift-3.0-preview-<X>-branch`/`swift-3.0-branch` 分支作为 Swift 3.0 发行版的一部分来跟踪代码源：

* [swift](https://github.com/apple/swift)
* [swift-lldb](https://github.com/apple/swift-lldb)
* [swift-cmark](https://github.com/apple/swift-cmark)
* [swift-llbuild](https://github.com/apple/swift-llbuild)
* [swift-package-manager](https://github.com/apple/swift-package-manager)
* [swift-corelibs-libdispatch](https://github.com/apple/swift-corelibs-libdispatch)
* [swift-corelibs-foundation](https://github.com/apple/swift-corelibs-foundation)
* [swift-corelibs-xctest](https://github.com/apple/swift-corelibs-xctest)

以下代码仓库将只有一个 `swift-3.0` 分支，而不是开发者预览分支，因为它们已经卓有成效地汇集了一些更改：

* [swift-llvm](https://github.com/apple/swift-llvm)
* [swift-clang](https://github.com/apple/swift-clang)

# 发布经理

发布的整体管理将由以下人员监督，在发布版本逐渐汇集，他们将宣布何时对 Swift 3.0 版本更严格的更改控制将会生效：

* [Ted Kremenek](https://github.com/tkremenek) 是 Swift 3.0 的发布经理。

* [Frédéric Riss](https://github.com/fredriss) 是 [swift-llvm](https://github.com/apple/swift-llvm) 和 [swift-clang](https://github.com/apple/swift-clang) 的发布经理。

* [Kate Stone](https://github.com/k8stone) 是 [swift-lldb](https://github.com/apple/swift-lldb) 的发布经理。

* [Tony Parker](https://github.com/parkera) 是 [swift-corelibs-foundation](https://github.com/apple/swift-corelibs-foundation) 的发布经理。

* [Daniel Steffen](https://github.com/das) 是 [swift-corelibs-libdispatch](https://github.com/apple/swift-corelibs-libdispatch) 的发布经理。

* [Mike Ferris](https://github.com/mike-ferris-apple) 是 [swift-corelibs-xctest](https://github.com/apple/swift-corelibs-xctest) 的发布经理。

* [Rick Ballard](https://github.com/rballard) 是 [swift-package-manager](https://github.com/apple/swift-package-manager) 的发布经理。

有关发布管理流程的任何问题，请随时直接发送电子邮件给 [swift-dev](https://lists.swift.org/mailman/listinfo/swift-dev) 或 [Ted Kremenek](https://github.com/tkremenek) 。

### 开发者预览的拉取请求

对于所有提交要求包含在开发者预览分支中更改的拉取请求应包含以下信息：

* __说明__：一个对于修复了的问题和做出了的增强的描述，应言简意赅。

* __范围__：一个对于其变更的影响/重要性的评估。比如，这个改变是否使得已有代码割裂开来等等。

* __服务请求（SR）的问题__：如果此变更修复/实现了 [bugs.swift.org](https://bugs.swift.org/) 列出的问题/增强功能，请写明该服务请求。

* __风险__：采用这个变更的（具体的）风险是什么？

* __测试__：哪些具体的测试已经做了或者需要进行去进一步验证这个变更的影响？

受到影响部分的一个或多个[代码所有者](https://swift.org/community/#code-owners)应该审核该变更。技术审核可以由代码所有者委托或请求，如果认为该请求是合适的或有用的。

__所有更改__都必__须经由拉取请求__并经过相应发布经理同意，方可进入开发者预览分支。

<br />
<sub>Original article: [https://swift.org/blog/swift-3-0-release-process/](https://swift.org/blog/swift-3-0-release-process/)</sub>

<sup>Original article copyright © 2016 Apple Inc. All rights reserved. Swift and the Swift logo are trademarks of Apple Inc.</sup>
