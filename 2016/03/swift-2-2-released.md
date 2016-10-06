# Swift 2.2 现已发布！

[阅读原文>>](https://swift.org/blog/swift-2-2-released/)

2016 年 3 月 21 日&nbsp;&nbsp;&nbsp;&nbsp;[Ted Kremenek](https://github.com/tkremenek/)

我们很高兴地在此宣告 Swift 2.2 的发布！这是 Swift 自从 2015 年 12 月 3 日开源以来的的首次正式发布。值得注意的是，此次发布包括来自 212 非 Apple 的贡献者所做出的贡献——从简单的漏洞修复到核心语言，以及 Swift 标准库的增强和改进。

### 语言改变

Swift 2.2 是一个较小的且在大部分代码上可以兼容 Swift 2.1 的语言发行版。它包含以下经历了 Swift [演进过程](https://swift.org/contributing/#participating-in-the-swift-evolution-process) 的语言改变：

* [SE-0001：允许（大部分）关键字被用于参数标签](https://github.com/apple/swift-evolution/blob/master/proposals/0001-keywords-as-argument-labels.md)
* [SE-0015：元组比较运算符](https://github.com/apple/swift-evolution/blob/master/proposals/0015-tuple-comparison-operators.md)
* [SE-0014：限制 `AnySequence.init`](https://github.com/apple/swift-evolution/blob/master/proposals/0014-constrained-AnySequence.md)
* [SE-0011：替换 `typealias` 关键字为 `associatedtype` 用于声明关联类型](https://github.com/apple/swift-evolution/blob/master/proposals/0011-replace-typealias-associated.md)
* [SE-0021：使用参数标签来命名函数](https://github.com/apple/swift-evolution/blob/master/proposals/0021-generalized-naming.md)
* [SE-0022：引用一个方法的 Objective-C 选择器](https://github.com/apple/swift-evolution/blob/master/proposals/0022-objc-selectors.md)
* [SE-0020：Swift 语言版本构建配置](https://github.com/apple/swift-evolution/blob/master/proposals/0020-if-swift-version.md)

除了这些语言的变化，Swift 2.2 还包含了大量的漏洞修复、诊断的增强，并可以产生更快的运行代码。

[Swift 软件包管理器](https://swift.org/package-manager/)还处于开发初期，并没有包含在这个版本中。

### 文档

[《Swift 编程语言》](https://swift.org/documentation/#the-swift-programming-language)的 Swift 2.2 更新版本现可在 Swift.org 上获取。也可在 Apple 的 iBooks 商店中免费下载。

### 平台

#### Linux（Ubuntu 14.04 和 Ubuntu 15.10）

Swift 2.2 包括 Linux 上的 Swift 支持。由于 Linux 的移植相对比较新，因此在此版本中不包括 [Swift 核心库](https://swift.org/core-libraries/)（它将出现在 Swift 3 中）。但是此移植包括 LLDB 和 REPL。

Ubuntu 14.04 和 Ubuntu 15.10 正式版的二进制文件已[可供下载](https://swift.org/download/)。

#### Apple（Xcode）

关于 Apple 平台上的开发，Swift 2.2 是作为 Xcode 7.3 的一部分进行分发的。

### 源

Swift 2.2 的开发是通过以下在 Github 上仓库的 `swift-2.2-branch` 分支来进行追踪的：

* [swift](https://github.com/apple/swift)
* [swift-llvm](https://github.com/apple/swift-llvm)
* [swift-clang](https://github.com/apple/swift-clang)
* [swift-lldb](https://github.com/apple/swift-lldb)
* [swift-cmark](https://github.com/apple/swift-cmark)

在那些仓库中的 `swift-2.2-RELEASE` 标签指定了组成 Swift 2.2 最终版本的具体修订。

`swift-2.2-branch` 分支将继续开放 —— 但在同样的发布管理过程下 —— 以累积更改以用于在将来潜在的，作为漏洞修复的“点”版本发布。

<br />
<sub>Original article: [https://swift.org/blog/swift-2-2-released/](https://swift.org/blog/swift-2-2-released/)</sub>

<sup>Original article copyright © 2016 Apple Inc. All rights reserved. Swift and the Swift logo are trademarks of Apple Inc.</sup>
