# 使用预编译的桥接头文件以获得更快速的混合匹配构建

[阅读原文>>](https://swift.org/blog/bridging-pch/)

2017 年 1 月 26 日&nbsp;&nbsp;&nbsp;&nbsp;[Graydon Hoare](https://github.com/graydon/)

一个对于包含 Objective-C 和 Swift（其中可以包含大量桥接头）的 Xcode 项目的构建时间进行的检查显示，Swift 编译器花费了大量时间为项目中的所有 Swift 文件重复处理相同的桥接头文件。在某些项目中，每个额外的 Swift 文件显着增加了整个构建时间，即使这些 Swift 文件是相当简洁的。

## 问题

每次编译混合语言目标中的 Swift 文件时，Swift 编译器都将解析项目的桥接头文件，以使 Swift 代码可以看到 Objective-C 代码。当此桥接头文件很大并且 Swift 编译器运行很多次的时候——比如在调试配置时——重复解析桥接头文件的时间成本可能是整个构建所花时间的绝大部分。

## 解决方案

在 Swift 3.1 中，编译器已经获得了一种新模式来降低此在此花费的时间：预编译桥接头文件。当启用此模式时，编译器不会再重复解析混合语言目标中每个 Swift 文件中所包含的桥接头文件，而是仅解析这些桥接头文件一次，并且缓存结果（临时的“预编译头”或“PCH”文件）以在目标中所有的 Swift 文件中重用。这其中利用到的技术与用在 Objective-C 和 C++ 代码中预编译前缀头文件中的技术相同。

在开发和测试这种模式的 Swift 项目中，它__减少了 30％ 的调试构建时间__。该模式对项目的加速取决于项目中的桥接头文件的大小，并且其不会影响[模块整体优化的构建](/2016/10/whole-module-optimizations.md)。但它可以在调试配置中进行迭代时显着提高编译时间性能。

## 试试看

这种模式是 Swift 3.1 的一部分，可以在 swift.org 上的每日快照版本以及 Xcode 8.3 beta 版本中使用。这个模式目前是实验性的，且必须手动启用。在未来的版本中，如果开发者的反馈意见表明这个模式运行良好，并可以提供明显的加速，它将被默认启用。在此期间如果想要尝试此模式，需要安装一个支持它的编译器，然后打开您项目的构建设置，并设置“其他Swift标志”包含 `-enable-bridging-pch` 这个选项：

![Enabling bridging PCH](https://swift.org/assets/images/bridging-pch-blog/build-setting.png)

## 报告反馈

如果您有带有大桥接头文件的项目，请尝试使用此新模式。如果您遇到任何问题，请在 [Swift.org 的 bug 跟踪系统](https://bugs.swift.org/)或 [Apple 的系统](https://bugreport.apple.com/)中报告一个bug。同时也欢迎通过电子邮件 [swift-users@swift.org](https://lists.swift.org/mailman/listinfo/swift-users) 邮件列表或 [Twitter](https://twitter.com/swiftlang)（提及 #SwiftBridgingPCH）进行一般反馈。

<br />
<sub>Original article: [https://swift.org/blog/bridging-pch/](https://swift.org/blog/bridging-pch/)</sub>

<sup>Original article copyright © 2017 Apple Inc. All rights reserved. Swift and the Swift logo are trademarks of Apple Inc.</sup>
