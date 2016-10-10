# Swift 2.2 中的新功能

[阅读原文>>](https://swift.org/blog/swift-2-2-new-features/)

2016 年 3 月 30 日

Swift 2.2 带来了新的语法，新的功能，也带来了一些被弃用的用法。它是将在今年晚些时间发布且有着更多更改的 Swift 3 之前的过渡的发布版。而且，Swift 2.2 中的变化是为了向 Swift 3 中更广泛的目标看齐，这些目标致力于使核心语言和标准库逐渐地稳定下来——通过添加缺失的功能，改善已有的功能以及移除语言中已经不需要的东西。Swift 2.2 中的所有更改已经经过了社区驱动的 [Swift 演进过程](https://swift.org/contributing/#participating-in-the-swift-evolution-process) ——自几个月前 Swift 开源以后，在此超过 30 个提案已经被提交、审查并且接受。

Swift 2.2 中的更改会对你的代码有着直接的影响，本文将用示例代码带领你并解释什么发生了变化且这些变化为什么会发生，以帮助你快速迁移到 Swift 2.2 的新语法。

作为提醒，“弃用”指的是那些不再建议使用，且将在以后完全删除的函数或语言功能。在实践中，这意味着现在 Swift 会发出编译器警告，并可能在未来的 Swift 3 中在产生一个编译器错误。

### 编译时 Swift 版本检查

Swift 在不断前进的路上可能偶尔会让开发者感觉难以跟上，但是对于库的开发者来说，他们会感到更难——如果有成千上万的人正在使用的你库，你可能需要支持不止一个版本的 Swift 以保证所有人都可以使用你的产品，不管他们使用的是什么版本。

Swift 2.2 引入了一个新的编译器指令，使得跨版本兼容不在话下：你现在可以指定一段代码块，其只被支持特定 Swift 语言版本的编译器读取。例如：

>```swift
>#if swift(>=3.0)
>print("Running Swift 3.0 or later")
>#else
>print("Running Swift 2.2 or earlier")
>#endif
>```

这个指令与 Swift 2 引入的，在运行时检查的 `#available` 语法不同——新功能是在编译时，这使得没有通过语言检查的代码在实际上变得不可见。如果你想的话你可以写出这样的代码：

>```swift
>#if swift(>=3.0)
>print("Running Swift 3.0 or later")
>#else
>BRING BACK WASH HE WAS MY FAVORITE
>#endif
>```

只要 Swift 编译器指向 3.0 或更高版本，编译将一切顺利因为编译器会忽略那些大写字母的消息。

一句警告：这个功能现在是无法使用的，因为`#if swift(>=2.2)`会导致 Swift 2.1 的编译器错误——它并不知道这意味着什么。但是一旦 Swift 3.0 发布后，且对于将来所有的版本来说，编译时的 Swift 版本检查将是对你工具箱的一个有益补充。

欲了解有关这个改变的更多信息，请参阅 [Swift 演进提案](https://github.com/apple/swift-evolution/blob/master/proposals/0020-if-swift-version.md)。

### 编译时检查的选择器

在 Swift 2.1 中，像这样的代码将被编译而不会产生任何问题：

>```swift
>override func viewDidLoad() {
>    super.viewDidLoad()
>
>    navigationItem.rightBarButtonItem =
>        UIBarButtonItem(barButtonSystemItem: .Add, target: self,
>                        action: "addNewFireflyRefernce")
>}
>
>func addNewFireflyReference() {
>    gratuitousReferences.append("We should start dealing in black-market beagles.")
>}
>```

虽然代码本身在语法上是没有问题的，但是应用程序会奔溃因为导航栏的按钮调用了一个方法 `addNewFireflyRefernce()` —— 在“reference”中少了一个“e”。这些简单的拼写错误会很轻易的导致程序错误，因此 Swift 2.2 废除了使用字符串作为选择器，作为代替，引入了新的语法：`#selector`。

使用`#selector`将在编译时检查你的代码，以确保你要调用的方法确实存在。更好的是，如果方法不存在，你会得到一个编译错误：Xcode 将拒绝构建你的应用程序，由此，这个可能的的程序错误来源也会被驱逐到遗忘之地。

下面是使用`#selector`改写的上文的代码示例：

>```swift
>override func viewDidLoad() {
>    super.viewDidLoad()
>
>    navigationItem.rightBarButtonItem =
>        UIBarButtonItem(barButtonSystemItem: .Add, target: self,
>                        action: #selector(addNewFireflyRefernce))
>}
>
>func addNewFireflyReference() {
>    gratuitousReferences.append("Curse your sudden but inevitable betrayal!")
>}
>```
