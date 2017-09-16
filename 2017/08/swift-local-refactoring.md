# Swift 局部重构

[阅读原文>>](https://swift.org/blog/swift-local-refactoring/)

2017 年 8 月 22 日&nbsp;&nbsp;&nbsp;&nbsp;[Xi Ge](https://twitter.com/xge_apple/)

Xcode 9 包括一个全新的重构引擎。它既可以在局部的单个 Swift 源文件，又可以在全局的范围内转换代码，例如重命名出现在多个文件甚至不同语言中的方法或属性。局部重构背后的逻辑完全在编译器和 SourceKit 中实现，且已经在 [swift 代码库](https://github.com/apple/swift)中开源。因此，每一个 Swift 爱好者都可以为语言贡献重构的操作。这篇文章讨论了如何在 Xcode 中实现并显现出一个简单的重构操作。

## 重构的种类

**局部重构**发生在单个文件的范围内。局部重构的例子包括提取方法和提取重复表达式。跨多个文件改变代码（如全局重命名）的**全局重构**目前需要 Xcode 的特殊协调，目前无法在 Swift 代码库内自行实现。这篇文章专注于局部重构，因为它们本身可以是非常强大的。

在编辑器中，重构的动作是由用户光标选择启动的。根据它们是如何初始化的，我们将重构动作分类为基于光标或基于范围的操作。**基于光标的重构**具有一个由光标位置充分指定的 Swift 源文件中的重构目标，例如重命名重构。相比之下，**基于范围的重构**需要一个开始和结束位置来指定其目标，例如提取方法重构。为了方便实现这两个类别，Swift 代码库提供了预分析结果，称为 [ResolvedCursorInfo](https://github.com/apple/swift/blob/7f29b362d68eb990a592257850aabadb24de61df/include/swift/IDE/Utils.h#L158) 和 [ResolvedRangeInfo](https://github.com/apple/swift/blob/60a91bb7360dde5ce9531889e0ed10a2edbc961a/include/swift/IDE/Utils.h#L344)，以解析有关 Swift 源文件中光标位置或范围的几个常见问题。

例如，[ResolvedCursorInfo](https://github.com/apple/swift/blob/7f29b362d68eb990a592257850aabadb24de61df/include/swift/IDE/Utils.h#L158) 可以告诉我们源文件中的位置是否指向表达式的开头，如果是，则提供该表达式的相应编译器对象。或者，如果光标指向一个名称，[ResolvedCursorInfo](https://github.com/apple/swift/blob/7f29b362d68eb990a592257850aabadb24de61df/include/swift/IDE/Utils.h#L158) 将给出与该名称对应的声明。类似地，[ResolvedRangeInfo](https://github.com/apple/swift/blob/60a91bb7360dde5ce9531889e0ed10a2edbc961a/include/swift/IDE/Utils.h#L344) 封装了有关给定源范围的信息，例如该范围是否具有多个入口点或出口点。

要为 Swift 实现一个新的重构，我们不需要从光标或范围位置的原始表示开始；相反地，我们可以从 [ResolvedCursorInfo](https://github.com/apple/swift/blob/7f29b362d68eb990a592257850aabadb24de61df/include/swift/IDE/Utils.h#L158) 和 [ResolvedRangeInfo](https://github.com/apple/swift/blob/60a91bb7360dde5ce9531889e0ed10a2edbc961a/include/swift/IDE/Utils.h#L344) 开始，使用它们即可导出重构特定的分析。

## 基于光标的重构

![基于光标的重构](https://swift.org/assets/images/local-refactoring/Cursor.png)

基于光标的重构由 Swift 源文件中的光标位置启动。重构操作实现了重构引擎使用的方法，即在 IDE 上显示可用操作并执行转换。

具体来说，为了显示出来可用的操作时：

1. 用户在 Xcode 编辑器中选择一个位置。
2. Xcode 向 [sourcekitd](https://github.com/apple/swift/tree/master/tools/SourceKit) 发出一个请求，来查看该位置存在的可用操作。
3. 对每个实现的重构操作使用一个 `ResolvedCursorInfo` 对象进行查询，以查看该操作是否适用于该位置。
4. 适用的操作列表作为响应从 [sourcekitd](https://github.com/apple/swift/tree/master/tools/SourceKit) 中返回，并由 Xcode 显示给用户。

当用户选择其中一个可用的操作时：

1. Xcode 向 [sourcekitd](https://github.com/apple/swift/tree/master/tools/SourceKit) 发出一个请求，以在源位置处执行选定的操作。
2. 对特定重构操作使用源于自同一位置的一个 `ResolvedCursorInfo` 对象进行查询，以验证该操作是否适用。
3. 重构操作被请求使用文本源代码编辑执行转换操作。
4. 返回源代码编辑作为来自 [sourcekitd](https://github.com/apple/swift/tree/master/tools/SourceKit) 的响应，并由 Xcode 编辑器应用。

要实现*字符串本地化*重构，我们需要先在 [RefactoringKinds.def](https://github.com/apple/swift/blob/60a91bb7360dde5ce9531889e0ed10a2edbc961a/include/swift/IDE/RefactoringKinds.def) 文件中声明这个重构，其中应包含如下条目：

> ```c++
> CURSOR_REFACTORING(LocalizeString, "Localize String", localize.string)
> ```

`CURSOR_REFACTORING` 指定了这个重构操作是在光标位置初始化的，由此在实现中将可使用 [ResolvedCursorInfo](https://github.com/apple/swift/blob/7f29b362d68eb990a592257850aabadb24de61df/include/swift/IDE/Utils.h#L158)。第一个字段 `LocalizeString` 在 Swift 代码库中指定了这个重构操作的内部名称。在此示例中，与此重构相对应的类的名称为 `RefactoringActionLocalizeString`。字符串文字 `"Localize String"` 是在 UI 中向用户显示的这个重构操作的显示名称。最后，"localize.string" 是一个稳定的，用于表示重构操作的键名，Swift 工具链用其与源代码编辑器通信。此条目还允许 C ++ 编译器为 String 局部重构及其调用者生成类的存根。由此，我们可以专注于实现所需的功能。

在指定这个条目后，我们需要实现两个函数来教授 Xcode：

1. 何时显示重构动作。
2. 当用户调用此重构操作时，应该应用什么代码更改。

两个声明都是从上述的条目自动生成的。要实现（1），我们需要在 [Refactoring.cpp](https://github.com/apple/swift/blob/60a91bb7360dde5ce9531889e0ed10a2edbc961a/lib/IDE/Refactoring.cpp) 中实现 `RefactoringActionLocalizeString` 的 [isApplicable](isApplicable) 函数，如下所示：

>```c++
>bool RefactoringActionLocalizeString::
>isApplicable(ResolvedCursorInfo CursorInfo) {
>  if (CursorInfo.Kind == CursorInfoKind::ExprStart) {
>    if (auto *Literal = dyn_cast<StringLiteralExpr>(CursorInfo.TrailingExpr) {
>      return !Literal->hasInterpolation(); // 非现实的 API。
>    }
>  }
>}
>```

使用 [ResolvedCursorInfo](ResolvedCursorInfo) 对象作为输入，检查何时使用 "本地化字符串" 填充可用的重构菜单几乎是琐碎无用的。在这种情况下，只要检查光标是否指向表达式的开始（第 3 行），并且表达式是没有被插入文字（第 5 行）的字符串文字（第 4 行）就足够了。

接下来，我们需要实现如果重构动作再被应用时，应该如何更改光标处的代码。为此，我们必须实现 `RefactoringActionLocalizeString` 的 [performChange](https://github.com/apple/swift/blob/60a91bb7360dde5ce9531889e0ed10a2edbc961a/lib/IDE/Refactoring.cpp#L599) 方法。在 `performChange` 的实现中，我们可以访问也被 [isApplicable](isApplicable) 接受的同一个 `ResolvedCursorInfo` 对象。

>```c++
>bool RefactoringActionLocalizeString::
>performChange() {
>  EditConsumer.insert(SM, Cursor.TrailingExpr->getStartLoc(), "NSLocalizedString(");
>  EditConsumer.insertAfter(SM, Cursor.TrailingExpr->getEndLoc(), ", comment: \"\")");
>  return false; // 返回 true 如果放弃了代码更改。
>}
>```

仍然使用字符串本地化作为示例，[performChange](https://github.com/apple/swift/blob/60a91bb7360dde5ce9531889e0ed10a2edbc961a/lib/IDE/Refactoring.cpp#L599) 功能实现起来相当简单。在函数体中，我们可以使用 [EditConsumer](https://github.com/apple/swift/blob/60a91bb7360dde5ce9531889e0ed10a2edbc961a/include/swift/IDE/Utils.h#L506)，通过适当的 Foundation API 调用来围绕光标指向的表达式进行文本编辑，如第 3 和第 4 行所示。
