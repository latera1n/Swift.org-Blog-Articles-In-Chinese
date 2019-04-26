# 标准库中的条件一致性

[阅读原文>>](https://swift.org/blog/conditional-conformance/)

2018 年 1 月 8 日&nbsp;&nbsp;&nbsp;&nbsp;[Ben Cohan](https://twitter.com/airspeedswift/)

Swift 4.1 的编译器带来了下一阶段的改进——来自[范型路线图：条件一致性](https://github.com/apple/swift/blob/master/docs/GenericsManifesto.md)。

本文将介绍 Swift 标准库中如何引入这一备受期待的功能，以及它如何影响您和您的代码。

## Equatable 容器

条件一致性最显著的好处是能够使存储其他类型的类型符合 `Equatable` 协议，比如 `Array` 数组或 `Optional` 可选类型。`Equatable` 协议是保证您可以在两个类型实例之间使用 `==` 的协议。让我们来看看为什么符合这个协议是非常有用的。

您一直可以使用 `==` 于两个任何等价元素的数组：

> ```swift
> [1,2,3] == [1,2,3]     // true
> [1,2,3] == [4,5]       // false
> ```

或者用在两个包含着等价类型的可选值上面：

> ```swift
> // The failable initializer from a String returns an Int?
> Int("1") == Int("1")                        // true
> Int("1") == Int("2")                        // false
> Int("1") == Int("swift")                    // false, Int("swift") is nil
> ```

这可以通过 `==` 运算符的重载来实现，就像这个对于 `Array` 类型一样：

> ```swift
> extension Array where Element: Equatable {
>     public static func ==(lhs: [Element], rhs: [Element]) -> Bool {
>         return lhs.elementsEqual(rhs)
>     }  
> }
> ```

但是，仅仅因为他们实现了 `==` 并不意味着 `Array` 类型或 `Optional` 类型符合 `Equatable`。由于这些类型可以存储非等价类型，因此我们需要能够表达它们只有在存储等价类型时才是等价的。

这意味着这些 `==` 运算符有一个很大的局限性：它们无法在两个层的深度中使用。如果你在 Swift 4.0 中尝试过类似的代码：

> ```swift
> // convert a [String] to [Int?]
> let a = ["1","2","x"].map(Int.init)
> 
> a == [1,2,nil]    // expecting 'true'
> ```

你会得到一个编译器错误：

> Binary operator ‘==’ cannot be applied to two ‘[Int?]’ operands.

这是因为如上所示，对于 `Array` 的 `==` 实现要求数组的元素是等价的，而 `Optional` 类型是不可等价的。

而有了条件一致性，我们便可以解决这个问题。它允许我们使用已经定义的 `==` 运算符来编写这些类型是符合 `Equatable` 的，如果它们所基于的类型是等价的：

> ```swift
> extension Array: Equatable where Element: Equatable {
>     // implementation of == for Array
> }
> 
> extension Optional: Equatable where Wrapped: Equatable {
>     // implementation of == for Optional
> }
> ```

`Equatable` 的一致性带来了 `==` 以外的其他好处。拥有等价元素为集合类型提供了其他辅助函数，例如搜索：

> ```swift
> a.contains(nil)                 // true
> [[1,2],[3,4],[]].index(of: [])  // 2
> ```

使用条件一致性，Swift 4.1 的 `Optional`、`Array` 和 `Dictionary` 类型现在已符合 `Equatable` 和 `Hashable` 协议，只要它们的值或元素符合这些协议。

这种方法也适用于 `Codable` 协议。如果您尝试编码不可编码类型的数组，现在将获得编译时错误，而不是您以前获得的运行时的自陷。

## 集合类型协议

条件一致性还有助于逐步为您的类型构建功能，从而避免代码重复。为了探索通过使用条件一致性在 Swift 标准库中实现的一些变化，我们将使用向 `Collection` 类型中添加一个新功能：惰性拆分的示例。我们将创建一个新类型，用于从集合中拆分切片，然后查看当基本集合是双向的时，条件一致性如何用于添加双向性能。

### 迫切 vs 惰性拆分

Swift 中的 `Sequence` 协议具有一个 `split` 方法，它将一个序列拆分成一个 `Array` 类型的子序列。

> ```swift
> let numbers = "15,x,25,2"
> let splits = numbers.split(separator: ",")
> // splits == ["15","x","25","2"]
> var sum = 0
> for i in splits {
>     sum += Int(i) ?? 0
> }
> // sum == 42
> ```

我们将这个 `split` 方法描述为“迫切”的，因为它迫切地将序列拆分为子序列，并在调用它们后立即将它们放入数组中。

但是假设您只想要前几个子序列？假设您有一个巨大的文本文件，并且您只想获取它的初始行以显示为预览。您不希望处理整个文件只是为了使用开头的几行。

这种问题也存在于 `map` 和 `filter` 之类的操作，它们在 Swift 中默认也是同样迫切的。为避免这种情况，标准库具有“惰性”序列和集合。您可以通过使用 `lazy` 的属性来访问它们。这些惰性序列和集合具有不立即运行的 `map` 等操作的实现。相反，它们在访问元素时即时执行映射或过滤。例如：

> ```swift
> // a huge collection
> let giant = 0..<Int.max
> // lazily map it: no work is done yet
> let mapped = giant.lazy.map { $0 * 2 }
> // sum the first few elements
> let sum = mapped.prefix(10).reduce(0, +)
> // sum == 90
> ```

创建 `mapped` 集合时，不会发生映射。实际上，您可能会注意到，如果您对 `giant` 的每个元素执行映射操作，它将自陷：当值加倍时，在中途它会溢出，而不再适合 `Int` 类型。但是使用惰性映射时，只有在访问元素时才会发生映射。因此，在此示例中，当 `reduce` 操作对它们求和时，将仅计算前十个值。

### 一个惰性拆分的封装器

标准库没有惰性拆分操作。下面的示例给出了一个可用的惰性拆分是如何工作的草稿。如果您有兴趣为 Swift 做出贡献，那么这将成为一个很好的[初始 bug](https://bugs.swift.org/browse/SR-6691?jql=labels%20%3D%20StarterProposal) 和[演进提议](https://github.com/apple/swift-evolution/blob/master/process.md)。

首先，我们创建一个简单的通用封装结构，它可以包含任何基本集合，以及一个用于标识要拆分的元素的闭包：

> ```swift
> struct LazySplitCollection<Base: Collection> {
>     let base: Base
>     let isSeparator: (Base.Element) -> Bool
> }
> ```

（我们会忽略访问控制之类的东西以保持这篇文章的代码的简洁性）

接下来我们遵循 `Collection` 协议。要成为一个集合，你只需要提供四个东西：一个 `startIndex` 和一个 `endIndex`；一个 `subscript` 用来给出给定索引的元素，以及一个 `index(after:)` 方法将索引向后推进一个。



<br />
<sub>Original article: <a href="https://swift.org/blog/conditional-conformance/">https://swift.org/blog/conditional-conformance/</a></sub>

<sup>Original article copyright © 2018 Apple Inc. All rights reserved. Swift and the Swift logo are trademarks of Apple Inc.</sup>
