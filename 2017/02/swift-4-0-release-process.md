# Swift 4 发布进程

[阅读原文>>](https://swift.org/blog/swift-4-0-release-process/)

2017 年 2 月 16 日&nbsp;&nbsp;&nbsp;&nbsp;[Ted Kremenek](https://github.com/tkremenek/)

这篇文章描述了 Swift 4 的目标、发布过程以及预估的时间表。

Swift 4 是一个主要的发布版本，预定于 2017 年秋天完成。它围绕着 Swift 3 代码展开，为其提供源代码稳定性，同时为语言的二进制稳定性进行了一些所需要的基础功能的工作。Swift 4 将包含对核心语言和标准库的显著增强，特别是对于泛型系统和字符串类型的改进。更多细节可以在 [Swift 演进页面](https://github.com/apple/swift-evolution#development-major-version--swift-40)找到。

## 源码兼容性

Swift 4 的编译器将提供两种语言模式：`-swift-version 3` 和 `-swift-version 4`。

#### Swift 版本 3 模式

第一种模式 `-swift-version 3` 是现有代码的默认模式。在这种模式下，一个强有力的目标就是使用 Swift 4 编译器来继续构建那些使用 Swift 3.1 编译器构建的绝大多数源代码。其中的例外将是修复编译器的错误，这会导致编译器拒绝编译那些一开始就不应该被接受的源代码。而这些情况在实践中应该相对较少地出现。

如果遇到 Swift 4 编译器意外地拒绝以前使用 Swift 3.1 编译器编译的代码的情况，请提交[错误报告](https://bugs.swift.org/)。

#### Swift 版本 4 模式

预期的设计是，包含多个 Swift 模块的项目，例如具有多个 Swift 目标的 Xcode 项目，将能够对每个模块（目标）级别采用特定的 Swift 语言模式，并且它们可以与同一编译的二进制文件自由交互。请注意，当这些目标使用相同的编译器编译时，此互操作性仅存在于二进制层级。

以下例子解释了此互操作性实现了哪些内容：

* 在 Xcode 中，应用程序目标可以在 Swift 4（即 `-swift-version 4`）中编写，但是项目中可以存在另外的一些被该应用程序使用的且由 Swift 3 编写的框架目标（即 `-swift-version 3`）。

* 以 Swift 4 编写的 Swift 包（即 `-swift-version 4`）应该能够使用以 Swift 3 编写的现有包，而不需要将其源代码更新为 Swift 4 版本。

总体而言，该方案将允许现有的 Swift 3 代码逐渐迁移到 Swift 4（例如，每次一个目标或软件包）。

可以在 [swift-evolution](https://lists.swift.org/mailman/listinfo/swift-evolution) 讨论组的[帖子](https://lists.swift.org/pipermail/swift-evolution/Week-of-Mon-20161128/029099.html)里找到Swift版本的源兼容性的更详细的描述。

## Swift 4 的快照

就像 Swift 3.1 那样，Swift 4 将有每日可下载的发行版快照。这些生成的快照将作为持续集成测试的一部分。因此，可下载快照的节奏将更加频繁和细化。如果测试通过，快照将会于每天发布。

一旦 Swift 4 发布，除了快照之外，官方的最终版本也将发布。

## 把改进带入 Swift 4

所有改进目前全部进入主线开发（即 master 主分支），直到发行经理公布最后一个分支的日期，这可能是在 2017 年初夏的某个时间。在那之后将存在一个“烘烤”的时期，那段时间里仅经过选择的、关键的修复将进入 `swift-4.0` 分支，而 `master` 主分支将被用于下一个发布版本的开发。

#### 分支

* __master__：除了 [swift-llvm](https://github.com/apple/swift-llvm)、[swift-clang](https://github.com/apple/swift-clang) 和 [swift-lldb](https://github.com/apple/swift-lldb) 代码仓库（参见[受影响的代码仓库](#受影响的代码仓库)）的例外情况，Swift 4 的开发将在 `master` 分支中进行。在 `master` 分支中的所有更改将成为 Swift 4 最终版本的一部分，直到最终分支日期。在那之后 `master` 分支将被用来跟踪下一个发布版本的开发工作。

* __swift-4.0-branch__：Swift 4 的发布管理发生将在 `swift-4.0-branch` 分支上。所有 Swift 4 的快照都是从这个分支创建的，Swift 4 也将从这个分支产生 GM 版本。

在操作上，`master` 分支将被定期合并到 `swift-4.0-branch` 分支中，大约每两周一次，直到最终的分支日期。两周的窗口为 `master` 分支中的热开发和策划发布分支之间提供了一个缓冲区。在 `master` 分支的每次分支合并之间，其中的更改可以被遴选（通过拉取请求）到 `swift-4.0-branch` 分支中。

这个计划的一个显著的例外是 swift-package-manager 代码仓库，它将每天从 master 分支合并新的更改到 swift-4.0-branch 分支。

## 把更改带入 Swift 4 的理念

* 与 Swift 3.1 的源码兼容性是 `-swift-version 3` 模式的首要任务。

* 当 Swift 4 汇集后，只会考虑与发布的核心目标一致的更改。

* Swift 4 中的所有语言和 API 方面的更改都将经历 [Swift 演进](https://github.com/apple/swift-evolution)过程，其中列出了发布文档的更改范围的标准。

* 随着发布版本的汇集，将更改拉入到 Swift 4 的标准将变得越来越受限。

## 受影响的代码仓库

以下代码仓库将使用 `swift-4.0-branch` 分支作为 Swift 4 发行版的一部分来跟踪代码源：

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

请注意，[swift-llvm](https://github.com/apple/swift-llvm)、[swift-clang](https://github.com/apple/swift-clang) 和 [swift-lldb](https://github.com/apple/swift-lldb) 代码仓库已经从 `master` 分支创建了 `swift-3.1-branch` 分支，并且不会再次重新创建此分支。

## 发布经理

发布的整体管理将由以下人员监督，在发布版本逐渐汇集，他们将宣布何时对 Swift 4 版本更严格的更改控制将会生效：

* [Ted Kremenek](https://github.com/tkremenek) 是 Swift 4 的发布经理。

* [Frédéric Riss](https://github.com/fredriss) 是 [swift-llvm](https://github.com/apple/swift-llvm) 和 [swift-clang](https://github.com/apple/swift-clang) 的发布经理。

* [Ben Cohen](https://github.com/airspeedswift) 是 Swift 核心库的发布经理。

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

__所有更改__（外部的更改将通过拉取请求自动从 `master` 分支合并）都必须__经由拉取请求__并经过相应发布经理同意，方可进入 `swift-4.0-branch` 分支。

<br />
<sub>Original article: [https://swift.org/blog/swift-4-0-release-process/](https://swift.org/blog/swift-4-0-release-process/)</sub>

<sup>Original article copyright © 2017 Apple Inc. All rights reserved. Swift and the Swift logo are trademarks of Apple Inc.</sup>
