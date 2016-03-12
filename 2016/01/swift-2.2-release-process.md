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


[original-article]: https://swift.org/blog/swift-2-2-release-process/
[swift-core-library]: https://swift.org/core-libraries/
[swift-package-manager]: https://swift.org/package-manager/
[swift]: https://github.com/apple/swift
[swift-llvm]: https://github.com/apple/swift-llvm
[swift-clang]: https://github.com/apple/swift-clang
[swift-lldb]: https://github.com/apple/swift-lldb
[swift-cmark]: https://github.com/apple/swift-cmark
