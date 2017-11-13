# Swift 4.0 中字典和集合的改进

[阅读原文>>](https://swift.org/blog/dictionary-and-set-improvements/)

2017 年 10 月 4 日&nbsp;&nbsp;&nbsp;&nbsp;[Nate Cook](https://twitter.com/nnnnnnnn/)

在 Swift 的最新版本中，字典和集合获得了许多新的方法和初始化器，这使常规任务比以往任何时候都变得更容易。现在，分组、过滤和转换值等操作可以在单一步骤中进行，这可以让您编写更具表现力且高效的代码。

这篇文章探讨了这些新的变化，以市场中的一些杂货数据为例。这个由名称和所属部门组成的 `GroceryItem` 结构体将用作数据类型：

> ```swift
> struct GroceryItem: Hashable {
>     var name: String
>     var department: Department
> 
>     enum Department {
>         case bakery, produce, seafood
>     }
> 
>     static func ==(lhs: GroceryItem, rhs: GroceryItem) -> Bool {
>         return (lhs.name, lhs.department) == (rhs.name, rhs.department)
>     }
> 
>     var hashValue: Int {
>         // 组合名称和所属部门的哈希值
>         return name.hashValue << 2 | department.hashValue
>     }
> }
> 
> // 为我们的商店创建一些杂货:
> let 🍎 = GroceryItem(name: "Apples", department: .produce)
> let 🍌 = GroceryItem(name: "Bananas", department: .produce)
> let 🥐 = GroceryItem(name: "Croissants", department: .bakery)
> let 🐟 = GroceryItem(name: "Salmon", department: .seafood)
> let 🍇 = GroceryItem(name: "Grapes", department: .produce)
> let 🍞 = GroceryItem(name: "Bread", department: .bakery)
> let 🍤 = GroceryItem(name: "Shrimp", department: .seafood)
> 
> let groceries = [🍎, 🍌, 🥐, 🐟, 🍇, 🍞, 🍤]
> ```

以下示例使用 `groceries` 数组并利用这些新工具来构建和转字典。

## 根据键来分组值

<img alt="从名称和值构建一个字典" src="https://swift.org/assets/images/dictionary-blog/grouping.png" srcset="https://swift.org/assets/images/dictionary-blog/grouping_2x.png 2x" style="float: right;">

一个新的分组初始化器使得从一系列值中构建一个字典，并按照从这些值计算的键分组变得很容易。我们将使用这个新的初始化器来构建按照其所属部门分组的杂货字典。

要在早期版本的 Swift 中执行此操作，您可以使用迭代从头构建一个字典。这需要类型注释，手动迭代并检查来每一个键在字典中是否已经存在。

> ```swift
> // Swift <= 3.1
> var grouped: [GroceryItem.Department: [GroceryItem]] = [:]
> for item in groceries {
>     if grouped[item.department] != nil {
>         grouped[item.department]!.append(item)
>    } else {
>         grouped[item.department] = [item]
>     }
> }
> ```

通过对 Swift 的更新，只需要一行代码，您就可以使用 `Dictionary(drouping:by)` 初始化器来创建字典，向其中传入一个为数组中的每个元素返回一个键的闭包。在以下代码中，这个闭包返回每个杂货物品的所属部门：

> ```swift
> // Swift 4.0
> let groceriesByDepartment = Dictionary(grouping: groceries,
>                                        by: { item in item.department })
> // groceriesByDepartment[.bakery] == [🥐, 🍞]
> ```

由此产生的 `groceriesByDepartment` 字典对于杂货列表中的每个所属部门都有着一个条目。每个键的值是对应该所属部门的杂货所组成的数组，与原始列表的顺序相同。 使用 `.bakery` 为键，`groceriesByDepartment` 将返回数组 `[🥐, 🍞]`。


## 转换一个字典的值

您可以通过使用新的 `mapValues(_:)` 方法来转换字典的值，同时保留相同的键。此代码将 `groceriesByDepartment` 中物品组成的数组转换为其计数，为其中每个物品所属部门的计数创建一个查找表：

> ```swift
> let departmentCounts = groceriesByDepartment.mapValues { items in items.count }
> // departmentCounts[.bakery] == 2
> ```

因为字典具有所有相同的键，只是具有不同的值，所以它可以使用与原始字典相同的内部布局，并且不需要重新计算任何哈希值。 这使得调用 `mapValues(_:)` 比从头开始重建字典来得更快。

## 从键值对构建字典

<img alt="按照杂货的所属部门分组" src="https://swift.org/assets/images/dictionary-blog/uniqueKeys.png" srcset="https://swift.org/assets/images/dictionary-blog/uniqueKeys_2x.png 2x" style="float: right;">

现在，您可以使用两种不同的初始化器以从键值对的序列创建字典：一种用于其中只具有唯一的键的情况，另一种用于其中含有重复的键的情况。

若要从一个键的序列和一个值的序列开始构建字典，可以使用 `zip(_:_:)` 函数将它们组合成单个序列对。例如，这段代码创建了一个元组序列，其中包含一个杂货物品名称和这个物品本身：

> ```swift
> let zippedNames = zip(groceries.map { $0.name }, groceries)
> ```

`zippedNames` 的每个元素是一个 `(String, GroceryItem)` 元组，第一个元素是 `("Apples", 🍎)`。因为每个杂货物品都有唯一的名称，所以下面的代码成功地创建了一个字典，它使用名称作为杂货商品的键：


> ```swift
> var groceriesByName = Dictionary(uniqueKeysWithValues: zippedNames)
> // groceriesByName["Apples"] == 🍎
> // groceriesByName["Kumquats"] == nil
> ```

仅当您确定您的数据具有唯一键时才使用 `Dictionary(uniqueKeysAndValues:)` 初始值设定项。序列中任何重复的键都会触发运行时的错误。

如果您的数据有（或可能有）重复键，请使用新的合并初始化器 `Dictionary(_:uniquingKeysWith:)`。这个初始化器接受一个键值对序列和一个闭包，每当一个键被重复时，这个闭包都会被调用。这个唯一化的闭包接受共享着同一个键的第一个和第二个值作为参数，且可以返回现有值，新值，或者将它们组合起来，任由您决定。

例如，下面的代码通过调用 `Dictionary(_：uniquingKeysWith:)` 将包含 `(String, String)` 元组的数组转换为字典。请注意，`dog` 是两个键值对中的共同的键。

> ```swift
> let pairs = [("dog", "🐕"), ("cat", "🐱"), ("dog", "🐶"), ("bunny", "🐰")]
> let petmoji = Dictionary(pairs,
>                          uniquingKeysWith: { (old, new) in new })
> // petmoji["cat"] == "🐱"
> // petmoji["dog"] == "🐶"
> ```

当运行至带有 `"dog"` 键的第二个键值对时，调用唯一化闭包并分别传入旧值和新值（`"🐕"` 和 `"🐶"`）。因为闭包总是返回第二个参数，所以在结果中，`"🐶"` 是 `dog` 键的值。

## 选择某些条目

字典现在有一个 `filter(_:)` 方法，它返回一个字典，而不是像在早期版本的 Swift 中一样仅仅返回一个键值对的数组。传递一个接受对为参数的闭包并返回 `true` 如果该键值对应存在于结果中。

> ```swift
> func isOutOfStock(_ item: GroceryItem) -> Bool {
>     // 在存货清单中查找 `item`
> }
> 
> let outOfStock = groceriesByName.filter { (_, item) in isOutOfStock(item) }
> // outOfStock["Croissants"] == 🥐
> // outOfStock["Apples"] == nil
> ```

此代码对于每个杂货物品都调用 `isOutOfStock(_:)` 函数，并仅仅保留缺货的杂货物品。

## 使用默认值

字典现在有第二个基于键的下标，可以更容易地获取和更新值。下面的代码定义了一个简单的购物车，实现为一个物品和它们计数的字典：

> ```swift
> // 从一个香蕉开始
> var cart = [🍌: 1]
> ```

由于某些键在字典中可能没有对应的值，因此在使用键查找值时，其结果是一个可选类型。

> ```swift
> // 一根香蕉:
> cart[🍌]    // Optional(1)
> // 然而并没有虾:
> cart[🍤]    // nil
> ```

现在，您可以使用键和 `default` 参数作为为字典下标，而不必使用零合并运算符（`??`）将可选值转换为您需要的实际计数。如果找到该键，则忽略该默认值并返回其值，如果找不到该键，则查找该下标返回您提供的默认值。

> ```swift
> // 仍是一个香蕉:
> cart[🍌, default: 0]    // 1
> // 且仍然没有虾:
> cart[🍤, default: 0]    // 0
> ```

您甚至可以通过新的下标修改一个值，这将简化将新物品添加到购物车所需的代码。

> ```swift
> for item in [🍌, 🍌, 🍞] {
>     cart[item, default: 0] += 1
> }
> ```

当这个循环处理每根香蕉（`🍌`）时，当前值被检索、递增并存储放回字典中。当添加面包（`🍞`）时，由于在字典中没有找到该键，便返回默认值（`0`）作为代替。在该值递增之后，字典将添加新的键值对。

在循环的最后，`cart` 为 `[🍌: 3, 🍞: 1]`。

## 合并两个字典

<img alt="合并两个购物车" src="https://swift.org/assets/images/dictionary-blog/merging.png" srcset="https://swift.org/assets/images/dictionary-blog/merging_2x.png 2x" style="float: right;">

除了更容易的增量更改之外，使用将一个字典合并至另一个的方法，字典也使得批量更改变得更简单。

要合并 `cart` 和其他字典的内容，可以使用变异的 `merge(_uniquingKeysWith:)` 方法。您传递的唯一化闭包的作用方式与在 `Dictionary(_uniquingKeysWith:)` 初始化程序中的闭包相同：只要有两个具有相同键的值，就会调用它，并返回一个值、另一个值或值的组合。

在此示例中，传递加法运算符作为 `uniquingKeysWith` 的参数，将所有匹配键的计数相加，以使更新后的购物车具有每个物品的正确总数：

> ```swift
> let otherCart = [🍌: 2, 🍇: 3]
> cart.merge(otherCart, uniquingKeysWith: +)
> // cart == [🍌: 5, 🍇: 3, 🍞: 1]
> ```

要使用合并的内容创建新字典而不是就地合并，请使用非变异的 `merge(_:uniquingKeysWith:)` 方法。

## 而这并不是全部…

还有一些我们没有涉及到的内容。字典现在具有新功能的自定义 `keys` 和 `values` 集合。`keys` 集合保持快速键查找，而可变的 `values` 集合可让您就地修改它们的值。

像字典一样，集合获得了一个新的 `filter(_:)` 方法，返回一组相同的类型，而不是像早期版本的 Swift 那样返回数组。而且终于，集合和字典现在都揭示了它们的当前容量并添加了一个 `reserveCapacity(_:)` 方法。有了这些添加的功能，您可以看到并控制其内部存储的大小。

除了自定义`keys` 和 `values` 的集合之外，所有这些更改都可以在 Swift 3.2 中找到。即使您还没有切换至使用 Swift 4.0，您仍然可以从今天可以开始就利用这些改进！

您可以在[字典](https://developer.apple.com/documentation/swift/dictionary)和[集合](https://developer.apple.com/documentation/swift/set)文档中找到关于所有这些新功能的更多信息，或者阅读 Swift 演进提案中关于[自定义 `keys` 和 `values` 集合](https://github.com/apple/swift-evolution/blob/master/proposals/0154-dictionary-key-and-value-collections.md)以及[其他字典和集增强](https://github.com/apple/swift-evolution/blob/master/proposals/0165-dict.md)的这些新添加功能背后原理的更多信息。

<br />
<sub>Original article: <a href="https://swift.org/blog/dictionary-and-set-improvements/">https://swift.org/blog/dictionary-and-set-improvements/</a></sub>

<sup>Original article copyright © 2017 Apple Inc. All rights reserved. Swift and the Swift logo are trademarks of Apple Inc.</sup>
