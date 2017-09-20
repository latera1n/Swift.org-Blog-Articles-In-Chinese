# Swift 局部重构

[阅读原文>>](https://swift.org/blog/swift-local-refactoring/)

2017 年 8 月 22 日&nbsp;&nbsp;&nbsp;&nbsp;[Xi Ge](https://twitter.com/xge_apple/)

Xcode 9 包括一个全新的重构引擎。它既可以在局部的单个 Swift 源文件中，又可以在全局的范围内变换代码，例如重命名出现在多个文件甚至不同语言中的方法或属性。局部重构背后的逻辑完全在编译器和 SourceKit 中实现，且已经在 [swift 代码库](https://github.com/apple/swift)中开源。因此，每一个 Swift 爱好者都可以为语言贡献重构的操作。这篇文章讨论了如何在 Xcode 中实现并显示出一个简单的重构操作。

## 重构的种类

**局部重构**发生在单个文件的范围内。局部重构的例子包括提取方法和提取重复表达式。跨多个文件改变代码（如全局重命名）的**全局重构**目前需要 Xcode 的特殊协调，目前无法在 Swift 代码库内自行实现。这篇文章专注于局部重构，因为它们本身也可以是非常强大的。

在编辑器中，重构的动作是由用户光标选择启动的。根据它们是如何初始化的，我们将重构动作分类为基于光标或者基于范围的操作。**基于光标的重构**具有一个由光标位置充分指定的 Swift 源文件中的重构目标，例如重命名重构。相比之下，**基于范围的重构**需要一个开始和结束位置来指定其目标，例如提取方法重构。为了方便实现这两个类别，Swift 代码库提供了预分析结果，称为 [ResolvedCursorInfo](https://github.com/apple/swift/blob/7f29b362d68eb990a592257850aabadb24de61df/include/swift/IDE/Utils.h#L158) 和 [ResolvedRangeInfo](https://github.com/apple/swift/blob/60a91bb7360dde5ce9531889e0ed10a2edbc961a/include/swift/IDE/Utils.h#L344)，以解析有关 Swift 源文件中光标位置或范围的几个常见问题。

例如，[ResolvedCursorInfo](https://github.com/apple/swift/blob/7f29b362d68eb990a592257850aabadb24de61df/include/swift/IDE/Utils.h#L158) 可以告诉我们源文件中的位置是否指向表达式的开头，如果是，则提供该表达式的相应编译器对象。或者，如果光标指向一个名称，[ResolvedCursorInfo](https://github.com/apple/swift/blob/7f29b362d68eb990a592257850aabadb24de61df/include/swift/IDE/Utils.h#L158) 将给出与该名称对应的声明。类似地，[ResolvedRangeInfo](https://github.com/apple/swift/blob/60a91bb7360dde5ce9531889e0ed10a2edbc961a/include/swift/IDE/Utils.h#L344) 封装了有关给定源范围的信息，例如该范围是否具有多个入口点或出口点。

要为 Swift 实现一个新的重构，我们不需要从光标或范围位置的原始表示开始；相反地，我们可以从 [ResolvedCursorInfo](https://github.com/apple/swift/blob/7f29b362d68eb990a592257850aabadb24de61df/include/swift/IDE/Utils.h#L158) 和 [ResolvedRangeInfo](https://github.com/apple/swift/blob/60a91bb7360dde5ce9531889e0ed10a2edbc961a/include/swift/IDE/Utils.h#L344) 开始，使用它们即可导出重特定的分析。

## 基于光标的重构

![基于光标的重构](https://swift.org/assets/images/local-refactoring/Cursor.png)

基于光标的重构由 Swift 源文件中的光标位置启动。重构操作实现了被重构引擎使用的方法，即在 IDE 上显示可用操作并执行变换的过程。

具体来说，为了要显示出可用的操作：

1. 用户在 Xcode 编辑器中选择一个位置。
2. Xcode 向 [sourcekitd](https://github.com/apple/swift/tree/master/tools/SourceKit) 发出一个请求，来查看该位置存在的可用操作。
3. 对每个实现的重构操作使用一个 `ResolvedCursorInfo` 对象进行查询，以查看该操作是否适用于该位置。
4. 适用的操作列表作为来自 [sourcekitd](https://github.com/apple/swift/tree/master/tools/SourceKit) 的响应被返回，并由 Xcode 呈现给用户。

当用户选择其中一个可用的操作时：

1. Xcode 向 [sourcekitd](https://github.com/apple/swift/tree/master/tools/SourceKit) 发出一个请求，以在源位置处执行选定的操作。
2. 对特定重构操作使用源自同一位置的一个 `ResolvedCursorInfo` 对象进行查询，以验证该操作是否适用。
3. 重构操作被请求使用文本源代码编辑执行变换操作。
4. 对源代码的编辑作为来自 [sourcekitd](https://github.com/apple/swift/tree/master/tools/SourceKit) 的响应被返回，并由 Xcode 编辑器应用。

要实现*字符串本地化*重构，我们需要先在 [RefactoringKinds.def](https://github.com/apple/swift/blob/60a91bb7360dde5ce9531889e0ed10a2edbc961a/include/swift/IDE/RefactoringKinds.def) 文件中声明这个重构，包括如下条目：

>```c++
> CURSOR_REFACTORING(LocalizeString, "Localize String", localize.string)
>```

`CURSOR_REFACTORING` 指定了这个重构操作是在光标位置初始化的，由此在实现中将可以使用 [ResolvedCursorInfo](https://github.com/apple/swift/blob/7f29b362d68eb990a592257850aabadb24de61df/include/swift/IDE/Utils.h#L158)。第一个字段 `LocalizeString` 在 Swift 代码库中指定了这个重构操作的内部名称。在此示例中，与此重构相对应的类的名称为 `RefactoringActionLocalizeString`。字符串文字 `"Localize String"` 是在 UI 中向用户显示的这个重构操作的显示名称。最后，“localize.string” 是一个稳定的，用于表示重构操作的键名，Swift 工具链使用它与源代码编辑器通信。此条目还允许 C++ 编译器为 String 局部重构及其调用者生成类的存根。由此，我们可以专注于实现所需的功能。

在指定这个条目后，我们需要实现两个函数来教授 Xcode：

1. 何时显示重构动作。
2. 当用户调用此重构操作时，应该应用什么代码更改。

两个声明都是从上述的条目自动生成的。要实现（1），我们需要在 [Refactoring.cpp](https://github.com/apple/swift/blob/60a91bb7360dde5ce9531889e0ed10a2edbc961a/lib/IDE/Refactoring.cpp) 中实现 `RefactoringActionLocalizeString` 的 [isApplicable](https://github.com/apple/swift/blob/60a91bb7360dde5ce9531889e0ed10a2edbc961a/lib/IDE/Refactoring.cpp#L646) 函数，如下所示：

>```c++
> bool RefactoringActionLocalizeString::
> isApplicable(ResolvedCursorInfo CursorInfo) {
>   if (CursorInfo.Kind == CursorInfoKind::ExprStart) {
>     if (auto *Literal = dyn_cast<StringLiteralExpr>(CursorInfo.TrailingExpr) {
>       return !Literal->hasInterpolation(); // 非真实 API。
>     }
>   }
> }
>```

使用 [ResolvedCursorInfo](https://github.com/apple/swift/blob/7f29b362d68eb990a592257850aabadb24de61df/include/swift/IDE/Utils.h#L158) 对象作为输入，检查何时使用“本地化字符串”填充可用的重构菜单几乎是琐碎无用的。在这种情况下，只要检查光标是否指向表达式的开始（第 3 行），并且表达式是没有被插值（第 5 行）的字符串文字（第 4 行）就足够了。

接下来，我们需要实现如果重构动作被应用时，如何更改光标处的代码。为此，我们必须实现 `RefactoringActionLocalizeString` 中的 [performChange](https://github.com/apple/swift/blob/60a91bb7360dde5ce9531889e0ed10a2edbc961a/lib/IDE/Refactoring.cpp#L599) 方法。在 `performChange` 的实现中，我们可以访问同样被 [isApplicable](https://github.com/apple/swift/blob/60a91bb7360dde5ce9531889e0ed10a2edbc961a/lib/IDE/Refactoring.cpp#L646) 接受的同一个 `ResolvedCursorInfo` 对象。

>```c++
> bool RefactoringActionLocalizeString::
> performChange() {
>   EditConsumer.insert(SM, Cursor.TrailingExpr->getStartLoc(), "NSLocalizedString(");
>   EditConsumer.insertAfter(SM, Cursor.TrailingExpr->getEndLoc(), ", comment: \"\")");
>   return false; // 如果放弃了代码更改则返回 true。
> }
>```

这里仍然使用字符串本地化作为例子，[performChange](https://github.com/apple/swift/blob/60a91bb7360dde5ce9531889e0ed10a2edbc961a/lib/IDE/Refactoring.cpp#L599) 功能实现起来相当简单。在函数体中，我们可以使用 [EditConsumer](https://github.com/apple/swift/blob/60a91bb7360dde5ce9531889e0ed10a2edbc961a/include/swift/IDE/Utils.h#L506)，通过适当的 Foundation API 调用来围绕光标指向的表达式进行文本编辑，如第 3 和第 4 行所示。

## 基于范围的重构

![基于位置的重构](https://swift.org/assets/images/local-refactoring/Range.png)

如上图所示，通过在 Swift 源文件中选择连续的代码范围来启动基于范围的重构。以*提取表达式*重构的实现为例，我们首先需要在 [RefactoringKinds.def](https://github.com/apple/swift/blob/60a91bb7360dde5ce9531889e0ed10a2edbc961a/include/swift/IDE/RefactoringKinds.def) 中声明以下项目。

>```c++
> RANGE_REFACTORING(ExtractExpr, "Extract Expression", extract.expr)
>```

此条目声明提取表达式重构由范围选择启动，范围选择以 `ExtractExpr` 内部命名，使用 `"Extract Expression"` 作为显示名称，并使用 “extract.expr” 作为稳定的键名用于服务通信的目的。

为了告诉 Xcode 在什么时候这个重构应该是可用的，我们还需要在 [Refactoring.cpp](https://github.com/apple/swift/blob/60a91bb7360dde5ce9531889e0ed10a2edbc961a/lib/IDE/Refactoring.cpp) 中实现对于这个重构的 [isApplicable](https://github.com/apple/swift/blob/60a91bb7360dde5ce9531889e0ed10a2edbc961a/lib/IDE/Refactoring.cpp#L646)，稍微不同的是，这里的输入是 [ResolvedRangeInfo](https://github.com/apple/swift/blob/60a91bb7360dde5ce9531889e0ed10a2edbc961a/include/swift/IDE/Utils.h#L344) 而不是 [ResolvedCursorInfo](https://github.com/apple/swift/blob/7f29b362d68eb990a592257850aabadb24de61df/include/swift/IDE/Utils.h#L158)。

>```c++
> bool RefactoringActionExtractExpr::
> isApplicable(ResolvedRangeInfo Info) {
>   if (Info.Kind != RangeKind::SingleExpression)
>     return false;
>   auto Ty = Info.getType();
>   if (Ty.isNull() || Ty.hasError())
>     return false;
>   ...
>   return true;
> }
>```

虽然这里比上述字符串本地化重构中的对应例子复杂一些，但这种实现也是能够自我解释的。第 3 至第 4 行检查给定范围的种类，其必须是单个表达式才能进行提取。第 5 至 7 行确保提取的表达式是符合语法规则形式的。现在我们暂时忽略例子中的需要检查的其他条件，有兴趣的读者可以参考 [Refactoring.cpp](https://github.com/apple/swift/blob/60a91bb7360dde5ce9531889e0ed10a2edbc961a/lib/IDE/Refactoring.cpp) 了解更多细节。对于代码更改的部分，我们可以使用相同的 [ResolvedRangeInfo](https://github.com/apple/swift/blob/60a91bb7360dde5ce9531889e0ed10a2edbc961a/include/swift/IDE/Utils.h#L344) 实例来发出文本编辑操作：

>```c++
> bool RefactoringActionExtractExprBase::performChange() {
>   llvm::SmallString<64> DeclBuffer;
>   llvm::raw_svector_ostream OS(DeclBuffer);
>   OS << tok::kw_let << " ";
>   OS << PreferredName;
>   OS << TyBuffer.str() <<  " = " << RangeInfo.ContentRange.str() << "\n";
>   Expr *E = RangeInfo.ContainedNodes[0].get<Expr*>();
>   EditConsumer.insert(SM, InsertLoc, DeclBuffer.str());
>   EditConsumer.insert(SM,
>                       Lexer::getCharSourceRangeFromSourceRange(SM, E->getSourceRange()),
>                       PreferredName)
>   return false; // 如果放弃了代码更改则返回 true。
> }
>```

第 2 至第 6 行构建了一个局部变量的声明，和该提取操作下的表达式的初始化值，例如 `let extractExpr = foo()`。第 8 行将声明插入本地上下文中的正确的源位置，第 9 行使用新变量表达式的引用替换了在原始位置出现的引用。如代码示例所示，在 [performChange](https://github.com/apple/swift/blob/60a91bb7360dde5ce9531889e0ed10a2edbc961a/lib/IDE/Refactoring.cpp#L599) 的函数体内，我们不仅可以访问用户选择的原始 [ResolvedRangeInfo](https://github.com/apple/swift/blob/60a91bb7360dde5ce9531889e0ed10a2edbc961a/include/swift/IDE/Utils.h#L344)，还可以访问其他重要的实用程序，比如编辑的消费者和源代理管理器，使实现更加方便。

## 诊断

由于各种原因，自动代码更改期间可能需要中止重构操作。当这种情况发生时，重构实现可以通过诊断将这种故障的原因传达给用户。重构诊断使用与编译器本身相同的机制，以重命名重构为例，如果给定的新名称是无效的 Swift 标识符，我们将发出一条错误消息。为了实现这个诊断，我们首先需要在 [DiagnosticsRefactoring.def](https://github.com/apple/swift/blob/60a91bb7360dde5ce9531889e0ed10a2edbc961a/include/swift/AST/DiagnosticsRefactoring.def) 中为诊断声明以下条目。

>```c++
> ERROR(invalid_name, none, "'%0' is not a valid name", (StringRef))
>```

在声明之后，我们可以使用 [isApplicable](https://github.com/apple/swift/blob/60a91bb7360dde5ce9531889e0ed10a2edbc961a/lib/IDE/Refactoring.cpp#L646) 或 [performChange](https://github.com/apple/swift/blob/60a91bb7360dde5ce9531889e0ed10a2edbc961a/lib/IDE/Refactoring.cpp#L599) 中的诊断。对于*本地重命名*重构，在 [Refactoring.cpp](https://github.com/apple/swift/blob/60a91bb7360dde5ce9531889e0ed10a2edbc961a/lib/IDE/Refactoring.cpp) 中发出诊断将类似这样：

>```c++
> bool RefactoringActionLocalRename::performChange() {
> ...
>   if (!DeclNameViewer(PreferredName).isValid()) {
>     DiagEngine.diagnose(SourceLoc(), diag::invalid_name, PreferredName);
>     return true; // 如果放弃了代码更改则返回 true。
>   }
> ...
> }
>```

## 测试

对应于实现新的重构动作的两个步骤，我们需要测试：

1. 上下文可用的重构被正确地填充。
2. 自动代码更改正确地更新了用户的代码库。

这两个部分都使用与编译器一起构建的 [swift-refactor](https://github.com/apple/swift/tree/60a91bb7360dde5ce9531889e0ed10a2edbc961a/tools/swift-refactor) 命令行实用程序进行测试。

### 上下文重构测试

>```c++
> func foo() {
>   print("Hello World!")
> }
> // 运行: %refactor -source-filename %s -pos=2:14 | %FileCheck %s -check-prefix=CHECK-LOCALIZE-STRING
> // CHECK-LOCALIZE-STRING: Localize String
>```

我们再来以字符串本地化为例。上述代码片段是上下文重构操作的测试。类似的测试可以在 [test/refactoring/RefactoringKind/](https://github.com/apple/swift/tree/master/test/refactoring/RefactoringKind) 中找到。

我们更详细地看一下`运行`的那一行，从 `％refactor` 实用程序的使用开始：

>```bash
> %refactor -source-filename %s -pos=2:14 | %FileCheck %s -check-prefix=CHECK-LOCALIZE-STRING
>```

当用户将光标指向字符串文字 “Hello World!” 时，此行将转储所有适用于重构的显示名称。`％refactor` 是一个别名，由测试运算符代替，以便在测试运行时给出 `swift-refactor` 的完整路径。`-pos` 给出了应该从中提取上下文重构动作的光标位置。由于 `String Localization` 是基于光标的，所以仅仅指定 `-pos` 就足够了。若要测试基于范围的重构，我们还需要指定 `-end-pos` 来指示重构目标的最终位置。所有位置表示的格式为 `行:列`。

为了确保这个工具的输出是按照预期的，我们使用 `％FileCheck` 实用程序：

>```bash
> %FileCheck %s -check-prefix=CHECK-LOCALIZE-STRING
>```

这将对于后面的带有前缀为 `CHECK-LOCALIZE-STRING` 的每一行检查由 `％refactor` 输出的文本。在这种情况下，它将检查可用的重构是否包括 `Localize String`。除了测试我们是否在正确的光标位置显示了正确的操作之外，我们还需要测试确保可用的重构没有被错误地填充，比如在字符串文字插值的情况下。

### 代码变换测试

我们还应该测试当应用重构时，自动代码更改是否符合我们的预期。作为准备工作，我们需要教授 [swift-refactor](https://github.com/apple/swift/tree/60a91bb7360dde5ce9531889e0ed10a2edbc961a/tools/swift-refactor) 一个重构种类的标识来指定我们正在测试的动作。为了实现这一点，以下条目已经被添加到了[swift-refactor.cpp](https://github.com/apple/swift/blob/master/tools/swift-refactor/swift-refactor.cpp) 中：

>```c++
> clEnumValN(RefactoringKind::LocalizeString, "localize-string", "Perform String Localization refactoring"),
>```

有了这样一个条目以后，[swift-refactor](https://github.com/apple/swift/tree/60a91bb7360dde5ce9531889e0ed10a2edbc961a/tools/swift-refactor) 便可以专门测试 String Localization 的代码变换部分。典型的代码变换测试由两部分组成：

1. 重构前的代码段。
2. 变换后的预期输出。

测试在（1）中执行指定的重构，并将结果与（2）进行比较。两者相同则通过，否则测试失败。

>```c++
> func foo() {
>   print("Hello World!")
> }
> // 运行: rm -rf %t.result && mkdir -p %t.result
> // 运行: %refactor -localize-string -source-filename %s -pos=2:14 > %t.result/localized.swift
> // 运行: diff -u %S/Iutputs/localized.swift.expected %t.result/localized.swift
>```

>```c++
> func foo() {
>   print(NSLocalizedString("Hello World!", comment: ""))
> }
>```

上述两个代码片段包括有意义的代码变换测试。第 4 行为重构产生的代码准备了一个临时源目录。使用新添加的 `-localize-string`，第 5 行在 `"Hello World!"` 的起始位置执行重构代码更改，并将结果转储到临时目录。最后，第 6 行将结果与第二个代码示例中所示的预期输出进行比较。

## 与 Xcode 集成

在 Swift 代码库中实现上述所有部分之后，我们便准备好通过与本地构建的开源工具链的集成，以在 Xcode 中测试/使用新添加的重构。

1. 运行 [build-toolchain](https://github.com/apple/swift/blob/master/utils/build-toolchain) 以在本地构建开源工具链。

2. 解压并将工具链复制到 `/Library/Developer/Toolchains/`。

3. 通过 `Xcode->Toolchains` 指定 Xcode 使用的本地工具链，如下图所示。

![指定工具链](https://swift.org/assets/images/local-refactoring/Toolchain.png)

## 潜在的本地化重构的想法

这篇文章只是简单介绍了现在可以在新的重构引擎中实现的一些事情。如果您对于扩展重构引擎以实现其他变换感到兴奋，Swift 的[问题数据库](https://bugs.swift.org/)包含着几个等待实现的[重构变换的想法](https://bugs.swift.org/issues/?jql=labels%3DStarterProposal%20AND%20labels%3DRefactoring%20AND%20resolution%3DUnresolved)。如果您想要提出新的重构想法，只要在 Swift 的[问题数据库](https://bugs.swift.org/)中使用标签 `Refactoring` 提交任务就可以了。

有关实施重构变换的进一步帮助，请参阅[文档](https://github.com/apple/swift/blob/master/docs/refactoring/SwiftLocalRefactoring.md)或请随时在 [swift-dev](https://lists.swift.org/mailman/listinfo/swift-dev) 邮件列表中提出问题。

<br />
<sub>Original article: <a href="https://swift.org/blog/swift-local-refactoring/">https://swift.org/blog/swift-local-refactoring/</a></sub>

<sup>Original article copyright © 2017 Apple Inc. All rights reserved. Swift and the Swift logo are trademarks of Apple Inc.</sup>
