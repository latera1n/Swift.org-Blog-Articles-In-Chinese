# Swift 4 发布进程

[阅读原文>>](https://swift.org/blog/swift-4-0-release-process/)

2017 年 2 月 16 日&nbsp;&nbsp;&nbsp;&nbsp;[Ted Kremenek](https://github.com/tkremenek/)

这篇文章描述了 Swift 4 的目标、发布过程以及预估的时间表。

Swift 4 是一个主要的发布版本，预定于 2017 年秋天完成。它围绕着 Swift 3 代码展开，为其提供源代码稳定性，同时为语言的二进制稳定性进行了一些所需要的基础功能的工作。Swift 4 将包含对核心语言和标准库的显著增强，特别是对于泛型系统和字符串类型的改进。更多细节可以在[Swift 演进页面](https://github.com/apple/swift-evolution#development-major-version--swift-40)找到。

## 源码兼容性

Swift 4 的编译器将提供两种语言模式：`-swift-version 3` 和 `-swift-version 4`。

#### Swift 版本 3 模式

第一种模式 `-swift-version 3` 是现有代码的默认模式。在这种模式下，一个强有力的目标就是使用 Swift 4 编译器来继续构建那些使用 Swift 3.1 编译器构建的绝大多数源代码。其中的例外将是修复编译器的错误，这会导致编译器拒绝编译那些一开始就不应该被接受的源代码。而这些情况在实践中应该相对较少地出现。

如果遇到 Swift 4 编译器意外地拒绝以前使用 Swift 3.1 编译器编译的代码的情况，请提交[错误报告](https://bugs.swift.org/)。

#### Swift 版本 4 模式

预期的设计是，包含多个 Swift 模块的项目，例如具有多个 Swift 目标的 Xcode 项目，将能够对每个模块（目标）级别采用特定的 Swift 语言模式，并且它们可以与同一编译的二进制文件自由交互。请注意，当这些目标使用相同的编译器编译时，此互操作性仅存在于二进制层级。

以下例子解释了此互操作性实现了哪些内容：

* 在 Xcode 中，应用程序目标可以在 Swift 4（即 `-swift-version 4`）中编写，但是项目中可以存在一些被该应用程序使用的另外的，由 Swift 3 编写的框架目标（即 `-swift-version 3`）。

* 以 Swift 4 编写的 Swift 包（即 `-swift-version 4`）应该能够使用以 Swift 3 编写的现有包，而不需要将其源代码更新为 Swift 4 版本。
