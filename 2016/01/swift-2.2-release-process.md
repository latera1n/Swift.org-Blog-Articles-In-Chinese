# Swift 2.2 发布进程

[阅读原文>>][original-article]

2016 年 1 月 5 日

这篇文章描述了 Swift 2.2 的所要达到的目标、发布进程和预估日程。

Swift 2.2 是其开源以后的第一个以开源版本发布的 Swift。它是一个与 Swift 2.1 基本上完全兼容的版本。虽然语言本身没有很显而易见的变化，但是其中包括了大量的内核提升（例如，bug 修复、代码诊断信息的增强以及更快速的代码生成）。这个版本的发布是为了充当 Swift 2 和 Swift 3 的过渡的中点，因为 Swift 3 中将包括更多的容易混淆的语言本身和标准库的变更。

发布版中将包含 Linux 平台上的 Swift，尽管它与其他平台相比相对较新而且存在已知警告。Swift 2.2 将不包含 [Swift 核心库][swift-core-library]但是将包括 LLDB 和 REPL。

[Swift 软件包管理器][swift-package-manager]仍然处在早起的开发中故将不会被包含在其中。

除了在 Swift.org 上发布之外，Swift 2.2 还会随着 Xcode 的未来版本一起分发。

## 流程
### 受影响的代码仓库
作为发布流程的一部分，下列代码仓库将会有一个 `swift-2.2-brach` 来跟踪源文件和代码：
* [swift][swift]
* [swift-llvm][swift-llvm]
* [swift-clang][swift-clang]
* [swift-lldb][swift-lldb]
* [swift-cmark][swift-cmark]

### 日程安排
* Swift 2.2 将于 2016 年 1 月 13 日从 `master` 分支出来。在 1 月 13 日后，`master` 分支将主要用于跟踪 Swift 3 的开发。上述安排尤其对 [swift][swift]、[swift-lldb][swift-lldb] 和 [swift-cmark][swift-cmark] 也适用。
* `swift-2.2-branch` 将会从先前创建的 [swift-llvm][swift-llvm] 和 [swift-clang][swift-clang] 仓库中的约为为 252576 的 LLVM 版本分支出来，这样做是为了提供更长的一段时间稳定其下层的 LLVM 平台。上述的仓库将不会从 `master` 分支重新合并。
* Swift 2.2 的最终发布日期仍待确定。因为 Swift 2.2 的本意是为了被用于新旧版本更替的交汇时期故可能会在 2016 年三月到五月之间的某个时间点发布。
* Swift 2.2 的[二进制编译][binary-builds]发布分之将会与 Swift 的 `master` 开发分支的快照一起提供。
* Swift 将会与 Swift 2.2 一样继续进行相同的分支过程并会在将来晚些时间公布。

### 准备变更到 Swift 2.2
* 根据上述规定，1 月 13 日之前的所有对 Swift 的 `master` 分支所做的修改都是 Swift 2.2 的一部分。
* 之后，仅仅一些仅语言/API 上的的改变将会被考虑纳入 Swift 2.2。这些变更应该仅为能够使源代码可以与 Swift 2.1 更兼容的改变，或是一些为源代码提供的此处将会在 Swift 3 中为编译错误的迁移警告。
* 接受更改的标准（正如发布管理者设定的那样）将会因为发布即将到来而随着时间推移变得愈加严格。
* 对于那些没有直接提交权限的参与者，对 `swift-2.2-branch` 所做的贡献可以由[拉取请求][pull-request]开始。拉取请求一般适用于拉取 `master` 中已经已经出现的改变，除非这些变化是针对 Swift 2.2 做出的而不会被包含在 Swift 3 之中。例如，一个 bug 的修复应该首先被放入 `master` 分支并随后拉取进入 swift-2.2-branch` 分支。
* 有着直接提交更改访问权限的核心贡献者将能够在日程中的一个严格管控分支更改的时间节点之前，在根据不同代码拥有者或发布管理者的指导下，直接严选或是应用变更纳入到 `swift-2.2-branch` 中。一旦 `swift-2.2-branch` 开始了严格的变更管控，我们将会在相关的部分的邮件订阅（比如 [swift-dev][swift-dev]）中对此发布一个声明。到那时所有的变更都需要经历一个通过[拉取请求][pull-request]开始的提名过程。

### 发布管理
总体上的发布管理将会被以下个人监督，随着发布的临近，他们会宣布什么时候 Swift 2.2 发布过程中更严格的变更管控将生效。
* [Ted Kremenek][ted-kremenek] 是 Swift 2.2 的总体发布管理者。
* [Bob Wilson][bob-wilson] 是 Swift 2.2 中 LLVM 以及 Clang 部分的发布管理者。
* [Kate Stone][kate-stone] 是 Swift 2.2 中 LLDB 部分的发布管理者。

如果有任何关于发布管理过程的问题，欢迎直接向 [swift-dev][swift-dev] 或 Ted 发送电子邮件。

### 拉取请求
所有的拉取请求提名包含变更至 `swift-2.2-branch` 应该包括以下信息：
* __解释__：一个对于修复的问题和做出的增强的描述，应言简意赅。
* __范围__：一个对于其变更的影响/重要性的评估。比如，这个改变是否使得已有代码割裂开来等等。
* __服务请求（SR）的问题__：如果此变更修复/实现了 [bugs.swift.org][bugs.swift.org] 列出的问题/增强功能，请写明该服务请求。
* __风险__：采用这个变更的（具体的）风险是什么？
* __测试__：哪些具体的测试已经做了或者需要进行去进一步验证这个变更的影响？

收到影响部分的一个或多个[代码所有者][code-owners]应该审核该变更。技术审核可以由代码所有者委托或请求，如果认为该请求是合适的或有用的。

在 `swift-2.2-branch` 进入严格的变更管制（发布管理者将会宣布）之前，代码管理者可以在对变更进行上述审核之后，直接接收拉取请求。一旦限制性的变更管控实施了之后，仅发布管理者被允许接收拉取请求进入  `swift-2.2-branch`。

<br />
<sub>Original article: [https://swift.org/blog/swift-2-2-release-process/][original-article]</sub>

<sup>Original article copyright © 2016 Apple Inc. All rights reserved. Swift and the Swift logo are trademarks of Apple Inc.</sup>

[original-article]: https://swift.org/blog/swift-2-2-release-process/
[swift-core-library]: https://swift.org/core-libraries/
[swift-package-manager]: https://swift.org/package-manager/
[swift]: https://github.com/apple/swift
[swift-llvm]: https://github.com/apple/swift-llvm
[swift-clang]: https://github.com/apple/swift-clang
[swift-lldb]: https://github.com/apple/swift-lldb
[swift-cmark]: https://github.com/apple/swift-cmark
[binary-builds]: https://swift.org/download/
[pull-request]: https://swift.org/blog/swift-2-2-release-process/#pull-requests
[swift-dev]: https://lists.swift.org/mailman/listinfo/swift-dev
[ted-kremenek]: https://github.com/tkremenek
[bob-wilson]: https://github.com/bob-wilson
[kate-stone]: https://github.com/k8stone
[bugs.swift.org]: https://bugs.swift.org/
[code-owners]: https://swift.org/community/#code-owners
