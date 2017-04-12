# Swift 3.1 现已发布！

[阅读原文>>](https://swift.org/blog/swift-3-1-released/)

2017 年 3 月 27 日

Swift 3.1 现已正式发布！Swift 3.1 是一个次要版本，其中包含对标准库的更改和改进。由于 IBM 和社区其他成员的努力，它还包括许多 Swift 在 Linux 上实现的更新。此外还有一些 Swift 软件包管理器的更新。

### 语言改变

Swift 3.1 是一个次要的语言发布版本。它与 Swift 3.0 兼容，并包含以下语言改变和更新，其中大部分是通过 Swift [演进过程][https://swift.org/contributing/#participating-in-the-swift-evolution-process]进行的：

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

查看更多：[SE-0045: 添加 `prefix(while:)` 和 `drop(while:)` 至标准库][https://github.com/apple/swift-evolution/blob/master/proposals/0045-scan-takewhile-dropwhile.md]

#### 根据 Swift 版本提供的可用性

Swift 3.1 扩展了 `@availability` 属性以使用 Swift 版本来指示声明的生命周期。例如，在 Swift 3.1 中已移除的 API 将被写为：

>```swift
> @available(swift, obsoleted: 3.1)
> class Foo {
>   //...
> }
>```