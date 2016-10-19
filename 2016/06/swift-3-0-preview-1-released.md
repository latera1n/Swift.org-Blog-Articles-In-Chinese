# Swift 3.0 预览版本 1 现已发布！

[阅读原文>>](https://swift.org/blog/swift-3-0-preview-1-released/)

2016 年 6 月 13 日

我们非常高兴地宣布 Swift 3.0 开发者预览版本 1！

正如 [Swift 3.0 发布进程](../05/swift-3-0-release-process.md)中所描述的那样，开发者预览（即“种子”或“beta版”）提供了合格的 Swift 3 构建版本，它比仅抓取了 `master` 分支的最新快照（即分支末端的开发）要更稳定。开发者预览应被视为 Swift 3 的仍在进行中的工作，除非另有说明，否则都不应被视为 Swift 3 的最终版本。

### 已经实现的 Swift 演进提案

以下列出的 Swift 演进提案是在 Swift 3.0 预览版本 1 中所新实现的：

* [SE-0002：移除柯里化的 `func` 声明语法](https://github.com/apple/swift-evolution/blob/master/proposals/0002-remove-currying.md)
* [SE-0003：从函数参数中移除 `var`](https://github.com/apple/swift-evolution/blob/master/proposals/0003-remove-var-parameters.md)
* [SE-0004：移除 `++` 和 `--` 运算符](https://github.com/apple/swift-evolution/blob/master/proposals/0004-remove-pre-post-inc-decrement.md)
* [SE-0005：更好地把 Objective-C API 翻译进 Swift 中](https://github.com/apple/swift-evolution/blob/master/proposals/0005-objective-c-name-translation.md)
* [SE-0006：把 API 指导方针应用于标准库](https://github.com/apple/swift-evolution/blob/master/proposals/0006-apply-api-guidelines-to-the-standard-library.md)
* [SE-0007：移除带有条件和增量方式的 C 语言风格 for 循环](https://github.com/apple/swift-evolution/blob/master/proposals/0007-remove-c-style-for-loops.md)
* [SE-0008：为可选类型序列新增一种惰性的 flatMap](https://github.com/apple/swift-evolution/blob/master/proposals/0008-lazy-flatmap-for-optionals.md)
* [SE-0016：为 Int 和 UInt 类型新增初始化器以便从 UnsafePointer 和 UnsafeMutablePointer 类型转换](https://github.com/apple/swift-evolution/blob/master/proposals/0016-initializers-for-converting-unsafe-pointers-to-ints.md)
* [SE-0017：更改 `Unmanaged` 类型以使用 `UnsafePointer` 类型](https://github.com/apple/swift-evolution/blob/master/proposals/0017-convert-unmanaged-to-use-unsafepointer.md)
* [SE-0019：Swift 测试](https://github.com/apple/swift-evolution/blob/master/proposals/0019-package-manager-testing.md)
* [SE-0023：API 设计指导方针](https://github.com/apple/swift-evolution/blob/master/proposals/0023-api-guidelines.md)
* [SE-0028：现代化 Swift 调试标识符（\_\_FILE\_\_ 等）](https://github.com/apple/swift-evolution/blob/master/proposals/0028-modernizing-debug-identifiers.md)
* [SE-0029：从函数的应用中移除隐式的元组泼贱行为](https://github.com/apple/swift-evolution/blob/master/proposals/0029-remove-implicit-tuple-splat.md)
* [SE-0031：为类型的修饰调整 inout 声明](https://github.com/apple/swift-evolution/blob/master/proposals/0031-adjusting-inout-declarations.md)
* [SE-0032：为 `SequenceType` 类型新增 `first(where:)` 方法](https://github.com/apple/swift-evolution/blob/master/proposals/0032-sequencetype-find.md)
* [SE-0033：导入 Objective-C 常量为 Swift 类型](https://github.com/apple/swift-evolution/blob/master/proposals/0033-import-objc-constants.md)
* [SE-0034：消除调试标识符中行控制语句的歧义](https://github.com/apple/swift-evolution/blob/master/proposals/0034-disambiguating-line.md)
* [SE-0037：澄清注释和运算符之间的相互作用](https://github.com/apple/swift-evolution/blob/master/proposals/0037-clarify-comments-and-operators.md)
* [SE-0039：把 Playground 字面量变的现代化](https://github.com/apple/swift-evolution/blob/master/proposals/0039-playgroundliterals.md)
* [SE-0040：替换属性参数的等号为冒号](https://github.com/apple/swift-evolution/blob/master/proposals/0040-attributecolons.md)
* [SE-0043：在带有多个模式的 “case” 标签中声明变量](https://github.com/apple/swift-evolution/blob/master/proposals/0043-declare-variables-in-case-labels-with-multiple-patterns.md)
* [SE-0044：导入为成员](https://github.com/apple/swift-evolution/blob/master/proposals/0044-import-as-member.md)
* [SE-0046：在包括第一个标签的所有参数上建立一致的标签行为](https://github.com/apple/swift-evolution/blob/master/proposals/0046-first-label.md)
* [SE-0047：使有返回值的函数默认显示未使用结果的警告](https://github.com/apple/swift-evolution/blob/master/proposals/0047-nonvoid-warn.md)
* [SE-0048：范型类型别名](https://github.com/apple/swift-evolution/blob/master/proposals/0048-generic-typealias.md)
* [SE-0049：把 @noescape 和 @autoclosure 转变为类型属性](https://github.com/apple/swift-evolution/blob/master/proposals/0049-noescape-autoclosure-type-attrs.md)
* [SE-0053：移除函数参数中显式使用的 `let`](https://github.com/apple/swift-evolution/blob/master/proposals/0053-remove-let-from-function-parameters.md)
* [SE-0054：废除 `ImplicitlyUnwrappedOptional` 类型](https://github.com/apple/swift-evolution/blob/master/proposals/0054-abolish-iuo.md)
* [SE-0055：用可选类型以明显不安全指针的为空性](https://github.com/apple/swift-evolution/blob/master/proposals/0055-optional-unsafe-pointers.md)
* [SE-0057：导入 Objective-C 的 轻量级范型](https://github.com/apple/swift-evolution/blob/master/proposals/0057-importing-objc-generics.md)
* [SE-0059：更新 API 命名指导方针并相应地重写设值 API](https://github.com/apple/swift-evolution/blob/master/proposals/0059-updated-set-apis.md)
* [SE-0061：新增范型结果和错误处理至 autoreleasepool()](https://github.com/apple/swift-evolution/blob/master/proposals/0061-autoreleasepool-signature.md)
* [SE-0062：引用 Objective-C 中的键路径](https://github.com/apple/swift-evolution/blob/master/proposals/0062-objc-keypaths.md)
* [SE-0064：引用 Objective-C 中取值和设值方法的选择器](https://github.com/apple/swift-evolution/blob/master/proposals/0064-property-selectors.md)
* [SE-0065：一个集合类型与下标的新模型](https://github.com/apple/swift-evolution/blob/master/proposals/0065-collections-move-indices.md)
* [SE-0066：标准化函数类型参数语法以需要使用括号](https://github.com/apple/swift-evolution/blob/master/proposals/0066-standardize-function-type-syntax.md)
* [SE-0069：可变性和基础值类型](https://github.com/apple/swift-evolution/blob/master/proposals/0069-swift-mutability-for-foundation.md)
* [SE-0070：使可选类型的要求仅限于 Objective-C](https://github.com/apple/swift-evolution/blob/master/proposals/0070-optional-requirements.md)
* [SE-0071：允许（大部分）关键字被用于成员引用](https://github.com/apple/swift-evolution/blob/master/proposals/0071-member-keywords.md)
* [SE-0085：软件包管理器的命令名称](https://github.com/apple/swift-evolution/blob/master/proposals/0085-package-manager-command-name.md)
* [SE-0094：添加 sequence(first:next:) 和 sequence(state:next:) 至 stdlib](https://github.com/apple/swift-evolution/blob/master/proposals/0094-sequence-function.md)

### 下载

#### Apple（Xcode）

Swift 3.0 预览版本 1 是作为 [Xcode 8.0 beta 1](https://developer.apple.com/xcode/download) 的一部分进行免费分发的。

#### Linux（Ubuntu 14.04 和 Ubuntu 15.10）

为 Ubuntu 14.04 和 Ubuntu 15.10 提供的官方二进制文件已经可以在 Swift.org 上[下载](https://swift.org/download/)。

### 文档

[《Swift 编程语言》](https://swift.org/documentation/#the-swift-programming-language)的 Swift 3.0 更新版本现可在 Swift.org 上获取。也可在 Apple 的 [iBooks 商店](https://itunes.apple.com/us/book/the-swift-programming-language/id1002622538?mt=11)中免费下载。

### Foundation 与 Linux（核心库）

并非所有有关 `NS` 为前缀的移除更改都已传入至对于 Foundation API 的核心库版本实现中。这个问题应该会在未来的测试版中解决。

### 迁移至 Swift 3

Swift 3 是在 Swift 2.2.1 基础上的一个具有代码破坏性的发布版。它包含许多语法增强和改进，但是由于 [SE-0005](https://github.com/apple/swift-evolution/blob/master/proposals/0005-objective-c-name-translation.md)，它也包含着有关 Objective-C API 如何导入到 Swift 中去的大量改变。有关迁移到 Swift 3 的指导和提示，请参阅[迁移指南](https://swift.org/migration-guide/)。

<br />
<sub>Original article: [https://swift.org/blog/swift-3-0-preview-1-released/](https://swift.org/blog/swift-3-0-preview-1-released/)</sub>

<sup>Original article copyright © 2016 Apple Inc. All rights reserved. Swift and the Swift logo are trademarks of Apple Inc.</sup>
