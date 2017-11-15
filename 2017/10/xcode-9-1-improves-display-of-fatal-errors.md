# Xcode 9.1 改进了致命错误的显示

[阅读原文>>](https://swift.org/blog/xcode-9-1-improves-display-of-fatal-errors/)

2017 年 10 月 5 日&nbsp;&nbsp;&nbsp;&nbsp;[Kuba Mracek](https://twitter.com/kubamracek/)

Swift 有可以让你指定程序的预期的语言结构。如果这些预期在运行时不能满足，程序将会被终止。例如，*索引到数组中*隐式表达了索引处于边界内的预期：

> ```swift
> // 如果 “index” 小于 0 或大于 “array.count - 1”，程序将终止运行。
> let element = array[index]
> ```

另一个在失败时终止程序的常见操作是*可选类型的强制展开*：

> ```swift
> // 如果 “self.navigationController” 是 nil，程序将终止运行。
> let nc = self.navigationController!
> ```

*前提条件*亦是另一个例子：

> ```swift
> // 如果 “index” 小于或等于 0，程序将终止运行。
> precondition(index > 0, "Index must be greater than zero.")
> ```

当预期不正确，或代码中存在错误时，Swift *保证*了程序将会自陷。特别是在开发过程中，通常情况下在前提条件不满足时，程序将终止并且调试器会显示这个错误。但是，在 Xcode 9.1 之前（目前作为测试版提供），调试器会像显示其他类型的崩溃一样显示这些情况——通常为 `EXC_BAD_INSTRUCTION` 或 `EXC_BREAKPOINT`（即低级别的 Mach 异常类型）。

不管是对于初学者还是对于经验丰富的开发人员来说，这都会引起困惑。在 Xcode 9.1 中，致命错误的显示方式已经被显著改进了。现在，程序在调试器中运行并发生自陷时，Xcode 将在编辑器中显示*失败原因*：

<img alt="Xcode 9.1 中的致命错误就" src="https://swift.org/assets/images/fatal-errors/xcode-fatalerror.png" width="1160">

这项改进涵盖了许多触发运行时自陷的事件，其中包括：

- `nil` 的强制解包
- 强制尝试表达式（`try!`）产生的错误
- 在数组中超出边界的索引
- 前提条件失败
- 断言失败
- `fatalError` 调用

请注意，仅当应用程序的入口点是使用 Swift 编写（即您的应用程序委托使用 `@UIApplicationMain`/`@NSApplicationMain` 属性）的时候，此项改进体验才是可用的。

Xcode 9.1 可以从 [developer.apple.com](https://developer.apple.com/download/) 下载（目前是预发布版本，官方发布版本将在今年晚些时候提供）。

<br />
<sub>Original article: <a href="https://swift.org/blog/xcode-9-1-improves-display-of-fatal-errors/">https://swift.org/blog/xcode-9-1-improves-display-of-fatal-errors/</a></sub>

<sup>Original article copyright © 2017 Apple Inc. All rights reserved. Swift and the Swift logo are trademarks of Apple Inc.</sup>
