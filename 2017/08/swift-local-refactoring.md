#Swift 本地重构

[阅读原文>>](https://swift.org/blog/swift-local-refactoring/)

2017 年 8 月 22 日&nbsp;&nbsp;&nbsp;&nbsp;[Xi Ge](https://twitter.com/xge_apple/)

Xcode 9 包括一个全新的重构引擎。它既可以在局部的单个 Swift 源文件，又可以在全局的范围内转换代码，例如重命名出现在多个文件甚至不同语言中的方法或属性。局部重构背后的逻辑完全在编译器和 SourceKit 中实现，且已经在 [swift 代码库](https://github.com/apple/swift)中开源。因此，每一个 Swift 爱好者都可以为语言贡献重构的操作。这篇文章讨论了如何在 Xcode 中实现并显现出一个简单的重构操作。

