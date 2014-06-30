# 从OCUnit过渡到XCTest #

`XCTest`是`Xcode5`中新引入的一个测试框架。`XCTest`是上一代现代化测试框架`OCUnit`的重新实现。`XCTest`提供和`Xcode`更好的集成和奠定了未来改进`Xcode`的测试能力的基础。`XCTest`的许多的功能都类似于之前的`OCUnit`。

## OCUnit和XCTest兼容性 ##

因为OCUnit在Xcode2.1以来都是在Xcode的基础上进行测试的，所以这里有太多包含现有OCUnit为基础的测试项目。他们可以继续使用，因为Xcode5的设计为他们提供了基于`OCUnit-`和`XCTest-`的两种项目全功能校验。

从早期的Xcode版本带入的Xcode5 OCUnit为基础的测试现有项目与iOS和OS X中的两个最新版本以及iOS的早于iOS的版本7和OS X早于OS X v10.8版本兼容。

OCUnit为基础的测试可以在iOS6和iOS7上运行，所以仍然能针对旧版本iOS和mac OS X的项目有效的选择使用OCUnit。当你需要把项目从Xcode 5回归至早期版本的Xcode应用程序并进行维护时，这也不失一个很好的选择。

如果您的项目开发工作主要集中在iOS7或更高版本，或在OS X v10.8或更高版本，或两者兼而有之，项目中使用XCTest将是一个正确的选择。

>**注意：**OCUnit在Xcode 5.1中被标记为已过时。

## OCUnit转换为XCTest ##

从OCUnit到XCTest的转换是一个复杂的操作，包括更新源文件，其中包括测试类和修改项目配置设置。Xcode中5有一个转换工作流程的助手来帮你实现这些转换，选择 Edit > Refactor > Convert OCUnit to XCTest。这要求你使用工具XCTest.est来迁移您的项目。

转换OCUnit到XCTest工具是基于`target-by-target`的。该工具会检查你选择的目标，并让你知道他们是否可以与XCTest进行自动转换。

>**注意：**建议您将所有目标在一个项目做OCUnit到XCTest迁移。

更新项目OCUnit到XCTest：

1. 选择 Edit > Refactor > Convert OCUnit to XCTest。
2. 点击下一步，进入到下一个工作表。
3. 在出现的表单中，选择要转换的测试目标。![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/twx-select_targets_2x.png)
4. 要查看一个特定的目标是否能与XCTest转换，请单击其名称。
5. 单击下一步按钮。

	助手弹出了一个`FileMerge`界面，您可以评估其他来源的变化。
![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/twx-conversion-filemerge_2x.png)

更新后的源文件是在左边，旧文件安是在右侧。您可以通过扫描所有可能的变化并丢弃那些你觉得有问题代码。

6.当你认为所有的变化都有效后，就可以单击保存按钮。

Xcode会把更改写入文件。

当转换完成，运行测试目标，并检查其输出，以确保试验按预期运行。如果出现任何问题，请参阅["Debugging Tests"](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/testing_5_debugging_tests/testing_5_debugging_tests.html#//apple_ref/doc/uid/TP40014132-CH6-SW1)一章，以完成测试。
	

