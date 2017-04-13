# Swift 3.1 现已发布！

[阅读原文>>](https://swift.org/blog/swift-3-1-released/)

2017 年 3 月 27 日

Swift 3.1 现已正式发布！Swift 3.1 是一个次要版本，其中包含对标准库的更改和改进。由于 IBM 和社区其他成员的努力，它还包括许多 Swift 在 Linux 上实现的更新。此外还有一些 Swift 软件包管理器的更新。

### 语言改变

Swift 3.1 是一个次要的语言发布版本。它与 Swift 3.0 兼容，并包含以下语言改变和更新，其中大部分是通过 Swift [演进过程](https://swift.org/contributing/#participating-in-the-swift-evolution-process)进行的：

#### 全新的 `Sequence` 协议成员

 `Sequence` 协议现已新增了两个成员：

>```swift
> protocol Sequence {
>   // ...
>   /// 返回一个子序列：跳过当 `predicate` 返回 `true` 时的元素并返回剩下的元素。
>   func drop(while predicate: (Self.Iterator.Element) throws -> Bool) rethrows -> Self.SubSequence
>   /// 返回一个子序列：返回最开始的元素直到 `predicate` 返回 `false` 并跳过剩下的元素。
>   func prefix(while predicate: (Self.Iterator.Element) throws -> Bool) rethrows -> Self.SubSequence
> }
>```

查看更多：[SE-0045: 添加 `prefix(while:)` 和 `drop(while:)` 至标准库](https://github.com/apple/swift-evolution/blob/master/proposals/0045-scan-takewhile-dropwhile.md)

#### 根据 Swift 版本提供的可用性

Swift 3.1 扩展了 `@availability` 属性以使用 Swift 版本来指示声明的生命周期。例如，在 Swift 3.1 中已移除的 API 将被写为：

>```swift
> @available(swift, obsoleted: 3.1)
> class Foo {
>   //...
> }
>```

查看更多：[SE-0041: 根据 Swift 版本提供的可用性](https://github.com/apple/swift-evolution/blob/master/proposals/0141-available-by-swift-version.md)

#### 增强的数字转换初始化器

Swift 3.1 为所有数字类型添加了一系列新的转换初始值，使其可以成功完成转换且不会丢失信息或返回空。

查看更多：[SE-0080: 可失败的数字转换初始化器](https://github.com/apple/swift-evolution/blob/master/proposals/0080-failable-numeric-initializers.md)

#### `UnsafeMutablePointer.initialize(from:)` 的弃用和替换

接受一个 `Collection` 的 `UnsafeMutablePointer.initialize(from:)` 的版本已经弃用，以支持 `UnsafeMutableBufferPointer` 上的一个接受 `Sequence` 的新方法，这样做的目的是为了提高内存安全性，并使序列中的内存能够更快地初始化。

查看更多：[SE-0147: 改动 UnsafeMutablePointer.initialize(from:) 以使用 UnsafeMutableBufferPointer](https://github.com/apple/swift-evolution/blob/master/proposals/0147-move-unsafe-initialize-from.md)

#### Linux 实现的增强

* `NSDecimal` 的实现
* `NSLengthFormatter` 的实现
* `Progress` 的实现
* `URLSession` 功能的许多改进，包括 API 覆盖和 `libdispatch` 的优化使用
* `NSArray`，`NSAttributedString` 等等 API 覆盖的增强
* `Data` 中的显着改善性能。[点击此处查看细节](https://github.com/apple/swift-corelibs-foundation/blob/master/Docs/Performance%20Refinement%20of%20Data.md)
* JSON 序列化增强的性能
* `NSUUID`，`NSURLComponents` 及其他中的内存泄漏的修复
* 增强的了测试覆盖，特别是在 `URLSession` 中