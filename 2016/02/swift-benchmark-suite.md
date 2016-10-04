# Swift 基准评测套件现已可用

2016 年 2 月 8 日&nbsp;&nbsp;&nbsp;&nbsp;[Luke Larson](https://github.com/lplarson/)

Apple 的 Swift 团队很高兴地宣布 Swift 的[基准评测套件](https://github.com/apple/swift/tree/master/benchmark)现已开源。

此套件包括基准评测、库及工具的源代码，其设计目的是为了帮助在项目代码提交之前跟踪 Swift 性能以及性能衰退问题，其中包括：

* 75 项涵盖了很多重要 Swift 工作量的基准评测
* 提供了经常需要用到的基准评测功能的库
* 一个用于运行基准评测和显示性能指标的驱动程序
* 一个用于比较 Swift 不同版本之间基准评测指标的工具

我们期待着与 Swift 社区一起努力，让 Swift 变得尽可能得迅速！

## 构建并运行基准评测

为了能发现潜在的性能衰退，我们鼓励 Swift 项目的贡献者在拉取请求之前对于他们即将提交的代码进行 Swift 基准评测。构建和运行 Swift 基准评测的说明现在已经可以在 [swift/benchmark/README.md](https://github.com/apple/swift/tree/master/benchmark) 中找到。

在将来，我们还计划向 Swift 持续集成系统中添加支持，以在拉取请求的时候运行基准评测。

## 贡献基准测试及改进

欢迎对 Swift 的基准评测套件作出贡献！我们也鼓励全新的包含很多关键工作量的基准评测拉取请求，或是对于基准评测的帮助库的添加，以及其他各种改进。请注意，Swift 的基准评测套件与 Swift 项目共享[许可证](https://github.com/apple/swift/blob/master/LICENSE.txt)，所以我们并不能接受来自由其他许可证涵盖的基准评测的 Swift 移植。更多关于此套件和添加基准评测的信息可从 [swift/benchmark/README.md](https://github.com/apple/swift/tree/master/benchmark) 中找到。
