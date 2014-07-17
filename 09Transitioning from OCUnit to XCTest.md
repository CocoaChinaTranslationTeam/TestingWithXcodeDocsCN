# 从OCUnit过渡到XCTest #

`XCTest`是`Xcode5`中新引入的一个测试框架。`XCTest`是上一代现代化测试框架`OCUnit`的重新现代化实现。`XCTest`提供了与`Xcode`更好的集成并且奠定了未来改进`Xcode`测试能力的基础。`XCTest`的许多的功能都类似于之前的`OCUnit`。

## OCUnit和XCTest兼容性 ##
自 Xcode2.1 以来，`OCUnit`都是 Xcode测试的基础，很多现有的项目都是基于`OCUnit`测试。它们可以继续使用，因为 Xcode 5为基于`OCUnit`和`XCTest` 的两种项目提供了等同的功能。
 
将基于`OCUnit`测试的现有项目从 Xcode 5 之前的版本添加进 Xcode 5中时，项目会兼容当前 iOS 和 OS X 版本，同样的，iOS 7之前的版本以及 OS X v10.8 之前的版本也是如此。
 
基于`OCUnit`的测试可以在 iOS 6 和 iOS 7 上运行，所以针对iOS 和 OS X 旧版本的项目仍然能有效地选择使用 `OCUnit`。当你需要把项目从 Xcode 5 回归至早期版本的Xcode，并对应用程序进行维护时，这也不失为一个很好的选择。
 
如果你的项目开发工作主要集中于 iOS 7 或更高版本，或在 OS X v10.8 或更高版本，或两者兼而有之，把项目转为使用`XCTest` 将是一个正确的选择。

>注意：OCUnit 在 Xcode 5.1 中被标记为已过时。

## 从OCUnit过渡至XCTest ##

从 OCUnit 到 XCTest 的转换是一个复杂的操作，包括更新源文件，其中包括测试类和修改项目配置设置。Xcode5 中有一个转换工作流程的助手可帮你实现这些转换，选择 Edit > Refactor > Convert OCUnit  to XCTest。使用该工具来成功地将项目迁移使用 XCTest。
 
OCUnit 到 XCTest 转换工具是基于`target-by-target`的。该工具会检查你选择的目标，并且在执行自动转换后让你知道它们是否可以用XCTest运行。
 
>注意：建议您在进行`OCUnit`到`XCTest`的迁移时，将所有目标放到一个项目中。

更新项目`OCUnit`到`XCTest`：

1.选择 Edit > Refactor > Convert OCUnit to XCTest。

2.点击下一步，进入到下一个工作表。

3.在出现的表单中，选择要转换的测试目标。

![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/twx-select_targets_2x.png)

4.要查看一个特定的目标是否能与 XCTest 转换，请单击其名称。

5.单击下一步按钮。

助手弹出了一个`FileMerge`界面，您可以评估其他来源的变化。
	
![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/twx-conversion-filemerge_2x.png)

左边是更新后的源文件，右边是原始文件。你可以浏览所有可能的变化，并丢弃那些你觉得有问题的代码。

6.如果你对所有的更改都满意，就可以单击保存按钮。

Xcode会把更改写入文件。

当转换完成，运行测试目标，并检查其输出，以确保测试按预期运行。如果出现任何问题，请参阅["Debugging Tests"](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/testing_5_debugging_tests/testing_5_debugging_tests.html#//apple_ref/doc/uid/TP40014132-CH6-SW1)一章来查看帮助。
	

