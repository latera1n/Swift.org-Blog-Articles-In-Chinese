# Swift 3.0 现已发布！

[阅读原文>>](https://swift.org/blog/swift-3-0-released/)

2016 年 9 月 13 日&nbsp;&nbsp;&nbsp;&nbsp;[Ted Kremenek](https://github.com/tkremenek/)

Swift 3.0，Swift 开源以来的第一个主要版本，现在已经正式发布！Swift 3 是一个重大的发布版本，其中包含对核心语言和标准库的重要改动和精炼，对 Swift 在 Linux 上移植版的主要补充，以及 [Swift 包管理器](https://swift.org/package-manager)的第一个正式发布版。

### 语言改变

Swift 3.0 是一个主要的语言版本。它不能与 Swift 2.2 和 2.3 在源代码方面兼容。它包含以下经历了 Swift [演进过程](https://swift.org/contributing/#participating-in-the-swift-evolution-process)的语言改变：

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
* [SE-0025：域内的访问级别](https://github.com/apple/swift-evolution/blob/master/proposals/0025-scoped-access-level.md)
* [SE-0029：从函数的应用中移除隐式的元组泼贱行为](https://github.com/apple/swift-evolution/blob/master/proposals/0029-remove-implicit-tuple-splat.md)
* [SE-0031：为类型的修饰调整 inout 声明](https://github.com/apple/swift-evolution/blob/master/proposals/0031-adjusting-inout-declarations.md)
* [SE-0032：为 `SequenceType` 类型新增 `first(where:)` 方法](https://github.com/apple/swift-evolution/blob/master/proposals/0032-sequencetype-find.md)
* [SE-0033：导入 Objective-C 常量为 Swift 类型](https://github.com/apple/swift-evolution/blob/master/proposals/0033-import-objc-constants.md)
* [SE-0034：消除调试标识符中行控制语句的歧义](https://github.com/apple/swift-evolution/blob/master/proposals/0034-disambiguating-line.md)
* [SE-0035：限制 `inout` 捕获至 `@noescape` 中](https://github.com/apple/swift-evolution/blob/master/proposals/0035-limit-inout-capture.md)
* [SE-0036：要求枚举类型实例成员使用前导点号](https://github.com/apple/swift-evolution/blob/master/proposals/0036-enum-dot.md)
* [SE-0037：澄清注释和运算符之间的相互作用](https://github.com/apple/swift-evolution/blob/master/proposals/0037-clarify-comments-and-operators.md)
* [SE-0038：软件包管理器的 C 语言目标支持](https://github.com/apple/swift-evolution/blob/master/proposals/0038-swiftpm-c-language-targets.md)
* [SE-0039：把 Playground 字面量变的现代化](https://github.com/apple/swift-evolution/blob/master/proposals/0039-playgroundliterals.md)
* [SE-0040：替换属性参数的等号为冒号](https://github.com/apple/swift-evolution/blob/master/proposals/0040-attributecolons.md)
* [SE-0043：在带有多个模式的 “case” 标签中声明变量](https://github.com/apple/swift-evolution/blob/master/proposals/0043-declare-variables-in-case-labels-with-multiple-patterns.md)
* [SE-0044：导入为成员](https://github.com/apple/swift-evolution/blob/master/proposals/0044-import-as-member.md)
* [SE-0046：在包括第一个标签的所有参数上建立一致的标签行为](https://github.com/apple/swift-evolution/blob/master/proposals/0046-first-label.md)
* [SE-0047：使有返回值的函数默认显示未使用结果的警告](https://github.com/apple/swift-evolution/blob/master/proposals/0047-nonvoid-warn.md)
* [SE-0048：范型类型别名](https://github.com/apple/swift-evolution/blob/master/proposals/0048-generic-typealias.md)
* [SE-0049：把 @noescape 和 @autoclosure 转变为类型属性](https://github.com/apple/swift-evolution/blob/master/proposals/0049-noescape-autoclosure-type-attrs.md)
* [SE-0052：更改 IteratorType 空值的后续保证](https://github.com/apple/swift-evolution/blob/master/proposals/0052-iterator-post-nil-guarantee.md)
* [SE-0053：移除函数参数中显式使用的 `let`](https://github.com/apple/swift-evolution/blob/master/proposals/0053-remove-let-from-function-parameters.md)
* [SE-0054：废除 `ImplicitlyUnwrappedOptional` 类型](https://github.com/apple/swift-evolution/blob/master/proposals/0054-abolish-iuo.md)
* [SE-0055：用可选类型以明显不安全指针的为空性](https://github.com/apple/swift-evolution/blob/master/proposals/0055-optional-unsafe-pointers.md)
* [SE-0057：导入 Objective-C 的轻量级范型](https://github.com/apple/swift-evolution/blob/master/proposals/0057-importing-objc-generics.md)
* [SE-0059：更新 API 命名指导方针并相应地重写设值 API](https://github.com/apple/swift-evolution/blob/master/proposals/0059-updated-set-apis.md)
* [SE-0060：默认参数的强制顺序](https://github.com/apple/swift-evolution/blob/master/proposals/0060-defaulted-parameter-order.md)
* [SE-0061：新增范型结果和错误处理至 autoreleasepool()](https://github.com/apple/swift-evolution/blob/master/proposals/0061-autoreleasepool-signature.md)
* [SE-0062：引用 Objective-C 中的键路径](https://github.com/apple/swift-evolution/blob/master/proposals/0062-objc-keypaths.md)
* [SE-0063：SwiftPM 系统模块搜索路径](https://github.com/apple/swift-evolution/blob/master/proposals/0063-swiftpm-system-module-search-paths.md)
* [SE-0064：引用 Objective-C 中取值和设值方法的选择器](https://github.com/apple/swift-evolution/blob/master/proposals/0064-property-selectors.md)
* [SE-0065：一个集合类型与下标的新模型](https://github.com/apple/swift-evolution/blob/master/proposals/0065-collections-move-indices.md)
* [SE-0066：标准化函数类型参数语法以需要使用括号](https://github.com/apple/swift-evolution/blob/master/proposals/0066-standardize-function-type-syntax.md)
* [SE-0067：增强的浮点协议](https://github.com/apple/swift-evolution/blob/master/proposals/0067-floating-point-protocols.md)
* [SE-0069：可变性和基础值类型](https://github.com/apple/swift-evolution/blob/master/proposals/0069-swift-mutability-for-foundation.md)
* [SE-0070：使可选类型的要求仅限于 Objective-C](https://github.com/apple/swift-evolution/blob/master/proposals/0070-optional-requirements.md)
* [SE-0071：允许（大部分）关键字被用于成员引用](https://github.com/apple/swift-evolution/blob/master/proposals/0071-member-keywords.md)
* [SE-0072：完全消除来自 Swift 的隐式桥接转换](https://github.com/apple/swift-evolution/blob/master/proposals/0072-eliminate-implicit-bridging-conversions.md)
* [SE-0076：为 UnsafeMutablePointer 的非破坏性复制方法新增接受一个源为 UnsafePointer 的重写](https://github.com/apple/swift-evolution/blob/master/proposals/0076-copying-to-unsafe-mutable-pointer-with-unsafe-pointer-source.md)
* [SE-0077：改善的运算符声明](https://github.com/apple/swift-evolution/blob/master/proposals/0077-operator-precedence.md)
* [SE-0081：移动 `where` 子句至声明的末尾](https://github.com/apple/swift-evolution/blob/master/proposals/0081-move-where-expression.md)
* [SE-0085：软件包管理器的命令名称](https://github.com/apple/swift-evolution/blob/master/proposals/0085-package-manager-command-name.md)
* [SE-0086：在 Swift Foundation 中舍弃 NS 前缀](https://github.com/apple/swift-evolution/blob/master/proposals/0086-drop-foundation-ns.md)
* [SE-0088：为 Swift 3 命名规范现代化 libdispatch](https://github.com/apple/swift-evolution/blob/master/proposals/0088-libdispatch-for-swift3.md)
* [SE-0089：重命名 `String.init<T>(_: T)`](https://github.com/apple/swift-evolution/blob/master/proposals/0089-rename-string-reflection-init.md)
* [SE-0091：改善协议中的运算符要求](https://github.com/apple/swift-evolution/blob/master/proposals/0091-improving-operators-in-protocols.md)
* [SE-0092：协议及协议扩展中的 Typealias](https://github.com/apple/swift-evolution/blob/master/proposals/0092-typealiases-in-protocols.md)
* [SE-0093：新增一个公共的 `base` 属性至切片类型中](https://github.com/apple/swift-evolution/blob/master/proposals/0093-slice-base.md)
* [SE-0094：新增 sequence(first:next:) 和 sequence(state:next:) 至 stdlib](https://github.com/apple/swift-evolution/blob/master/proposals/0094-sequence-function.md)
* [SE-0095：替换 `protocol<P1,P2>` 语法为 `P1 & P2` 语法](https://github.com/apple/swift-evolution/blob/master/proposals/0095-any-as-existential.md)
* [SE-0096：转换 dynamicType 由属性变为运算符](https://github.com/apple/swift-evolution/blob/master/proposals/0096-dynamictype.md)
* [SE-0099：重组条件分句](https://github.com/apple/swift-evolution/blob/master/proposals/0099-conditionclauses.md)
* [SE-0101：重新配置 `sizeof` 及相关函数进入一个统一的 `MemoryLayout` 结构体](https://github.com/apple/swift-evolution/blob/master/proposals/0101-standardizing-sizeof-naming.md)
* [SE-0102：移除 `@noreturn` 属性并引入一个空 `Never` 类型](https://github.com/apple/swift-evolution/blob/master/proposals/0102-noreturn-bottom-type.md)
* [SE-0103：使非逸出闭包成为默认](https://github.com/apple/swift-evolution/blob/master/proposals/0103-make-noescape-default.md)
* [SE-0106：为 `OSX` 平台配置测试新增一个 `macOS` 别名](https://github.com/apple/swift-evolution/blob/master/proposals/0106-rename-osx-to-macos.md)
* [SE-0107：UnsafeRawPointer API](https://github.com/apple/swift-evolution/blob/master/proposals/0107-unsaferawpointer.md)
* [SE-0109：移除 `Boolean` 协议](https://github.com/apple/swift-evolution/blob/master/proposals/0109-remove-boolean.md)
* [SE-0111：移除函数参数标签的类型系统的重要性](https://github.com/apple/swift-evolution/blob/master/proposals/0111-remove-arg-label-type-significance.md)
* [SE-0112：改进的 NSError 桥接](https://github.com/apple/swift-evolution/blob/master/proposals/0112-nserror-bridging.md)
* [SE-0113：新增 FloatingPoint 类型的整数数值修约](https://github.com/apple/swift-evolution/blob/master/proposals/0113-rounding-functions-on-floatingpoint.md)
* [SE-0114：更新 Buffer 类型的“Value”名称至“Header”名称](https://github.com/apple/swift-evolution/blob/master/proposals/0114-buffer-naming.md)
* [SE-0115：重命名字面量语法协议](https://github.com/apple/swift-evolution/blob/master/proposals/0115-literal-syntax-protocols.md)
* [SE-0116：导入 Objective-C `id` 为 Swift `Any` 类型](https://github.com/apple/swift-evolution/blob/master/proposals/0116-id-as-any.md)
* [SE-0117：允许区分公共访问和公共可覆盖性](https://github.com/apple/swift-evolution/blob/master/proposals/0117-non-public-subclassable-by-default.md)
* [SE-0118：闭包参数名称和标签](https://github.com/apple/swift-evolution/blob/master/proposals/0118-closure-parameter-names-and-labels.md)
* [SE-0120：修订 `partition` 方法签名](https://github.com/apple/swift-evolution/blob/master/proposals/0120-revise-partition-method.md)
* [SE-0121：移除 `Optional` 类型的比较运算符](https://github.com/apple/swift-evolution/blob/master/proposals/0121-remove-optional-comparison-operators.md)
* [SE-0124：`Int.init(ObjectIdentifier)` 和 `UInt.init(ObjectIdentifier)` 应该拥有一个 `bitPattern:` 标签](https://github.com/apple/swift-evolution/blob/master/proposals/0124-bitpattern-label-for-int-initializer-objectidentfier.md)
* [SE-0125：移除 `NonObjectiveCBase` 和 `isUniquelyReferenced`](https://github.com/apple/swift-evolution/blob/master/proposals/0125-remove-nonobjectivecbase.md)
* [SE-0127：清理 stdlib 指针和缓冲区程序](https://github.com/apple/swift-evolution/blob/master/proposals/0127-cleaning-up-stdlib-ptr-buffer.md)
* [SE-0128：更改不可失败的 UnicodeScalar 初始化器为可失败](https://github.com/apple/swift-evolution/blob/master/proposals/0128-unicodescalar-failable-initializer.md)
* [SE-0129：软件包管理器测试命名规范](https://github.com/apple/swift-evolution/blob/master/proposals/0129-package-manager-test-naming-conventions.md)
* [SE-0130：替换重复的 `Character` 和 `UnicodeScalar` 形式的  String.init](https://github.com/apple/swift-evolution/blob/master/proposals/0130-string-initializers-cleanup.md)
* [SE-0131：添加 `AnyHashable` 至标准库](https://github.com/apple/swift-evolution/blob/master/proposals/0131-anyhashable.md)
* [SE-0133：重命名 `flatten()` 至 `joined()`](https://github.com/apple/swift-evolution/blob/master/proposals/0133-rename-flatten-to-joined.md)
* [SE-0134：重命名 String 类型中两处 UTF8 相关的属性](https://github.com/apple/swift-evolution/blob/master/proposals/0134-rename-string-properties.md)
* [SE-0135：软件包管理器中对通过 Swift 版本来区分软件包的支持](https://github.com/apple/swift-evolution/blob/master/proposals/0135-package-manager-support-for-differentiating-packages-by-swift-version.md)
* [SE-0136：数值的内存布局](https://github.com/apple/swift-evolution/blob/master/proposals/0136-memory-layout-of-values.md)
* [SE-0137：避免对于遗留协议设计的锁定](https://github.com/apple/swift-evolution/blob/master/proposals/0137-avoiding-lock-in.md)

### 迁移至 Swift 3

Swift 3 是一个具有代码破坏性的发布版，其主要原因是 [SE-0005](https://github.com/apple/swift-evolution/blob/master/proposals/0005-objective-c-name-translation.md) 和 [SE-0006](https://github.com/apple/swift-evolution/blob/master/proposals/0006-apply-api-guidelines-to-the-standard-library.md) 中的更改。这些更改不仅影响到标准库的 API 名称，而且还包括有关 Objective-C API（特别是来自 Cocoa 的那些）如何导入到 Swift 中的彻底更改。许多更改在很大程度上是机械的，但是它们可能大量地存在于一个典型的 Swift 项目中。

为了帮助你移动到 Swift 3，Xcode 8.0 包含了一个代码迁移器，可以自动处理许多需要改变源代码的更改。我们还提供了一个迁移指南，可以指导你完成许多更改——特别是帮你完成那些不是很机械的且需要更多直接审查的更改。

### 文档

[《Swift 编程语言》](https://swift.org/documentation/#the-swift-programming-language)的 Swift 3.0 更新版本现可在 Swift.org 上获取。也可在 Apple 的 iBooks 商店中免费下载。

### 平台

#### Linux（Ubuntu 14.04 和 Ubuntu 15.10）

Linux 移植版现在已经包含 [Swift 核心库](https://swift.org/core-libraries/)和 [Swift 软件包管理器](https://swift.org/package-manager)。

Ubuntu 14.04 和 Ubuntu 15.10 正式版的二进制文件已[可供下载](https://swift.org/download/)。

#### Apple（Xcode）

关于 Apple 平台上的开发，Swift 3.0 是作为 [Xcode 8.0](https://itunes.apple.com/app/xcode/id497799835) 的一部分进行分发的。

### 源

Swift 3.0 的开发是通过以下在 Github 上代码仓库的 `swift-3.0-branch` 分支来进行追踪的：

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

在那些仓库中的 `swift-3.0-RELEASE` 标签指定了组成 Swift 3.0 最终版本的具体修订。

`swift-3.0-branch` 分支将继续开放——但在同样的[发布管理过程](https://swift.org/blog/swift-3-0-release-process/)下——以累积更改以用于在将来潜在的，作为漏洞修复的“点”版本发布。

<br />
<sub>Original article: [https://swift.org/blog/swift-3-0-released/](https://swift.org/blog/swift-3-0-released/)</sub>

<sup>Original article copyright © 2016 Apple Inc. All rights reserved. Swift and the Swift logo are trademarks of Apple Inc.</sup>
