# Swift 的 Linux 端口

[阅读原文>>][original-article]

2015 年 12 月 3 日

随着 Swift 开源项目的发布，我们同时也分发了一个可以在 Linux 操作系统上运行的端口！你可以使用 Swift 源代码自行编译，也可以[下载用于 Ubuntu 的预编译库][pre-built-libraries-for-ubuntu]。虽然这个端口项目的正处于开发的阶段，但是我们还是很高兴地告诉大家，现在它可用于一些实验目的的代码编写工作。目前在 Linux 上它仅支持 x86_64 体系结构。

下面是目前为止在此端口中可正常工作部分的要点：

* __不依赖 Objective-C Runtime 的 Swift__：Linux 上的 Swift 不仅不依赖于 Objective-C runtime，而且还不包含 Objective-C。当 Swift 被设计为可以与 Objective-C 紧密互联互通的同时，它也被设计为可以在任何没有 Objective-C runtime 存在的地方运行。

* Linux 之上的 __Swift 语言核心和[标准库][standard-library]__与在 Apple 平台上的 Swift 共享着绝大部分相同的功能实现和 API。仅有的一些轻微的特性差异是由于 Objective-C runtime 的缺失而导致的（下详）。

* __Glibc 模块__：正如 Apple 平台上的 Darwin 模块那样，绝大多数的 Linux C 标准库都可以通过此模块获得。但是有一些头文件没有被导入其中，例如 tgmath.h。

    若想试用 Glibc 模块，只需`import Glibc`。

* __Swift 核心库__：[核心库][core-libraries]提供了能在没有 Objective-C 的 Linux 上运行的却来自于 Foundation 和 XCTest 的核心 API 功能实现。这之所以能成为现实是基于以下出发点：这些 API 要跨平台可用，不管你正在使用的是 Apple 平台上的 Swift 还是 Linux 平台上的 Swift。

* __LLDB Swift 调试和 REPL__：正如你能在 OS X 上做的一样，你可以[调试你的 Swift 二进制文件][debug-swift-binaries]，并且可以[在 REPL 中进行实验][experiment-in-the-repl]。

* __Swift 软件包管理器__在 Linux 平台上与在 Apple 平台上是一样的，都是第一类公民对象。

下面是一些目前为止不太能正常工作的或是已有未来计划的部分的要点：

* __libdispatch__：这是核心库的一部分且正在进行一些适配 Linux 支持的更新。你可以通过关注 [GitHub 上的 libdispatch 项目][libispatch-on-github]来了解开发进展。

* __某些 C API__：尽管总体而言在我们在支持支持的平台上提供了全面的支持，但是有一些 C 的架构我们尚未导入 Swift 之中。随之导致了某些 API 的不可用，例如那些提供可变参数/`va_list` 的架构。尽管如此，在最近几个月中 Swift 与 C 之间的互联互通性能已经得到了大幅度的提升，这让 Swift 得到了例如命名和匿名联合体、匿名结构和位字段的支持。

* __某些 `String` API__：标准库中的 `String` 类现在缺失一些重要 API 的实现，这是由于目前它们是和 Apple 平台上的 `NSString` 类相关联的。

* __执行期间的自省__：当 Apple 平台上的一个 Swift 类被 `@objc` 标记或者当他们是 `NSObject` 的子类时，你可以通过使用 Objective-C runtime 来枚举一个类中可用的方法或者使用选择器来调用各种方法。但由于它依赖于 Objective-C runtime 来实现，因此这项功能目前在 Linux 平台上是缺失的。

* __`Array<T> as? Array<S>`__ 例如对容器进行转换类型操作使之变为不同的相关类型的一些机制目前为止无法正常工作。因为这些机制都依赖于与 Objective-C 之间的一些桥接机制。

能够提供对 Linux 的支持给我们发布的开源项目开了一个好头，对此我们跟到十分激动，因为大家现在就可以去亲自试试看！因为还有很多未尽工作等着我们，因此我们衷心希望你能[为 Swift 贡献出自己的一份力量][contribute-to-swift]，让这个 Linux 支持端口变得更加完善。

<br />
<sub>Original article: [https://swift.org/blog/swift-linux-port/][original-article]</sub>

<sup>Original article copyright © 2016 Apple Inc. All rights reserved. Swift and the Swift logo are trademarks of Apple Inc.</sup>

[original-article]: https://swift.org/blog/swift-linux-port/
[pre-built-libraries-for-ubuntu]: https://swift.org/download/
[standard-library]: https://swift.org/compiler-stdlib/
[core-libraries]: https://swift.org/core-libraries/
[debug-swift-binaries]: https://swift.org/getting-started/#using-the-lldb-debugger
[experiment-in-the-repl]: https://swift.org/getting-started/#using-the-repl
[libispatch-on-github]: https://github.com/apple/swift-corelibs-libdispatch
[contribute-to-swift]: https://swift.org/contributing/
