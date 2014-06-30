# 从OCUnit过渡到XCTest #

`XCTest`是`Xcode5`中新引入的一个测试框架。`XCTest`是上一代现代化测试框架`OCUnit`的重新现代化实现。`XCTest`提供了与`Xcode`更好的集成并且奠定了未来改进`Xcode`测试能力的基础。`XCTest`的许多的功能都类似于之前的`OCUnit`。

## OCUnit和XCTest兼容性 ##

因为 OCUnit 自 Xcode2.1 以来都是在 Xcode 的基础上进行测试的，所以有很多现有的项目都是基于 OCUnit 测试。他们可以继续使用，因为 Xcode5 的设计为他们提供了基于`OCUnit`和`XCTest`的两种项目全功能校验。

从 Xcode 5 之前的版本将基于 OCUnit 测试的现有项目添加进 Xcode 5中时，项目会兼容当前 ios 和 OS X 版本，同样的，ios 7 之前的版本以及 OSX v10.8 之前的版本也是。

基于 OCUnit 的测试可以在 iOS6 和 iOS7 上运行，所以针对旧版本 iOS 和 mac OS X 的项目仍然能有效的选择使用 OCUnit。当你需要把项目从 Xcode 5 回归至早期版本的 Xcode 应用程序并进行维护时，这也不失为一个很好的选择。

如果您的项目开发工作主要集中在 iOS7 或更高版本，或在 OS X v10.8 或更高版本，或两者兼而有之，项目中使用 XCTest 将是一个正确的选择。

>**注意：**OCUnit 在 Xcode 5.1 中被标记为已过时。

## OCUnit转换为XCTest ##

从 OCUnit 到 XCTest 的转换是一个复杂的操作，包括更新源文件，其中包括测试类和修改项目配置设置。Xcode5 中有一个转换工作流程的助手来帮你实现这些转换，选择 Edit > Refactor > Convert OCUnit to XCTest。使用该工具来成功的将您的项目迁移使用 XCTest。

转换 OCUnit 到 XCTest 工具是基于`target-by-target`的。该工具会检查你选择的目标，并让你知道他们是否可以与 XCTest 进行自动转换。

>**注意：**建议您在进行 OCUnit 到 XCTest 迁移时，将所有目标放到一个项目中。

更新项目 OCUnit 到 XCTest：

1.选择 Edit > Refactor > Convert OCUnit to XCTest。

2.点击下一步，进入到下一个工作表。

3.在出现的表单中，选择要转换的测试目标。

![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/twx-select_targets_2x.png)

4.要查看一个特定的目标是否能与 XCTest 转换，请单击其名称。

5.单击下一步按钮。

助手弹出了一个`FileMerge`界面，您可以评估其他来源的变化。
	
![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/twx-conversion-filemerge_2x.png)

更新后的源文件是在左边，原始文件则是在右侧。您可以通过浏览所有可能的变化并丢弃那些你觉得有问题的代码。

6.当你认为所有的变化都有效后，就可以单击保存按钮。

Xcode会把更改写入文件。

当转换完成，运行测试目标，并检查其输出，以确保试验按预期运行。如果出现任何问题，请参阅["Debugging Tests"](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/testing_5_debugging_tests/testing_5_debugging_tests.html#//apple_ref/doc/uid/TP40014132-CH6-SW1)一章来查看帮助。
	

