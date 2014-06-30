# 命令行测试 #

使用`Xcode`的命令行工具,可以编写自动化脚本来构建和测试您的项目。使用此功能充分利用了现有的构建自动化系统的优势。

## 使用xcodebuild运行测试 ##

`xcodebuild`命令行工具驱动测试就像`OS X Server`中的`Xcode IDE`和`Xcode`服务。用`BuildAction`运行`xcodebuil`测试。用`-destination`参数指定不同的测试目的。例如，要在本地`OS X`"My Mac 64 Bit"测试`MyApp`，指定目标和架构时用这个命令：

	> xcodebuild test -project MyAppProject.xcodeproj -scheme MyApp -destination 'platform=OS X,arch=x86_64'

如果你有`development-enabled`设备驱动插件,你按名称或id调用他们。例如，如果有一个名为"Development iPod touch"通过连接测试你的代码，可以使用下面的命令：
	
	> xcodebuild test -project MyAppProject.xcodeproj -scheme MyApp -destination 'platform=iOS,name=Development iPod touch'

测试也可以在`iPhone`模拟器上运行。使用模拟器可以轻松地应对不同的外形和操作系统的版本。例如：
	
	> xcodebuild test -project MyAppProject.xcodeproj -scheme MyApp -destination 'platform=iOS Simulator,name=iPhone,0S=7.0'

 `-destination`参数可以链接在一起，让你只需发出一个命令，就可以跨越目标进行指定的集成共享方案。例如,下面的命令把之前的三个例子合并到一个命令中:

	> xcodebuild test -project MyAppProject.xcodeproj -scheme MyApp
	-destination 'platform=OS X,arch=x86_64'
	-destination 'platform=iOS,name=Development iPod touch'
	-destination 'platform=iOS Simulator,name=iPhone,0S=7.0'

如果测试失败，xcodebuild将返回一个非零的退出代码。

这些都是你所需要了解的命令行运行测试的要领。有关`xcodebuild`更详细的信息，请在终端的应用程序窗口中使用`man`命令进行查找。