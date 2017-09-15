# Swift 局部重构

[阅读原文>>](https://swift.org/blog/swift-local-refactoring/)

2017 年 8 月 22 日&nbsp;&nbsp;&nbsp;&nbsp;[Xi Ge](https://twitter.com/xge_apple/)

Xcode 9 包括一个全新的重构引擎。它既可以在局部的单个 Swift 源文件，又可以在全局的范围内转换代码，例如重命名出现在多个文件甚至不同语言中的方法或属性。局部重构背后的逻辑完全在编译器和 SourceKit 中实现，且已经在 [swift 代码库](https://github.com/apple/swift)中开源。因此，每一个 Swift 爱好者都可以为语言贡献重构的操作。这篇文章讨论了如何在 Xcode 中实现并显现出一个简单的重构操作。

## 重构的种类

**局部重构**发生在单个文件的范围内。局部重构的例子包括提取方法和提取重复表达式。跨多个文件改变代码（如全局重命名）的**全局重构**目前需要 Xcode 的特殊协调，目前无法在 Swift 代码库内自行实现。这篇文章专注于局部重构，因为它们本身可以是非常强大的。

在编辑器中，重构的动作是由用户光标选择启动的。根据它们是如何初始化的，我们将重构动作分类为基于光标或基于范围的操作。**基于光标的重构**具有一个由光标位置充分指定的 Swift 源文件中的重构目标，例如重命名重构。相比之下，**基于范围的重构**需要一个开始和结束位置来指定其目标，例如提取方法重构。为了方便实现这两个类别，Swift 代码库提供了预分析结果，称为 [ResolvedCursorInfo](https://github.com/apple/swift/blob/7f29b362d68eb990a592257850aabadb24de61df/include/swift/IDE/Utils.h#L158) 和 [ResolvedRangeInfo](https://github.com/apple/swift/blob/60a91bb7360dde5ce9531889e0ed10a2edbc961a/include/swift/IDE/Utils.h#L344)，以解析有关 Swift 源文件中光标位置或范围的几个常见问题。

例如，[ResolvedCursorInfo](https://github.com/apple/swift/blob/7f29b362d68eb990a592257850aabadb24de61df/include/swift/IDE/Utils.h#L158) 可以告诉我们源文件中的位置是否指向表达式的开头，如果是，则提供该表达式的相应编译器对象。或者，如果光标指向一个名称，[ResolvedCursorInfo](https://github.com/apple/swift/blob/7f29b362d68eb990a592257850aabadb24de61df/include/swift/IDE/Utils.h#L158) 将给出与该名称对应的声明。 类似地，[ResolvedRangeInfo](https://github.com/apple/swift/blob/60a91bb7360dde5ce9531889e0ed10a2edbc961a/include/swift/IDE/Utils.h#L344) 封装了有关给定源范围的信息，例如该范围是否具有多个入口点或出口点。

要为 Swift 实现一个新的重构，我们不需要从光标或范围位置的原始表示开始；相反地，我们可以从 [ResolvedCursorInfo](https://github.com/apple/swift/blob/7f29b362d68eb990a592257850aabadb24de61df/include/swift/IDE/Utils.h#L158) 和 [ResolvedRangeInfo](https://github.com/apple/swift/blob/60a91bb7360dde5ce9531889e0ed10a2edbc961a/include/swift/IDE/Utils.h#L344) 开始，使用它们即可导出重构特定的分析。

## 基于光标的重构
