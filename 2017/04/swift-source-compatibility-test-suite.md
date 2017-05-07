# Swift 源代码兼容性测试套件现已可用

[阅读原文>>](https://swift.org/blog/swift-source-compatibility-test-suite/)

2017 年 4 月 24 日

我们很高兴地宣布一个新的 [Swift 源代码兼容性测试套件](https://github.com/apple/swift-source-compat-suite)的发布，来作为未来 Swift 版本中维护源代码兼容性的一部分。

源代码兼容性测试套件是由社区驱动的，意味着开源项目所有者可以提交他们的项目以包含在套件中。有关将开源项目添加到测试套件的说明，请参见 Swift.org 的 [Swift 源代码兼容性](https://swift.org/source-compatibility)部分。

[Swift 的持续集成系统](https://ci.swift.org/)会定期在 Swift 的开发版本中定期构建套件中的项目，以尽快捕获源代码的兼容性回归。

Swift 编译器开发人员现在可以使用 Swift 的拉取请求测试系统以在更改与源兼容性测试套件之间进行测试，有助于在合并之前捕获源代码的兼容性回归。

这个项目的目标是得到一个强大的、包含数千个项目的源兼容性测试套件。我们期待项目拥有者通过将开源的 Swift 项目纳入测试套件来帮助实现这一目标。

<br />
<sub>Original article: <a href="https://swift.org/blog/swift-source-compatibility-test-suite/">https://swift.org/blog/swift-source-compatibility-test-suite/</a></sub>
