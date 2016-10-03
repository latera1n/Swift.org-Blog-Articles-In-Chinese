# 持续集成现已可用

[阅读原文>>](https://swift.org/blog/swift-ci/)

2016 年 2 月 1 日 &nbsp; [Nicole Jacque](https://github.com/najacque/)

我们很高兴地宣布我们已经为 Swift 项目推出了[持续集成](https://swift.org/continuous-integration)（又名 CI）功能！

我们的 CI 系统由 [Jenkins](https://jenkins-ci.org/) 提供支持。在 Apple 平台上，它能为 macOS 和 iOS 模拟器构建并运行测试。而对于 Linux 来说，它能为 Ubuntu 14.04 和 Ubuntu 15.10（均为 x86_64 架构）构建并运行测试。除了使用它来测试活跃分支，CI 系统还能生成可从 Swift.org 下载的快照。

CI 系统的目的是随着时间的推移可以逐步增加更多的配置，特别是在到其他平台和体系结构的移植，以及来自 Swift 开发社区的支持达到一个可观的数量的情况下。

CI 不仅是一个监视 Swift 项目的运行状况的强大工具，也可以被用做一个在项目作出改变之前审查项目变化的工具。为了实现这些功能，我们已经__在测试中集成了拉取请求__，这个功能允许在提交代码之前即完成测试，且不会让 `master` 分支变得不稳定。测试结果结果将被嵌入到拉请求中并公布。当有人做出更改并破坏了版本构建，他们会被自动的电子邮件告知。

## 测试拉取请求

当在审查拉取请求中的更改时，社区中的有代码提交权限的成员可以触发 CI 系统中拉取请求的测试。这些测试可以在 macOS、Linux 平台或在双平台上被触发运行。

![pull request CI trigger](https://swift.org/continuous-integration/images/ci_pull_command.png)

随后测试状态会被嵌入在拉取请求中并发布，并显示测试正在进行中。你可以点击“详细信息”链接直接进入状态页面来查看正在进行中的测试。

![CI Progress](https://swift.org/continuous-integration/images/ci_pending.png)

当测试完成后，结果将在拉取请求中更新。

![CI Pass](https://swift.org/continuous-integration/images/ci_pass.png)

如果在测试过程中发现任何问题，你会得到一个可以查看失败测试详细信息的链接。

![CI Failure](https://swift.org/continuous-integration/images/ci_failure.png)

在不久的将来，我们也将支持运行性能测试。我们也欢迎社区参与，来帮助我们扩展测试到更多配置的情况下。

<br />
<sub>Original article: <https://swift.org/blog/swift-ci/></sub>

<sup>Original article copyright © 2016 Apple Inc. All rights reserved. Swift and the Swift logo are trademarks of Apple Inc.</sup>
