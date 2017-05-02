# Swift 3.1 现已发布！

[阅读原文>>](https://swift.org/blog/swift-3-1-released/)

2017 年 3 月 27 日

Swift 3.1 现已正式发布！Swift 3.1 是一个次要版本，其中包含对标准库的更改和改进。感谢 IBM 和社区其他成员的努力，使得其中还包括许多 Swift 在 Linux 上实现的更新。此外还有一些 Swift 软件包管理器的更新。

### 语言改变

Swift 3.1 是一个次要的语言发布版本。它与 Swift 3.0 兼容，并包含以下语言改变和更新，其中大部分是通过 Swift [演进过程](https://swift.org/contributing/#participating-in-the-swift-evolution-process)进行的：

#### 全新的 `Sequence` 协议成员

 `Sequence` 协议现已新增了两个成员：

>```swift
> protocol Sequence {
>   // ...
>   /// 返回一个子序列：跳过当 `predicate` 返回 `true` 时的元素并返回剩下的元素。
>   func drop(while predicate: (Self.Iterator.Element) throws -> Bool) rethrows -> Self.SubSequence
>   /// 返回一个子序列：返回最开始的元素直到 `predicate` 返回 `false` 并跳过剩下的元素。
>   func prefix(while predicate: (Self.Iterator.Element) throws -> Bool) rethrows -> Self.SubSequence
> }
>```

查看更多：[SE-0045: 添加 `prefix(while:)` 和 `drop(while:)` 至标准库](https://github.com/apple/swift-evolution/blob/master/proposals/0045-scan-takewhile-dropwhile.md)

#### 根据 Swift 版本提供的可用性

Swift 3.1 扩展了 `@availability` 属性以使用 Swift 版本来指示声明的生命周期。例如，在 Swift 3.1 中已移除的 API 将被写为：

>```swift
> @available(swift, obsoleted: 3.1)
> class Foo {
>   //...
> }
>```

查看更多：[SE-0041: 根据 Swift 版本提供的可用性](https://github.com/apple/swift-evolution/blob/master/proposals/0141-available-by-swift-version.md)

#### 增强的数字转换初始化器

Swift 3.1 为所有数字类型添加了一系列新的转换初始值，使其可以成功完成转换且不会丢失信息或返回空。

查看更多：[SE-0080: 可失败的数字转换初始化器](https://github.com/apple/swift-evolution/blob/master/proposals/0080-failable-numeric-initializers.md)

#### UnsafeMutablePointer.initialize(from:) 的弃用和替换

接受一个 `Collection` 的 `UnsafeMutablePointer.initialize(from:)` 的版本已经弃用，以支持 `UnsafeMutableBufferPointer` 上的一个接受 `Sequence` 的新方法，这样做的目的是为了提高内存安全性，并使序列中的内存能够更快地初始化。

查看更多：[SE-0147: 改动 UnsafeMutablePointer.initialize(from:) 以使用 UnsafeMutableBufferPointer](https://github.com/apple/swift-evolution/blob/master/proposals/0147-move-unsafe-initialize-from.md)

#### Linux 实现的增强

* `NSDecimal` 的实现
* `NSLengthFormatter` 的实现
* `Progress` 的实现
* `URLSession` 功能的许多改进，包括 API 覆盖和 `libdispatch` 的优化使用
* `NSArray`，`NSAttributedString` 等其他许多 API 覆盖的增强
* `Data` 中的显着改善性能。[点击此处查看细节](https://github.com/apple/swift-corelibs-foundation/blob/master/Docs/Performance%20Refinement%20of%20Data.md)
* JSON 序列化增强的性能
* `NSUUID`，`NSURLComponents` 及其他中的内存泄漏的修复
* 增强的了测试覆盖，特别是在 `URLSession` 中

### 软件包管理器的更新

#### 可编辑的软件包

默认情况下，软件包依赖关系存储在由工具管理的构建目录中，并且新的 `swift package edit` 命令允许用户在软件包上“开始编辑”，这将使用户获得对其的控制（进入 `Package` 目录）、免除依赖关系的更新、并允许用户提交并推送对该包的更改。

查看更多：[SE-0082: 软件包管理器的可编辑软件包](https://github.com/apple/swift-evolution/blob/master/proposals/0082-swiftpm-package-edit.md)

#### 版本的钉固

你使用的每个依赖关系的版本现在将被记录在一个 `Package.pins` 文件中，你可以在此登记以便与软件包的其他用户共享这些版本。`swift package pin` 和 `swift package unpin` 命令提供了进一步的控制。解决依赖关系时，默认情况下会获取软件包依赖关系的固定版本，但是 `swift package update` 命令将会重新解析为这些依赖关系的最新允许版本，并更新钉固文件。

查看更多：[SE-0145: 软件包管理器版本的钉固](https://github.com/apple/swift-evolution/blob/master/proposals/0145-package-manager-version-pinning.md)

#### 工具的版本

软件包现在可以指定所需的 Swift 工具的最低版本。 可以使用 `swift package tools-version` 命令编辑此要求，并记录在 `Package.swift` 清单的顶部。需要 Swift 工具的软件包的更新版本将在依赖关系解析时所忽略，因此软件包可以采用新的 Swift 功能，而不会破坏正在使用旧版 Swift 工具的客户端。 所需工具的最低版本决定了哪个 Swift 语言版本用于解释 `Package.swift` 清单，以及哪个版本的 `PackageDescription` API 可用。

查看更多：[SE-0152: 软件包管理器工具版本](https://github.com/apple/swift-evolution/blob/master/proposals/0152-package-manager-tools-version.md)

#### Swift 语言的兼容性版本

软件包现在可以指定它们其中的源代码是以 Swift 3 或 Swift 4 语言版本编写的。如果未指定，则从软件包的最小 Swift 工具版本推断出默认值。

查看更多：[SE-0151: 软件包管理器 Swift 语言的兼容性版本](https://github.com/apple/swift-evolution/blob/master/proposals/0152-package-manager-tools-version.md)

#### 其他软件包管理器的提升

* 在以前可能已被解析为不正确的依赖关系的一些情况下，软件包依赖解关系的析现在已经被修正。现在，依赖关系周期将在构建时期被检测，并且由于增量构建，它将会重新构建尽可能少的源代码。

* `swift test` 命令现在支持使用 `--parallel` 标志来并行地运行测试。`swift build`、`swift test` 和所有 `swift package` 中解析依赖关系的命令现在支持使用 `--enable-prefetching` 标志来并行获取这些依赖关系。

Swift 软件包管理器的相关文档可以[在代码仓库中](https://github.com/apple/swift-package-manager/tree/swift-3.1-branch/Documentation)找到。

### 迁移到 Swift 3.1

Swift 3.1 与 Swift 3.0 源兼容。为了帮助你移动到 Swift 3.1，[Xcode 8.3](https://itunes.apple.com/app/xcode/id497799835) 包含了一个代码迁移器，可以自动处理许多需要改变源代码的更改。我们还提供了一个[迁移指南](https://swift.org/migration-guide/)，可以指导你完成许多更改——特别是帮你完成那些不是很机械的且需要更多直接审查的更改。

### 文档

[《Swift 编程语言》](https://swift.org/documentation/#the-swift-programming-language)的 Swift 3.1 更新版本现可在 Swift.org 上获取。也可在 Apple 的 iBooks 商店中免费下载。

### 平台

#### Linux（Ubuntu 14.04、Ubuntu 16.04 和 Ubuntu 16.10）

Ubuntu 14.04、Ubuntu 16.04 和 Ubuntu 16.10 正式版的二进制文件已[可供下载](https://swift.org/download/)。

#### Apple（Xcode）

关于 Apple 平台上的开发，Swift 3.1 是作为 [Xcode 8.3](https://itunes.apple.com/app/xcode/id497799835) 的一部分进行分发的。

### 源

Swift 3.1 的开发是通过以下在 Github 上代码仓库的 `swift-3.1-branch` 分支来进行追踪的：

* [swift](https://github.com/apple/swift)
* [swift-llvm](https://github.com/apple/swift-llvm)
* [swift-clang](https://github.com/apple/swift-clang)
* [swift-lldb](https://github.com/apple/swift-lldb)
* [swift-cmark](https://github.com/apple/swift-cmark)
* [swift-corelibs-foundation](https://github.com/apple/swift-corelibs-foundation)
* [swift-corelibs-libdispatch](https://github.com/apple/swift-corelibs-libdispatch)
* [swift-corelibs-xctest](https://github.com/apple/swift-corelibs-xctest)
* [swift-llbuild](https://github.com/apple/swift-llbuild)
* [swift-package-manager](https://github.com/apple/swift-package-manager)
* [swift-xcode-playground-support](https://github.com/apple/swift-xcode-playground-support)
* [swift-compiler-rt](https://github.com/apple/swift-compiler-rt)

在那些仓库中的 `swift-3.1-RELEASE` 标签指定了组成 Swift 3.1 最终版本的具体修订。

`swift-3.1-branch` 分支将继续开放——但在同样的[发布管理过程](https://swift.org/blog/swift-3-0-release-process/)下——以累积更改以用于在将来潜在的，作为漏洞修复的“点”版本发布。

<br />
<sub>Original article: <a href="https://swift.org/blog/swift-3-1-released/">https://swift.org/blog/swift-3-1-released/</a></sub>

<sup>Original article copyright © 2017 Apple Inc. All rights reserved. Swift and the Swift logo are trademarks of Apple Inc.</sup>
