#快速开始

本文的目的在于让测试成为你软件开发的重要组成部分，并使测试更方便并易于使用。

##Test Navigator 测试导航栏
测试时我们会频繁使用Xcode5的测试导航栏。

测试导航是Xcode工作区的一部分，被设计用来方便的创建、管理、运行和审核测试功能。点击导航的选择栏，在问题导航和调试导航栏的中间那个就是测试导航。当你的工程定义了一组测试功能，你会在导航栏看到如下图所示：

![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/twx-test_nav-overall_2x.png)

上面的测试导航展示了一个样板工程中的测试包、类和方法的分级表。这个工程是一个计算器应用。计算器引擎实现为一个框架包。你可以`SampleCalcTests`测试包的分级的顶部看到应用中的测试代码。

>注意: Xcode的目标生成产品。Xcode的测试目标生成测试包并展示在测试导航栏中。

>如果你的测试使用存储数据文件、图片，和其他的类型，则可以把它们添加到测试包中，并使用`NSBundle`的API在运行时访问。和测试类一同使用`+[NSBundle bundleForClass:]`来保证测试类从包中取得正确的数据。更多的信息可见[NSBundle Class Reference](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSBundle_Class/Reference/Reference.html#//apple_ref/doc/uid/TP40003624).

>Xcode schemes 控制那些编译的内容。Schemes也可以控制可用的测试方法来执行测试操作。你可以在测试导航面板列表中通过Control+单击项目来启动或关闭测试包、类和方法，或者从快捷菜单中启用或者关闭测试，也可以在scheme中启用或者关闭测试。
        
        
     
此视图中的激活的测试包是`SampleCalcTests`。`SampleCalcTests`包括了一个测试类，总共有9个测试方法。
当你按住表中任何一个项目的箭头，运行按钮![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/twx-test_navigator_run_button_2x.png)
会展示右边的项目名。这是比较快捷的方式运行包里所有的测试或者任何独立的测试。测试返回通过或失败结果给Xcode。当测试被执行，标识会更新从而向你展示结果，绿色的对勾标记是通过，红色的X为失败。在下面的测试导航面板中，两个测试被判定为失败。

![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/twx-test_nav-indicators_2x.png)  


点击列表中的任意测试类或测试方法都会在源码编辑器中打开测试类。测试类和方法标记在源码编辑器的侧栏中，和标记放在一起，和在测试导航面板中的工作方式相同。测试失败在源码编辑器中相关的断言处展示结果字符串。

测试导航面板底部是添加按钮 (+) ，还有一个过滤控制器。你可以缩小范围，比如只在活跃的scheme中测试或者只测试失败的测试，也可以通过名称筛选测试。

更多测试导航详细信息可见[Test Navigator Help](https://developer.apple.com/library/mac/recipes/xcode_help-test_navigator/_index.html#//apple_ref/doc/uid/TP40013329)。



##给你的应用添加测试

在Xcode5中创建新的应用和框架/库类会预配置一个测试目标。当你打开新工程，在测试导航面板上可以看到一个测试包、一个测试类和一个测试方法的模板。但是如果打开一个比较老的版本的Xcode的工程就不会有测试目标了。下面的工作流程展示了一个假定没有集成测试目标的工程。


###创建测试目标
打开测试目标，点击左下角的（+）按钮，从菜单中选择New Test Target。
![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/twx-add_testing_01_2x.png)

根据你的设置偏好和需求，在新目标助手中编辑Product Name和其他参数。

![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/twx-add_testing_02_2x.png)

点击完成按钮来添加目标，测试导航面板中包含了模板测试类和一个测试方法。
![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/twx-add_testing_03_2x.png)

###运行测试并查看结果
现在你已经把测试加到了工程中，你会想要运行这些测试来做一些有用的事。但首先要在测试导航面板中把鼠标悬停在`SampleCalcTests`测试类上并点击运行按钮来运行所有的测试方法，在测试导航面板中抛出了一个错误来。点击`testExample`方法可以看到下面图中的测试结果、源代码和高亮的错误情况:

![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/twx-add_testing_04_2x.png)



测试失败的原因是测试的模板类包含了一个默认的测试方法`XCTFail()`，一个强制判为失败的断言。

###编辑测试代码然后继续运行

因为这是一个计算器应用的样板工程，你可能要在最开始的时候检查它是否能正确执行加法。因为测试编译到app项目里面，不管多复杂，你都可以按照你的需求添加所有的上下文和其他所需信息来执行测试。

插入下方的#import和实例变量声明到`SampleCalcTests.m`文件中     



    #import <XCTest/XCTest.h>
    //
    // Import the application specific header files
    #import "CalcViewController.h"
    #import "CalcAppDelegate.h"
 
    @interface CalcTests : XCTestCase {
    // add instance variables to the CalcTests class
    @private
        NSApplication       *app;
        CalcAppDelegate     *appDelegate;
        CalcViewController  *calcViewController;
        NSView              *calcView;
    }
    @end




给测试方法一个描述性名称，比如`testAddition`，然后添加到implementation资源中。





    - (void) testAddition
    {
       // obtain the app variables for test access
       app                  = [NSApplication sharedApplication];
       calcViewController   = (CalcViewController*)[[NSApplication sharedApplication] delegate];
       calcView             = calcViewController.view;
 
       // perform two addition tests
       [calcViewController press:[calcView viewWithTag: 6]];  // 6
       [calcViewController press:[calcView viewWithTag:13]];  // +
       [calcViewController press:[calcView viewWithTag: 2]];  // 2
       [calcViewController press:[calcView viewWithTag:12]];  // =
        XCTAssertEqualObjects([calcViewController.displayField stringValue], @"8", @"Part 1 failed.");
 
       [calcViewController press:[calcView viewWithTag:13]];  // +
       [calcViewController press:[calcView viewWithTag: 2]];  // 2
       [calcViewController press:[calcView viewWithTag:12]];  // =
        XCTAssertEqualObjects([calcViewController.displayField stringValue], @"10", @"Part 2 failed.");
    }





现在，点击运行按钮运行`testAddition`方法。

![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/twx-add_testing_05_2x.png)

>**注意：**测试导航的列表的改变，反应了`testExample`已经被替换为`testAddition`。

这里仍有错误，看下面的代码，第一部分成功，第二部分有问题。仔细观察，错误很明显:字符引用"11"多了1。改为“10”就能测试成功了。


![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/twx-add_testing_06_2x.png)



###相同的通用代码使用setUp() 和 tearDown()方法

Xcode运行测试方法一次测试所有测试包中的测试类。在这个小例子里只有一个测试方法被实现了，这里需要访问三个计算器应用对象变量的功能。如果在该类中写了4个或5个测试方法，你会发现你在每一次测试方法获得应用的对象状态中，重复了相同的代码。XCTest框架为测试类提供了实例方法`setUp` 和 `tearDown`,你可以用它在每个测试方法运行之前或者之后运行通用代码的调用。


setUp和tearDown很简单。在`testAddition`资源里`Mac_Calc_Tests.m`，在第四行用// obtain the app variable for test access 开头，并将它们粘贴到模板提供的默认的`setUp`实例方法中。

    - (void)setUp
    {
        [super setUp];
        // Put setup code here. This method is called before the invocation of each test method in the class.
 
       // obtain the app variables for test access
       app                  = [NSApplication sharedApplication];
       calcViewController   = (CalcViewController*)[[NSApplication sharedApplication] delegate];
       calcView             = calcViewController.view;
    }

现在使用少的重复代码添加第二个测试方法：`testMultiplication`以及其他。

![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/twx-add_testing_07_2x.png)


##总结
当你看完这篇简短的快速开始，向工程添加测试很简单。这里有几个要注意的地方:

*  Xcode配置好了大部分的基本设置。当你需要添加一个测试目标，Xcode创建测试包文件，添加测试包到测试导航中，添加XCTest框架到工程里，并且提供了测试模板类和方法。
*  测试导航可以很方便的定位和编辑测试方法。你可以使用导航里的指示按钮直接运行测试，或在测试类实现打开时直接在资源编辑器中运行。当测试失败，测试导航中的标识跟错误的地方相对应。
*  一个测试方法包括多个断言，结果为一个通过或失败。创建的简单或复杂跟你的项目需要而定。
*  setup和tearDown实例方法提供给你一个多次测试相同代码的方式，更加的合理和易于调试。
























