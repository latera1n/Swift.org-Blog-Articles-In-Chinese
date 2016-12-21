# Swift 3.1 发布进程

[阅读原文>>](https://swift.org/blog/swift-3-0-release-process/)

2016 年 12 月 9 日&nbsp;&nbsp;&nbsp;&nbsp;[Ted Kremenek](https://github.com/tkremenek/)

这篇文章描述了 Swift 3.1 的目标、发布过程以及预估的时间表。

Swift 3.1 旨在与 Swift 3.0 [源码兼容](https://swift.org/blog/swift-3-1-release-process/#source-compatibility)。它将包含对核心语言的一些附加增强，以及对 Swift 软件包管理器、Linux 上的 Swift 以及对编译器和标准库的一般质量改进的改善。

Swift 3.1 将预定于在 2017 年春季发布。

## 源码兼容性

源码兼容性是一个强大的目标，即继续使用 Swift 3.1 编译器构建那些使用 Swift 3.0 编译器构建的绝大多数源代码。有一个例外将是编译器 bug 的修复，这将导致编译器拒绝那些本来就不应该被接受的代码。而这些案例在实践中是相对罕见的。

有关 Swift 版本的源码兼容性的意图的描述可以在 [swift-evolution](https://lists.swift.org/mailman/listinfo/swift-evolution) 邮件列表上的一个[帖子](https://lists.swift.org/pipermail/swift-evolution/Week-of-Mon-20161128/029099.html)中找到。

如果遇到 Swift 3.1 编译器意外拒绝之前使用 Swift 3.0 编译器所编译的代码，请提交[错误报告](https://bugs.swift.org/)。

## Swift 3.1 的快照

以前的版本的 Swift 已有“开发人员预览”，例如“预览1”、“预览2”等，它们代表 Swift 版本在开始汇集时的稳定快照。开发者预览经常是有不规则间隔的，有时没有为 Swift 社区提供足够的粒度去尝试其中的新功能或是验证这个版本在汇集时的错误修复。

而对于 Swift 3.1，取而代之的将会是发布分支中每日可下载的快照。快照将作为[持续集成](https://ci.swift.org/)测试的一部分生成。因此，可下载快照的节奏将会变得更频繁且粒度更高的。假设测试通过，快照将于每天发布。

一旦 Swift 3.1 正式发布，除了快照之外，还将发布官方最终版本。

## 把改进带入 Swift 3.1

Swift 3.1 旨在提供一个有限范围更改的版本，并希望在 2017 年初将重点转移到 Swift 4 的开发。为了实现这一目标，Swift 3.1 将仅包括 1 月 16 日之前的主线开发（例如 `master` 分支）中的更改。在那之后，将有一个“烘焙”期间，其中只有经过挑选的、关键的修复将进入 `swift-3.1-branch`，且 `master` 分支将会被移动到 Swift 4 的开发中。

### 分支

* __master__：除了 [swift-llvm](https://github.com/apple/swift-llvm) 和 [swift-clang](https://github.com/apple/swift-clang) 代码仓库（参见[受影响的代码仓库](#受影响的代码仓库)）的例外，Swift 3.1 的开发将在 `master` 分支中进行。在 `master` 分支中的所有更改将成为 Swift 3.1 最终版本的一部分，直到 1 月 16 日。在那之后 `master` 分支将跟踪 Swift 4 的开发工作。

* __swift-3.1-branch__：Swift 3.1 的发布管理发生将在 `swift-3.1-branch` 分支上。所有 Swift 3.1 的快照都是从这个分支创建的，Swift 3.1 也将从这个分支产生 GM 版本。

在操作上，`master` 分支将被定期合并到 `swift-3.1-branch` 分支中，大约每两周一次，直到1月16日。两周的窗口为 `master` 分支中的热开发和策划发布分支之间提供了一个缓冲区。在 `master` 分支的每次分支合并之间，其中的更改可以被遴选（通过拉取请求）到 `swift-3.1-branch` 分之中。

这个计划的一个显著的例外是 [swift-package-manager](https://github.com/apple/swift-package-manager) 代码仓库，它将每天从 `master` 分支合并新的更改到 `swift-3.1-branch` 分支。

### 把更改带入 Swift 3.1 的理念

* 与 Swift 3.0 源代码兼容拥有首要的优先权。

* 当 Swift 3.1 汇集后，只会考虑与发布的核心目标一致的更改。

* Swift 3.1 中的所有语言和 API 方面的更改都将通过 [Swift 演进](https://github.com/apple/swift-evolution)过程。

* Swift 3.1 的主要工作应围绕 1 月 16 日的日期进行，但之后仍可以经发布经理判断后，将改动带入 Swift 3.1。随着发布版本的汇集，将更改拉入到 Swift 3.1 的标准将变得越来越受限。

## 受影响的代码仓库

以下代码仓库将使用 `swift-3.1-branch` 分支作为 Swift 3.1 发行版的一部分来跟踪代码源：

* [swift](https://github.com/apple/swift)
* [swift-lldb](https://github.com/apple/swift-lldb)
* [swift-cmark](https://github.com/apple/swift-cmark)
* [swift-llbuild](https://github.com/apple/swift-llbuild)
* [swift-package-manager](https://github.com/apple/swift-package-manager)
* [swift-corelibs-libdispatch](https://github.com/apple/swift-corelibs-libdispatch)
* [swift-corelibs-foundation](https://github.com/apple/swift-corelibs-foundation)
* [swift-corelibs-xctest](https://github.com/apple/swift-corelibs-xctest)
* [swift-llvm](https://github.com/apple/swift-llvm)
* [swift-clang](https://github.com/apple/swift-clang)

请注意，[swift-llvm](https://github.com/apple/swift-llvm) 和 [swift-clang](https://github.com/apple/swift-clang) 代码仓库已经从 `master` 分支创建了分支 `swift-3.1-branch`，并且不会再次重新创建此分支。

## 发布经理

发布的整体管理将由以下人员监督，在发布版本逐渐汇集，他们将宣布何时对 Swift 3.1 版本更严格的更改控制将会生效：

* [Ted Kremenek](https://github.com/tkremenek) 是 Swift 3.1 的发布经理。

* [Frédéric Riss](https://github.com/fredriss) 是 [swift-llvm](https://github.com/apple/swift-llvm) 和 [swift-clang](https://github.com/apple/swift-clang) 的发布经理。

* [Jason Gosnell](https://github.com/gosnellj) 是 [swift-lldb](https://github.com/apple/swift-lldb) 的发布经理。

* [Tony Parker](https://github.com/parkera) 是 [swift-corelibs-foundation](https://github.com/apple/swift-corelibs-foundation) 的发布经理。

* [Daniel Steffen](https://github.com/das) 是 [swift-corelibs-libdispatch](https://github.com/apple/swift-corelibs-libdispatch) 的发布经理。

* [Brian Croom](https://github.com/briancroom) 是 [swift-corelibs-xctest](https://github.com/apple/swift-corelibs-xctest) 的发布经理。

* [Rick Ballard](https://github.com/rballard) 是 [swift-package-manager](https://github.com/apple/swift-package-manager) 的发布经理。

有关发布管理流程的任何问题，请随时直接发送电子邮件给 [swift-dev](https://lists.swift.org/mailman/listinfo/swift-dev) 或 [Ted Kremenek](https://github.com/tkremenek) 。

## 发布分支的拉取请求

对于所有提交要求包含在发布分支中更改的拉取请求应包含以下信息：

* __说明__：一个对于修复了的问题和做出了的增强的描述，应言简意赅。

* __范围__：一个对于其变更的影响/重要性的评估。比如，这个改变是否使得已有代码割裂开来等等。

* __服务请求（SR）的问题__：如果此变更修复/实现了 [bugs.swift.org](https://bugs.swift.org/) 列出的问题/增强功能，请写明该服务请求。

* __风险__：采用这个变更的（具体的）风险是什么？

* __测试__：哪些具体的测试已经做了或者需要进行去进一步验证这个变更的影响？

受到影响部分的一个或多个[代码所有者](https://swift.org/community/#code-owners)应该审核该变更。技术审核可以由代码所有者委托或请求，如果认为该请求是合适的或有用的。

__所有更改__（外部的更改将通过拉取请求自动从 `master` 分支合并）都必须__经由拉取请求__并经过相应发布经理同意，方可进入 `swift-3.1-branch` 分支。

<br />
<sub>Original article: [https://swift.org/blog/swift-3-1-release-process/](https://swift.org/blog/swift-3-1-release-process/)</sub>

<sup>Original article copyright © 2016 Apple Inc. All rights reserved. Swift and the Swift logo are trademarks of Apple Inc.</sup>







