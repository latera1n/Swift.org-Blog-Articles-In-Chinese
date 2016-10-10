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

一句警告：这个功能现在是无法使用的，因为 `#if swift(>=2.2)` 会导致 Swift 2.1 的编译器错误——它并不知道这意味着什么。但是一旦 Swift 3.0 发布后，且对于将来所有的版本来说，编译时的 Swift 版本检查将是对你工具箱的一个有益补充。

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

使用 `#selector` 将在编译时检查你的代码，以确保你要调用的方法确实存在。更好的是，如果方法不存在，你会得到一个编译错误：Xcode 将拒绝构建你的应用程序，由此，这个可能的的程序错误来源也会被驱逐到遗忘之地。

下面是使用 `#selector` 改写的上文的代码示例：

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

在代码构建的时候，编译器将返回“使用未解析的标识符‘addNewFireflyRefernce’”的错误——闪闪发亮！

有关更多信息，请参阅 [Swift 演进提案](https://github.com/apple/swift-evolution/blob/master/proposals/0022-objc-selectors.md)或阅读详细说明原理的 [swift-evolution-announce 帖子](https://lists.swift.org/pipermail/swift-evolution-announce/2016-January/000026.html)。

### 更多作为参数标签的关键字

Swift有很多关键字：诸如 `class`，`func`，`let` 和 `pulic` 等一些有着特殊意义而且不能被用作标识符的小东西。Swift 一直是允许你使用关键字作为参数标签，但是只有当你把它们放在反引号里，像这样：

>```swift
>func visitCity(name: String, `in` state: String) {
>    print("I'm going to visit \(name) in \(state)")
>}
>
>visitCity("Nashville", `in`: "Tennessee")
>```

从 Swift 2.2 开始，任何关键字可以用作参数标签，除了 `inout`，`var` 和 `let`。如果你有代码在反引号中使用了关键字，你会得到一个 Xcode 的修复提示让你去删除它们。 所以，像这样的代码现在已经成为可能：

>```swift
>func visitCity(name: String, in state: String) {
>    print("I'm going to visit \(name) in \(state)")
>}
>
>visitCity("Nashville", in: "Tennessee")
>```

欲了解有关这个改变的更多信息，请参阅 [Swift 演进提案](https://github.com/apple/swift-evolution/blob/master/proposals/0001-keywords-as-argument-labels.md)。

### 元组比较已经内置

元组是Swift中的一个基本数据类型，且带来了许多好处——尤其是能从函数返回多个值。 Swift 2.2 引入了比较两个元组平等的能力，这意味着它将对一个元组中的每个元素与另一个元组中的匹配元素进行检查，并报告为 true 如果所有元素都相匹配。

例如，以下代码将打印“No match”：

>```swift
>let singer = (first: "Taylor", last: "Swift")
>let alien = (first: "Justin", last: "Bieber")
>
>if singer == alien {
>    print("They match! That explains why you never see them together…")
>} else {
>    print("No match.")
>}
>```

Swift 2.2 元组将比较到第 6 个元数，这也就是只要元组包含不超过里六个元素，它们就可以进行比较的花哨的说法。

一个警告：Swift 2.2 在检查相等性时将忽略你的元素名称，因此 `singer` 和 `bird` 在下面的代码中将被视为相等：

>```swift
> let singer = (first: "Taylor", last: "Swift")
>let bird = (name: "Taylor", breed: "Swift")
>
>if singer == bird {
>    print("This explains why she sings so well.")
>} else {
>    print("No match.")
>}
>```

欲了解有关这个改变的更多信息，请参阅 [Swift 演进提案](https://github.com/apple/swift-evolution/blob/master/proposals/0015-tuple-comparison-operators.md)。

### 元组泼贱语法已经弃用

再多谈论元组一段时间，Swift 2.2 弃用了一个很少使用的功能，我提到它只是因为它惊奇的名字。 在 Swift 2.1 及更早版本中，你可以使用精心制作的元组来填充函数的参数。 因此，如果你有一个函数需要两个参数，你可以使用有两个元素的元去组调用它，只要元组有着正确的类型和元素名称。例如：

>```swift
>func describePerson(name: String, age: Int) {
>    print("\(name) is \(age) years old")
>}
>
>let person = ("Malcolm Reynolds", age: 49)
>describePerson(person)
>```

这种语法——被有爱地称为“元组泼贱语法”——是与大家习惯的 Swift 的自我记录，可读性风格的对立，因此它在 Swift 2.2 中已被弃用。

有关更多信息，请参阅 [Swift 演进提案](https://github.com/apple/swift-evolution/blob/master/proposals/0029-remove-implicit-tuple-splat.md)或阅读详细说明原理的 [swift-evolution-announce 帖子](https://lists.swift.org/pipermail/swift-evolution-announce/2016-February/000033.html)。


### C 语言风格的 `for` 循环已经弃用

虽然 Swift 有几个惯用的循环选项，C 语言风格的循环仍然是语言的一部分且偶尔被使用。例如：


>```swift
>for var i = 0; i < 10; i++ {
>    print(i)
>}
>```

这些用法已经在 Swift 2.2 中被弃用，并将在 Swift 3.0 中被完全删除——这是迈向从此不用再输入分号的另一步。

如果使用 Xcode，你可能会得到一个修复提示，它会将你的 C 语言风格的循环转换成现代 Swift 风格。 在前面代码的情况下，结果将使用如下的一个范围：

>```swift
>for i in 0 ..< 10 {
>    print(i)
>}
>```

但是，修复提示的功能有限，因此你需要自己做一些工作。 例如下面的两个循环，此时修复提示无法帮到你：

>```swift
>>for var i = 10; i > 0; i-- {
>    print(i)
>}
>
>for var i = 0; i < 10; i += 2 {
>    print(i)
>}
>```

在第一种情况下，应使用 `(1...10).reverse()` 来创建反向范围。 这_并不_同于写下 `i in 10...1`，这段代码将会被编译但在运行时缺会崩溃。 在第二种情况下，你应该使用 `stride(to:by:)` 来累加 2。 所以，在 Swift 2.2 中正确重写的两个循环应像是这样的：

>```swift
>for i in (1...10).reverse() {
>    print(i)
>}
>
>for i in 0.stride(to: 10, by: 2) {
>    print(i)
>}
>```

有关更多信息，请参阅 [Swift 演进提案](https://github.com/apple/swift-evolution/blob/master/proposals/0007-remove-c-style-for-loops.md)或阅读详细说明原理的 [swift-evolution-announce 帖子](https://lists.swift.org/pipermail/swift-evolution-announce/2015-December/000001.html)。

#### `++` 和 `--` 已经弃用

如果你使用 C 语言风格的循环，下一个变化可能会让你更加惊讶：`++` 和 `--` 也已经被弃用，不管是作为前缀还是后缀操作符。这意味着如同 `for var i = 0; i < 10; i++` 的代码包含不止一个，而是_两_个弃用，这即使在快速行进中的 Swift 世界里也相当不同寻常的。

此更改意味着下面的所有代码现在都已被弃用，并将在 Swift 3 中完全停止工作：

>```swift
>i++
>i--
>++i
>--i
>i = i++
>```










