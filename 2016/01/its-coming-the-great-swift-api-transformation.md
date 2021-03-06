# 即将到来：绝佳的 Swift API 风格转变

[阅读原文>>][original-article]

2016 年 1 月 29 日&nbsp;&nbsp;&nbsp;&nbsp;[Dave Abrahams][dave-abrahams]

Cocoa、Swift 标准库和可能甚至是你自己的类型和方法，都将作出改变，而且你可以帮助决定如何改变。

自从 Swift 发布以来，Swift 标准库中的 Cocoa 接口和 API 之间的样式上的鸿沟就一直存在，其中很多东西经常十分不必要地看起来_差异明显_。这并不仅仅欠缺美学上的考虑，不一致性和缺少可预知性使得万事更难，不论是编程、除错或是维护。令人庆幸的是，尽管这些鸿沟存在，但 Swift 开发者仍然编写出了大量的优秀的代码，随之也逐步发展出了一种“Swifty”的代码应该看起来和体验起来是什么样子的感觉。

开发者的经历告诉我们，在审视 API 的时候，我们很容易就能看出其中有提升的空间。这些提升的空间不仅在于编译器引入 Objective-C API 的方式——导致了一些在 Swift 中看起来不是很舒服的结果，而且在于 Swift 标准库中缺失一定程度的，也是 Cocoa 用户使用的时候所预期的规律性和连贯性。对此身在 Apple 的我们决定做点什么。

为了保持 Cocoa 和 Swift 核心库的一致性，我们需要定点打击：实现一种 API 中大家都可以遵循的，统一的、书写上的方法。对此，我们以回溯并质疑我们最初的假设开始。现存的指导方针必定是极好的，但是其中很多内容却是为 Objective-C 而设定的，它们并没有覆盖具体的 Swift 功能，例如缺省参数。更重要的是，它们制定的时候并没有受到新兴的“Swift 之感”的影响，而这正是我们觉得应该达到的最重要的一点。

我们在开发这些指导方针的同时便把它们运用到了标准库、整个 Cocoa 和几个实例项目之中。在此期间我们重复了评估结果并不断改进的过程。在 Swift 开源之前，我们便已经非公开地完成了这些改进，并即将在下个发行版本中带为大家带来我们改进的结果。但是我们觉得一个 Swift 的新的时代已经来临：是时候向全世界展示一下我们最近做了些什么了。下面就是一个代码式样的例子，在作出改变之前这段代码是这样的：

```swift
class UIBezierPath : NSObject, NSCopying, NSCoding { ... }
...
path.addLineToPoint(CGPoint(x: 100, y: 0))
path.fillWithBlendMode(kCGBlendModeMultiply, alpha: 0.7)
```

和改变之后：

```swift
class UIBezierPath : Object, Copying, Coding { ... }
...
path.addLineTo(CGPoint(x: 100, y: 0))
path.fillWith(kCGBlendModeMultiply, alpha: 0.7)
```

我们已经把提议的风格转变的三个部分发布在 [Swift 评审组][swift’s-evolution-group]上以供公共审核：[Cocoa 引入方式的改变][swift’s-evolution=group:-changes-to-how-cocoa-is-imported]、[核心库外表上的改变][changes-to-the-surface-of-the-standard-library]以及把所有紧密联系在一起的 [API 指导方针][api-guidelines]。参与者们也已经开始向我们提供改进的建议，由此我们能够从中看出它们是[如何影响 API][affect－apis] 的。

举个~~🌰~~例子，我们[发现][explored]的[有一个建议][one-suggestion]是把这个调用：

```swift
path.addArcWithCenter(
  origin, radius: 20.0, 
  startAngle: 0.0, endAngle: CGFloat(M_PI) * 2.0, clockwise: true)
```

改变成这样：

```swift
path.addArc(
  center: origin, radius: 20.0, 
  startAngle: 0.0, endAngle: CGFloat(M_PI) * 2.0, clockwise: true)
```

我们是否会对此作出改变？虽然评审团已经确定了，但也是时候让大家听到你呼声了。审核周期已经延长至 __2 月 5 日 星期五__。如果你有兴趣帮助塑造你的语言和框架的未来，请[加入我们的讨论][join-the-discussion]。提议和相关的审核帖子在这里：

* [API 设计指导方针][api-design－guidelines]——[讨论][discussion-0]
* [更好的将 Objective-C API 翻译进入 Swift][better-translation-of-objective-c-apis-into-swift]——[讨论][discussion-1]
* [应用 API 指导方针到标准库][apply-api-Guidelines-to-the-standard-library]——[讨论][discussion-2]

<br />
<sub>Original article: [https://swift.org/blog/swift-api-transformation/][original-article]</sub>

<sup>Original article copyright © 2016 Apple Inc. All rights reserved. Swift and the Swift logo are trademarks of Apple Inc.</sup>

[original-article]: https://swift.org/blog/swift-api-transformation/
[dave-abrahams]: https://github.com/dabrahams/
[swift’s-evolution-group]: https://swift.org/community/#mailing-lists
[swift’s-evolution=group:-changes-to-how-cocoa-is-imported]: https://swift.org/community/#mailing-lists
[changes-to-the-surface-of-the-standard-library]: https://github.com/apple/swift-evolution/blob/master/proposals/0006-apply-api-guidelines-to-the-standard-library.md
[api-guidelines]: https://github.com/apple/swift-evolution/blob/master/proposals/0023-api-guidelines.md
[affect－apis]: https://github.com/apple/swift-3-api-guidelines-review/pull/5/files
[explored]: http://news.gmane.org/find-root.php?message_id=18A8335F%2d65F3%2d46A1%2dA494%2dAA89AC10836B%40apple.com
[one-suggestion]: http://news.gmane.org/find-root.php?message_id=3C5040B3%2dA205%2d46FA%2d98D3%2d5696D678EB39%40gmail.com
[join-the-discussion]: https://swift.org/contributing/#participating-in-the-swift-evolution-process
[api-design－guidelines]: https://github.com/apple/swift-evolution/blob/master/proposals/0023-api-guidelines.md
[discussion-0]: http://news.gmane.org/find-root.php?message_id=ABB71FFD%2d1AE8%2d43D3%2dB3F5%2d58225A2BAD66%40apple.com
[better-translation-of-objective-c-apis-into-swift]: https://github.com/apple/swift-evolution/blob/master/proposals/0005-objective-c-name-translation.md
[discussion-1]: http://news.gmane.org/find-root.php?message_id=CC036592%2d085D%2d4095%2d8D73%2d1DA9FC90A07B%40apple.com
[apply-api-Guidelines-to-the-standard-library]: https://github.com/apple/swift-evolution/blob/master/proposals/0006-apply-api-guidelines-to-the-standard-library.md
[discussion-2]: http://news.gmane.org/find-root.php?message_id=73E699B0%2dFAD2%2d46DA%2dB74E%2d849445A2F38A%40apple.com
