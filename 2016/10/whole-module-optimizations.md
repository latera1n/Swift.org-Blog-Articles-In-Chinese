# Swift 3 中的模块整体优化

[阅读原文>>](https://swift.org/blog/whole-module-optimizations/)

2016 年 10 月 21 日&nbsp;&nbsp;&nbsp;&nbsp;[Erik Eckstein](https://github.com/eeckstein/)

模块整体优化是 Swift 编译器的优化模式。经过模块整体优化能够赢取的性能在很大程度上取决于项目本身，但可以提升至两倍甚至五倍。

模块整体优化可以使用 `-whole-module-optimization`（或 `-wmo`）编译器标志来启用，并且在 Xcode 8 中，对于所有新项目它默认是开启的。此外，在 Swift 软件包管理器的发布版本在编译时，使用了模块整体优化。

那么它究竟是做什么的呢？让我们先看看编译器在没开启模块整体优化的时候是如何工作的。

### 模块以及其编译

模块是一组 Swift 文件。每个模块编译为一个单独的分发单元——框架或是可执行文件。在单文件编译（不使用 `-wmo`）时，Swift 编译器将会为模块中的每个文件所单独调用。实际上，这就是在幕后所发生的一起。作为使用者，你不必手动执行此操作。 它会由编译器驱动程序或由 Xcode 的构建系统自动完成。

![single file compilation](https://swift.org/assets/images/wmo-blog/single-file.png)

在完成读取和解析源文件（也会完成一些其他工作，如类型检查）之后，编译器优化 Swift 代码，生成机器代码并写入目标文件。最后，链接器将组合所有对象文件并生成共享库或可执行文件。

在单文件编译时，编译器优化的范围只是单一文件。这就把函数交叉优化，比如函数内联或范型特化，限制在了同一文件中调用和定义的函数之中。

我们来看一个例子。假设我们有一个来自模块的文件，名为 utils.swift，它包含一个范型工具的数据结构 `Container <T>` 和一个方法 `getElement`，这个方法在整个模块中都被调用，例如在 main.swift 中。

main.swift：

>```swift
>func add (c1: Container<Int>, c2: Container<Int>) -> Int {
>  return c1.getElement() + c2.getElement()
>}
>```

utils.swift：

>```swift
>struct Container<T> {
>  var element: T
>
>  func getElement() -> T {
>    return element
>  }
>}
>```

当编译器优化 main.swift 时，它不知道如何实现 `getElement`，而只知道它存在。所以编译器生成一个对 `getElement` 的调用。另一方面，当编译器优化 utils.swift 时，它也并不知道被调用函数的具体类型。因此，它只能生成这个函数的泛型版本代码，而这会比生成的函数的具体版本的代码要慢得多。

即便 `getElement` 中的简单返回语句也需要在类型的元数据中查找以找出如何复制元素。它可以是一个简单的 `Int` 类型，也可以是一个更大的类型，甚至可以涉及一些引用计数的操作。编译器只是通通不知道这些。

### 模块整体优化

有了模块整体优化，编译器可以把很多事情做的更好。当在编译时使用 `-wmo` 选项，编译器将对属于同一个模块的所有文件进行整体优化。

![whole-module compilation](https://swift.org/assets/images/wmo-blog/wmo.png)

这会带来两大优势。首先，编译器观察到了模块中的所有功能的实现方法，所以它可以进行像函数的内联和函数的例化等一些优化。函数的例化意味着编译器会创建一个对于特定调用上下文优化过的新的函数版本。例如，编译器可以对于具体类型来例化范型函数。

在我们的示例中，编译器生成专用于具体类型 `Int`，即范型 `Container` 的其中一个版本的函数。

>```swift
>struct Container {
>  var element: Int
>
>  func getElement() -> Int {
>    return element
>  }
>}
>```

然后编译器可以将专门的 `getElement` 函数内联到 `add` 函数中。

>```swift
>func add (c1: Container<Int>, c2: Container<Int>) -> Int {
>  return c1.element + c2.element
>}
>```

这样编译下来只有几个机器指令。这与单文件代码相比有很大的区别，因为后者存在两个对于 `getElement` 函数的调用。

跨文件的函数例化和函数内联只是编译器能够进行模块整体优化的几个例子。即使编译器决定不执行函数的内联，那么如果仅仅观察到函数的实现也会对编译大有裨益。例如，它可以推断其关于引用计数操作的行为。知道了这个以后，编译器便能够去消除函数调用时的冗余引用计数操作。

模块整体优化的第二个重要好处是编译器可以推断非公共函数的所有使用之处。因为非公共函数只能在模块中使用，因此编译器可以确保查看到对于这些函数的所有引用。那么，编译器可以用这些信息做什么？

一个非常基本的优化是消除所谓的“死的”函数和方法，即从未被调用或以其他方式使用的函数和方法。有了模块整体优化，编译器便知道一个非公共函数或方法是否根本没有被调用，如果是这样的话，它便可以消除这个方法或者函数。那么为什么程序员写一个函数，而这个函数根本没被用上呢？其实，这并不是清除死函数这个优化的最重要的用处。因为函数变死通常是一些其他优化的副作用。

假设 `add` 函数是唯一调用 `Container.getElement` 之处。那么在内联 `getElement` 之后，此函数便不再使用，因此也可以删除它。即使编译器决定不内联 `getElement`，编译器也可以删除 `getElement` 的原始范型通用版本，因为 `add` 函数只调用它的专用版本。

### 编译时间

使用单文件编译，编译器驱动程序在单独的进程中启动每个文件的编译，使得编译可以并行完成。此外，自上次编译后未修改的文件是不需要重新编译的（假设所有依赖关系也未做修改）。这也就是所谓的增量编译。所有这些都节省了大量的编译时间，特别是如果你只做一个小的改变。但这在使用模块整体优化时是如何工作的？让我们更详细地看看编译器如何工作在模块整体优化模式。

![whole-module compilation details](https://swift.org/assets/images/wmo-blog/wmo-detail.png)

在内部，编译器在多个阶段运行：解析器、类型检查、SIL 优化和 LLVM 后端。

解析和类型检查在大多数情况下是非常快速的，但是我们仍然期望它在随后的 Swift 版本中会更快。SIL 优化器（SIL 代表“Swift 中间语言”）执行所有重要的 Swift 专用优化，如范型的特化、函数内联等。编译器的这个阶段通常需要占用编译时间的三分之一。大多数编译时间都由 LLVM 后端消耗，因为它运行了低级优化并执行了代码的生成。

在 SIL 优化器中执行完模块整体优化后，模块将再次被分为多个部分。LLVM 后端在多个线程中处理每个部分。它还避免了重新处理从上一次生成以来没有改变的代码部分。因此，即使有了模块整体优化，编译器也能够并行（多线程）和递增地执行编译的大部分工作。

### 结论

模块整体优化是一个获得最大性能，却又不必担心如何在模块中的文件之间分布 Swift 代码的好方法。如果优化（如上所述）在关键代码段中介入作用，编译性能可能会比单文件编译高出5倍。而且你还能够获得这种比单一整体程序优化方法所得编译时间更优的高性能。


<br />
<sub>Original article: [https://swift.org/blog/whole-module-optimizations/](https://swift.org/blog/whole-module-optimizations/)</sub>

<sup>Original article copyright © 2016 Apple Inc. All rights reserved. Swift and the Swift logo are trademarks of Apple Inc.</sup>

