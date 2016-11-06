# Swift 3 中的模块整体优化

模块整体优化是 Swift 编译器的优化模式。经过模块整体优化能够赢取的性能在很大程度上取决于项目本身，但它可以提升至两倍甚至五倍。

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

当编译器优化 main.swift 时，它不知道如何实现 getElement，它只知道它存在。所以编译器生成一个对 getElement 的调用。另一方面，当编译器优化 utils.swift 时，它也并不知道被调用函数的具体类型。因此，它只能生成这个函数的泛型版本代码，而它比生成的函数的具体版本的代码要慢得多的。

即便 `getElement` 中的简单返回语句也需要在类型的元数据中查找以找出如何复制元素。它可以是一个简单的 `Int` 类型，也可以是一个更大的类型，甚至可以涉及一些引用计数的操作。编译器只是通通不知道这些。

### 模块整体优化
