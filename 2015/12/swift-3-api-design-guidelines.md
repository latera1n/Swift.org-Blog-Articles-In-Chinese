# Swift 3 API 设计指导方针

[阅读原文>>][original-article]

2015 年 12 月 3 日

编程语言的常用库的设计对一种语言的使用感受起着很大影响。好的库不仅用起来像是一种语言本身的延伸，而且库与库之间的一致性对整体的编程体验有着很大的提升。为了帮助构建更出色的 Swift 库，[Swift 3 的目标][goals-for-swift-3]之一就是去定义一套 API 设计的指导方针并把这些设计设计指导方针一致地运用其中。

定义 Swift API 设计指导方针的努力主要包括以下几方面，这几方面一起是为了给 Swift 的编程过程提供一个更加聚合的感觉。这几方面分别是：

* __Swift API 设计指导方针__：实际的 API 设计指导方针正在积极开发中。最新的 [Swift API 设计指导方针][swift-api-design-guidelines] 的草稿已可供查阅。

* __Swift 标准库__：整个 Swift 标准库正处在审核和更新的过程以确保其符合 Swift API 设计指导方针。实际的工作是在 Swift 代码仓库的 [swift-3-api-guidelines 分支][swift-3-api-guidelines-branch]上开展的。

* __导入的 Objective-C API__：用不同的启发式来更新 Objective-C API 到 Swift API 的翻译过程以使 Objective-C API 更好地匹配 Swift API 设计指导方针。 这篇[更好地把 Objective-C 翻译成 Swift][better-translation-of-objective-c-apis-into-swift] 的提议描述了这些变换是如何达成的。由于这些过程自然而然地包括了一些启发式，我们便在 Cocoa 和 Cocoa Touch 框架中，以及运用了这些框架的代码中追踪了它带来的影响。在 [Swift 3 API 设计指导方针审核][swift-3-api-design-guidelines-review]的代码仓库中提供了一种可以查看自动翻译对运用了 Cocoa 和 Cocoa Touch 的 Swift 代码起了哪些影响的途径。某些 Objective-C API 被粗暴地翻译进入 Swift 中，这些 API 将会相应地被注释（例如，用`NS_SWIFT_NAME` 来注释）来提升 Swift 成品代码的质量。尽管这些变更主要会对 Apple 平台造成一定影响（此平台上的 Swift 运用了 Objective-C runtime），在同时它还会对提供了与 Objective-C 相同 API 功能的跨平台 [Swift 核心库][swift-core-libraries]产生直接影响。

* __Swift 指导方针检查__：已有的 Swift 代码被编写成符合不同代码风格的形式，这其中包括 [Cocoa 上的 Objective-C 编码指导方针][objective-c-coding-guidelines-for-cocoa]。由于用于导入 Objective-C API 的启发式的作用，Swift 编译器能够（可选地！）检查不符合 Swift API 指导方针的那些常用 API 的设计模式。

* __Swift 2 到 Swift 3 的迁移器__：对 Swift 标准库和导入的 Objective-C API 的更新是一些可能破坏代码的变化。我们对此所做的努力包括创建一个迁移器来更新 Swift 2 代码以使其能够使用 Swift 3 的 API。

所有的这些主要方面都在积极的开发中。如果你有兴趣加入我们，请查看 [Swift API 设计指导方针][swift-api-design-guidelines]、[Swift 标准库的变化][swift-standard-library-changes]、关于 [Objective-C API 导入器的变化][objective-c-importer-changes]的提议以及与此相关的[审核代码仓库][review-repository]并加入 [swift-evolution 订阅][swift-evolution-mailing-list]的讨论。

<br />
<sub>Original article: [https://swift.org/blog/swift-3-api-design/][original-article]</sub>

<sup>Original article copyright © 2016 Apple Inc. All rights reserved. Swift and the Swift logo are trademarks of Apple Inc.</sup>

[original-article]: https://swift.org/blog/swift-3-api-design/
[goals-for-swift-3]: https://github.com/apple/swift-evolution/blob/master/README.md
[swift-api-design-guidelines]: https://swift.org/documentation/api-design-guidelines/
[swift-3-api-guidelines-branch]: https://github.com/apple/swift/tree/swift-3-api-guidelines
[better-translation-of-objective-c-apis-into-swift]: https://github.com/apple/swift-evolution/blob/master/proposals/0005-objective-c-name-translation.md
[swift-3-api-design-guidelines-review]: https://github.com/apple/swift-3-api-guidelines-review
[swift-core-libraries]: https://swift.org/core-libraries
[objective-c-coding-guidelines-for-cocoa]: https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html
[swift-standard-library-changes]: https://github.com/apple/swift/tree/swift-3-api-guidelines
[objective-c-importer-changes]: https://github.com/apple/swift-evolution/blob/master/proposals/0005-objective-c-name-translation.md
[review-repository]: https://github.com/apple/swift-3-api-guidelines-review
[swift-evolution-mailing-list]: https://swift.org/community/#swift-evolution