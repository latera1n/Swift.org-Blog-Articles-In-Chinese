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
> // Create some groceries for our store:
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

<img alt="按照杂货的所属部门分组" src="https://swift.org/assets/images/dictionary-blog/grouping.png" srcset="https://swift.org/assets/images/dictionary-blog/grouping_2x.png 2x" style="float: right;">

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

通过对 Swift 的更新，只需要一行代码，您就可以使用 `Dictionary(drouping:by)` 初始化器来创建字典，向其中传入一个为数组中的每个元素返回一个键的闭包。在以下代码中，这个闭包返回每个杂货项目的所属部门：

> ```swift
> // Swift 4.0
> let groceriesByDepartment = Dictionary(grouping: groceries,
>                                        by: { item in item.department })
> // groceriesByDepartment[.bakery] == [🥐, 🍞]
> ```

由此产生的 `groceriesByDepartment` 字典对于杂货列表中的每个所属部门都有着一个条目。每个键的值是对应该所属部门的杂货所组成的数组，与原始列表的顺序相同。 使用 `.bakery` 为键，`groceriesByDepartment` 将返回数组 `[🥐, 🍞]`。


## 转换一个字典的值

您可以通过使用新的 `mapValues(_:)` 方法来转换字典的值，同时保留相同的键。此代码将 `groceriesByDepartment` 中项目组成的数组转换为其计数，为其中每个项目所属部门的计数创建一个查找表：

> ```swift
> let departmentCounts = groceriesByDepartment.mapValues { items in items.count }
> // departmentCounts[.bakery] == 2
> ```

















