# Swift 软件包管理器的 Manifest 配置文件 API 设计

[阅读原文>>](https://swift.org/blog/swift-package-manager-manifest-api-redesign/)

2017 年 6 月 21 日&nbsp;&nbsp;&nbsp;&nbsp;[Ankit Aggarwal](https://twitter.com/aciidb0mb3r/)

Swift 4 中的软件包管理器包括重新设计的 `Package.swift` manifest 配置文件 API。新 API 更容易使用，并遵循了设计指南。由于 Swift 3 软件包管理器中的目标推理规则经常令人混乱，为了有利于在 manifest 配置文件中明确指定软件包结构的实践做法，我们修改了这些规则，并删除了大部分的推断。

Swift 3 的软件包仍然可以正常工作，因为 Swift 4 中的软件包管理器是向后兼容的。Manifest 配置文件的版本由软件包的*工具版本*选择。工具版本在 manifest 配置文件的第一行中使用特殊注释语法指定：`// swift-tools-version：<specifier>`。若省略此处的特殊注释，软件包将默认为 3.1.0 版工具。

工具版本还确定了用于编译软件包源的默认 Swift 语言版本。现有的 Swift 3 软件包将以 Swift 3 兼容模式进行编译。如果不希望使用默认版本，您可以选择使用 Swift 3 和 Swift 4 manifest 配置文件中的 `swiftLanguageVersions` 属性来设置用于编译该软件包的语言版本。 这意味着可以升级包以使用较新的 manifest 配置文件格式，而不会将其源代码升级到 Swift 4。

## 用 Swift 4 创建一个新的软件包

使用 `init` 子命令来创建一个 Swift 4 的软件包：

>```sh
>$ mkdir mytool && cd mytool
>$ swift package init
>$ swift build
>$ swift test
>```

由以上命令生成的 `Package.swift` manifest 配置文件如下所示。

>```swift
>// swift-tools-version:4.0
>// swift-tools-version 声明了构建这个软件包的最小 Swift 版本。
>
>import PackageDescription
>
>let package = Package(
>    name: "mytool",
>    products: [
>        // 产品（products）定义了由一个软件包产生的可执行文件和库，并使他们对其他软件包可见。
>        .library(
>            name: "mytool",
>            targets: ["mytool"]),
>    ],
>    dependencies: [
>        // 依赖关系（dependencies）声明了这个软件包依赖哪些其他软件包。
>        // .package(url: /* package url */, from: "1.0.0"),
>    ],
>    targets: [
>        // 目标（targets）是一个软件包中最基本的构建区块。一个目标定义了一个模块或者测试套件。
>        // 目标可以依赖这个软件包中其他的目标，以及这个软件包所依赖的其他软件包中的产品。
>        .target(
>            name: "mytool",
>            dependencies: []),
>        .testTarget(
>            name: "mytoolTests",
>            dependencies: ["mytool"]),
>    ]
>)>
>```

上述 Swift 4 manifest 配置文件与以前的清单格式有三个主要区别：

1. 4.0 版本的工具使用 `// swift-tools-version:4.0` 来指定。
2. 必须显式声明所有目标及其依赖关系。
3. 公共目标被作为使用了新的产品 API 的产品。Swift 4 软件包中的目标可以取决于其他软件包中的产品或相同软件包中的目标。

## 自定义的目标布局

新的 manifest 配置文件支持软件包的自定义布局。软件包不再需要遵循复杂的、基于惯例的布局规则。而是只有一条规则：如果未提供目标的路径，将会(按顺序)搜索目录 `Sources`、`Source`、`src`、`srcs` 和 `Tests` 以查找目标。

自定义布局使 C 语言库对接到 Swift Package Manager 更容易。以下是服务器端 Swift 社区中使用的两个 C 语言库的 manifest 配置文件：

#### [LibYAML](https://github.com/yaml/libyaml)

>```
>Copyright (c) 2006-2016 Kirill Simonov, licensed under MIT license (https://github.com/yaml/libyaml/blob/master/LICENSE)
>```

>```swift
>// swift-tools-version:4.0
>
>import PackageDescription
>
>let packages = Package(
>    name: "LibYAML",
>    products: [
>        .library(
>            name: "libyaml",
>            targets: ["libyaml"]),
>    ],
>    targets: [
>        .target(
>            name: "libyaml",
>            path: ".",
>            sources: ["src"])
>    ]
>)
>```

#### [Node.js http-parser](https://github.com/nodejs/http-parser)

>```
>Copyright by Authors (https://github.com/nodejs/http-parser/blob/master/AUTHORS), licensed under MIT license (https://github.com/nodejs/http-parser/blob/master/LICENSE-MIT)
>```

>```swift
>// swift-tools-version:4.0
>
>import PackageDescription
>
>let packages = Package(
>    name: "http-parser",
>    products: [
>        .library(
>            name: "httpparser",
>            targets: ["http-parser"]),
>    ],
>    targets: [
>        .target(
>            name: "http-parser",
>            publicHeaders: ".",
>            sources: ["http_parser.c"])
>    ]
>)
>```
