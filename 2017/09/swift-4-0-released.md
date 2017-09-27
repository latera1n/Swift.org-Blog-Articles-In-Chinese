# Swift 4.0 现已发布！

[阅读原文>>](https://swift.org/blog/swift-4-0-released/)

2017 年 9 月 19 日&nbsp;&nbsp;&nbsp;&nbsp;[Ted Kremenek](https://github.com/tkremenek/)

Swift 4 现已正式发布！Swift 4 在 Swift 3 强大实力的基础上，提供更优越的鲁棒性和稳定性。包括了为 Swift 3 提供源代码兼容性，对标准库进行的改进，并添加了归档和序列化等功能。

您可以通过收看 [《WWDC 2017：Swift 的新功能》](https://developer.apple.com/videos/play/wwdc2017/402/)演示以观看快速概述，并尝试 Ole Begemann 汇总的这个 [playground](https://github.com/ole/whats-new-in-swift-4) 中的一些新功能。

### 语言更新

Swift 4.0 是一个主要的语言版本，包含以下通过 Swift 演进过程的语言更改和更新：

#### 字符串

Swift 4 包括一个更快且更易用的 `String` 实现，它保留了 Unicode 的正确性，并增加了对创建、使用和管理子字符串的支持。

详情请见：

* [SE-0163 字符串修订: 集合一致性、C 语言交互操作、转码](https://github.com/apple/swift-evolution/blob/master/proposals/0163-string-revision-1.md)
* [SE-0168 多行字符串字面量](https://github.com/apple/swift-evolution/blob/master/proposals/0168-multi-line-string-literals.md)
* [SE-0178 向 `Character` 类中添加 `unicodeScalars` 属性](https://github.com/apple/swift-evolution/blob/master/proposals/0178-character-unicode-view.md)
* [SE-0180 字符串索引大修](https://github.com/apple/swift-evolution/blob/master/proposals/0180-string-index-overhaul.md)
* [SE-0182 字符串新行的转义](https://github.com/apple/swift-evolution/blob/master/proposals/0182-newline-escape-in-strings.md)
* [SE-0183 子字符串性能的可视性](https://github.com/apple/swift-evolution/blob/master/proposals/0183-substring-affordances.md)

#### 集合

Swift 4 增加了创建、使用和管理集合类型的改进。

详情请见：

* [SE-0148 通用下标](https://github.com/apple/swift-evolution/blob/master/proposals/0148-generic-subscripts.md)
* [SE-0154 为字典键值提供自定义的集合](https://github.com/apple/swift-evolution/blob/master/proposals/0154-dictionary-key-and-value-collections.md)
* [SE-0165 字典和集合类型的增强](https://github.com/apple/swift-evolution/blob/master/proposals/0165-dict.md)
* [SE-0172 单方面的范围](https://github.com/apple/swift-evolution/blob/master/proposals/0172-one-sided-ranges.md)
* [SE-0173 添加 `MutableCollection.swapAt(_:_:)`](https://github.com/apple/swift-evolution/blob/master/proposals/0173-swap-indices.md)

#### 归档和序列化

Swift 4 支持结构化和枚举类型的归档，并可以对外部格式（如 JSON 和 plist）进行类型安全的序列化。

详情请见：

* [SE-0166 Swift 归档和序列化](https://github.com/apple/swift-evolution/blob/master/proposals/0166-swift-archival-serialization.md)

#### 其他语言更改

Swift 4 还实现了以下来自 Swift 演进过程的语言提案：

* [SE-0104 面向协议的整数](https://github.com/apple/swift-evolution/blob/master/proposals/0104-improved-integers.md)
* [SE-0142 允许 where 子句以限制关联类型](https://github.com/apple/swift-evolution/blob/master/proposals/0142-associated-types-constraints.md)
* [SE-0156 类和子类型的存在关系](https://github.com/apple/swift-evolution/blob/master/proposals/0156-subclass-existentials.md)
* [SE-0160 限制 `@objc` 推断](https://github.com/apple/swift-evolution/blob/master/proposals/0160-objc-inference.md)
* [SE-0164 在协议扩展中移除对 final 的支持](https://github.com/apple/swift-evolution/blob/master/proposals/0164-remove-final-support-in-protocol-extensions.md)
* [SE-0169 提升 `private` 声明和扩展之间的相互作用](https://github.com/apple/swift-evolution/blob/master/proposals/0164-remove-final-support-in-protocol-extensions.md)
* [SE-0170 NSNumber 的桥接和数字类型](https://github.com/apple/swift-evolution/blob/master/proposals/0170-nsnumber_bridge.md)
* [SE-0171 `inout` 的 reduce 方法](https://github.com/apple/swift-evolution/blob/master/proposals/0171-reduce-with-inout.md)
* [SE-0176 强制对内存的专有访问](https://github.com/apple/swift-evolution/blob/master/proposals/0176-enforce-exclusive-access-to-memory.md)
* [SE-0179 Swift `run` 命令](https://github.com/apple/swift-evolution/blob/master/proposals/0179-swift-run-command.md)

#### 全新的兼容模式

使用 Swift 4，您可能不需要修改代码即可使用新版本的编译器。编译器支持两种语言模式：

* *Swift 3.2*：在这种模式下，编译器将接受使用 Swift 3.x 编译器构建的大多数源代码。为了提供这种级别的源兼容性，对于先前存在的 API（作为 Apple 提供的标准库或 API 的一部分）的更新将不会出现在此模式中。Swift 4 中的大部分新语言功能都以这种语言模式提供。

* *Swift 4.0*：此模式包括所有 Swift 4.0 语言和 API 更改。尽管与之前 Swift 各个版本之间的许多重大更改相比，源代码更改数量相当轻微，许多项目仍将需要一些源代码的迁移。

语言模式由 `-swift-version` 标识指定给编译器，由 Swift 软件包管理器和 Xcode 自动处理。

这些语言模式的一个优点是，您可以立刻开始使用全新的 Swift 4 编译器，且可以按照自己的节奏地，一次使用一个模块地完全迁移到 Swift 4，以利用它的新功能。

有关 Swift 4 迁移和兼容性模式的更多信息，请参阅[迁移到Swift 4](https://swift.org/migration-guide-swift4/)。

### 软件包管理器的更新

Swift 4 为 Swift 软件包管理器引入了新的工作流功能和更完整的 API：

* 在标记您的第一个正式版本之前，您可以轻松同时开发多个软件包，或者在多个软件包的分支上一起工作。
* 软件包产品已经正式化，使控制软件包发布给客户哪一个库成为可能。
* 新的软件包 API 允许软件包指定一些新的设置，使软件包的作者能够更好地控制软件包的构建方式，以及如何在硬盘上组织安排源代码。总的来说，用于创建软件包的 API 现在更整洁、更清晰，同时也保留了与旧软件包的源兼容性。
* 在 macOS 上，现在 Swift 软件包的构建发生在一个防止网络访问和文件系统修改的沙箱中，这有助于减轻恶意制作的 manifest 配置文件的影响。

此外，Swift 软件包管理器建立在 Swift 3.1（[SE-0159](https://github.com/apple/swift-evolution/blob/master/proposals/0152-package-manager-tools-version.md)）中引入的软件包管理器工具版本控制的基础上，该软件包允许软件包作者指定构建软件包所需的 Swift 版本——现在包括了 Swift 4。

有关软件包管理器增强的更多信息，请参阅：

* [SE-0146 软件包管理器产品的定义](https://github.com/apple/swift-evolution/blob/master/proposals/0146-package-manager-product-definitions.md)
* [SE-0149 软件包管理器对树顶开发的支持](https://github.com/apple/swift-evolution/blob/master/proposals/0149-package-manager-top-of-tree.md)
* [SE-0150 软件包管理器对分支的支持](https://github.com/apple/swift-evolution/blob/master/proposals/0150-package-manager-branch-support.md)
* [SE-0158 软件包管理器的 Manifest 配置文件 API 重新设计](https://github.com/apple/swift-evolution/blob/master/proposals/0158-package-manager-manifest-api-redesign.md)
* [SE-0162 软件包管理器的自定义目标布局](https://github.com/apple/swift-evolution/blob/master/proposals/0162-package-manager-custom-target-layouts.md)
* [SE-0175 软件包管理器修订的依赖解析](https://github.com/apple/swift-evolution/blob/master/proposals/0175-package-manager-revised-dependency-resolution.md)
* [SE-0179 Swift `run` 命令](https://github.com/apple/swift-evolution/blob/master/proposals/0179-swift-run-command.md)
* [SE-0181 软件包管理器 C/C++ 语言标准支持](https://github.com/apple/swift-evolution/blob/master/proposals/0181-package-manager-cpp-language-version.md)

### 文档

[《Swift 编程语言》](https://swift.org/documentation/#the-swift-programming-language)的 Swift 4.0 更新版本现可在 Swift.org 上获取。也可在 Apple 的 [iBooks 商店](https://itunes.apple.com/us/book/the-swift-programming-language/id881256329?mt=11)中免费下载

### 平台

#### Linux

Ubuntu 16.10、Ubuntu 16.04 和 Ubuntu 14.04 正式版的二进制文件已[可供下载](https://swift.org/download/)。

#### Apple（Xcode）

关于 Apple 平台上的开发，Swift 4.0 是作为 [Xcode 9.0](https://itunes.apple.com/app/xcode/id497799835) 的一部分进行分发的。

### 源

Swift 4.0 的开发是通过以下在 Github 上代码仓库的 swift-4.0-branch 分支来进行追踪的：

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
* [swift-integration-tests](https://github.com/apple/swift-integration-tests)

在那些仓库中的 `swift-4.0-RELEASE` 标签指定了组成 Swift 4.0 最终版本的具体修订。

`swift-4.0-branch` 分支将继续开放——但在同样的发布管理过程下——以累积更改以用于在将来潜在的，作为漏洞修复的“点”版本发布。

<br />
<sub>Original article: <a href="https://swift.org/blog/swift-4-0-released/">https://swift.org/blog/swift-4-0-released/</a></sub>

<sup>Original article copyright © 2017 Apple Inc. All rights reserved. Swift and the Swift logo are trademarks of Apple Inc.</sup>