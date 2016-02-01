# Swift.org 博客

[阅读原文>>][original-article]

2015 年 12 月 3 日

欢迎来到 Swift.org 上的博客！我们于今日发布了 Swift 开源项目以及 Swift.org 网站。在这个开源社区里，我们可以一起工作，去发现并纠正问题，添加增强功能，并把 Swift 带入新的平台，这些都让我们无比激动。

专注于 Swift 的工程师们将在这个博客里宣布消息并突出展示社区里重要的话题。

## 项目

Swift 是由很多不同的项目所构成的，由此也提供了一个可以建立非凡项目的完整生态系统。Swift 编译器项目可以翻译 Swift 句法以产生诊断信息来帮助你去编写正确的代码，而且它还使用了 LLVM 去生成机器指令。LLDB 项目是一个第一类的调试器，它包含了一个用于交互式编程的 REPL（读取﹣求值﹣输出循环）。此外，Swift 标准库项目包含了你使用 Swift 编写软件时所需的所有核心类型和基础功能。

今天我们又发布了另外两个开源的 Swift 项目：Swift 核心库项目和一个全新的 Swift 软件包管理器项目。

## Swift 软件包管理器

[Swift 软件包管理器][swift-package-manager]是一个全新的项目，它力图创造一个强大且用户友好的工具来提供 Swift 代码的构建和分享功能。我们致力于保证软件包管理器在分享代码方面强大功能，而不是在分享已经编译好的二进制库方面。此项目正处于早期的开发阶段，并将会把 Swift 的开放合作过程用于它的设计开发。

你可以在 [GitHub 上的 Apple 主页][github-apple-homepage]中找到一些实例软件包仓库，除此之外，你还可以找到软件包管理器本身的代码和更多信息。

## 核心库

[Swift 核心库][swift-core-library]项目是一个凌驾于 Swift 标准库之上的高层级 API 的集合。这些库提供诸如本地化、网络原始函数、单元测试、用户偏好设置等等功能。这些库还引入了编码规范，这些规范将会对你在编写更多的 Swift 代码、创建新软件包时候大有裨益。

核心库基于包含在 Apple 平台之中的各类框架，即 Foundation、libdispatch 以及 XCTest。这些框架的 Swift 开源版本是为了让用户更简单地在多平台上使用相同 Swift 代码的一致功能。

## Swift.org 网站

此网站是 Swift 项目的主页，其中包含了你参与社区所需资源的链接。我们邀请你随意点击导航区域来探索一下这个网站，但是在这我们还是要在我们的首次博客发表中强调几个关键的链接：

 * [GitHub 上的 Apple 主页][github-apple-homepage]存放了 Swift 的全部源代码
 * [Swift 讨论组][swift-mailing-lists]将用于我们的互动
 * [入门][getting-started]页面将帮助你安装好一个 Swift 开发环境
 * [下载][download]页面包含一些为支持平台所预编译的库
 * [Swift Evolution Process][swift-evolution-process] 描述了新功能是怎样被提议的

欢迎来到 Swift 开源社区。

___—— Swift 团队___

<br />
<sub>Original article: [https://swift.org/blog/welcome/][original-article]</sub>

<sup>Original article copyright © 2016 Apple Inc. All rights reserved. Swift and the Swift logo are trademarks of Apple Inc.</sup>

[swift-package-manager]: https://swift.org/package-manager/
[github-apple-homepage]: http://github.com/apple
[swift-core-library]: https://swift.org/core-libraries/
[swift-mailing-lists]: https://swift.org/community/#mailing-lists
[getting-started]: https://swift.org/getting-started/
[download]: https://swift.org/download/
[swift-evolution-process]: https://swift.org/contributing/#evolution-process
[original-article]: https://swift.org/blog/welcome/
