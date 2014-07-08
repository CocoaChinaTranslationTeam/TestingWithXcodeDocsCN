#编写测试类与方法
当你添加一个有测试导航的测试目标到工程里，Xcode会在测试导航里展示出测试类与方法。在测试目标里是包含测试方法的测试类。本章节讲述怎样创建测试类和编写测试方法。


##测试目标，测试包，测试导航
在学习创建测试类前，有必要看看测试导航。它的核心是创建和运行测试工作。

测试导航罗列了测试包里所有的组建，在一个有层次的列表里展示出测试类和测试方法。下面是一个工程的测试导航试图，包含了多个测试目标，展示了测试包、类与方法的嵌套层级。

![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/twx-testnav-bundle_hierarchy_2x.png)

测试包里包含多个测试类，可以把测试类分为多个关联或功能组群。比如，计算器示例工程创建了`BasicFunctionsTests`, `AdvancedFunctionsTests`, 和 `DisplayTests classes`三个类，但他们都属于`Mac_Calc_Tests`测试包。

![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/twx-testnav-three_test_classes_2x.png)

一些测试类型可能会共享某些类型的安装和卸载需求，这样的话把这些测试整合到类里面会更加合理，可以最小化减少每一个测试方法的代码。

##创建测试类
点击加号按钮（+），点击测试导航的添加新测试类选项可以创建新测试类。
![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/twx-testnav-new_test_class_2x.png)

每一个测试类都添加到工程中的 _TestClassName.m_ 文件，名字基于你在配置菜单中键入的测试类名。

![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/twx-test_class_config_sheet_2x.png)



    注意：所有的测试类都是`XCTestCase`的子类，由`XCTest`框架提供。

尽管Xcode会默认的把测试类添加到为工程的测试目标创建的组中，你还是可以自己选择要组织的文件。按下图标准的Xcode添加文件方式：

![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/twx-file_handling_sheet_2x.png)


你可以用相同的方法在工程导航中添加文件。具体的使用方法详见[添加已存在的文件或文件夹](https://developer.apple.com/library/mac/recipes/xcode_help-structure_navigator/articles/Adding_an_Existing_File_or_Folder.html#//apple_ref/doc/uid/TP40009934-CH17)

    注意：当你创建Xcode5或更搞版本的工程，一个测试目标和相关的测试包都会默认的被创建，名字根据工程名获得。比如创建的工程名叫`MyApp`，自动生成的包名会叫`MyAppTests`，测试类命叫做`MyAppTests`,关联在`MyAppTests.m`实例文件中。

##测试类的结构
测试类包含如下的基本结构

    #import <XCTest/XCTest.h>
     
    @interface MyAppTests : XCTestCase
    @end
     
    @implementation MyAppTests
     
    // setUp and tearDown
    - (void)setUp
    {
        [super setUp];
        // Put additional setup code here.
    }
     
    - (void)tearDown
    {
        // Put additional teardown code here.
        [super tearDown];
    }
     
    // test methods
    - (void)testXXXX
    {
        // setup code
        // test logic and XCTest assertions go here.
        // teardown code
    }
    @end

测试类用Objective-C实现。注意实现包含了setup和teardown的基本实例方法；这些方法是必须的。如果类中所有的测试方法都需要相同的代码，你可以定制setUp和tearDown来包含这些代码。你添加的代码在每一个测试方法的之前和之后执行。你可以有选择性的添加定制的设置(+ (void)setUp) 和卸载 teardown (+ (void)tearDown) 方法，他们在类里所有测试方法的之前和之后执行。

##测试执行的流程
当测试执行，XCTest找到所有继承于`XCTestCase`的类，为每一个类运行他们的测试方法。

每一个类，测试开始于setup方法。每一个方法，一个新的类实例被创建，他的setup方法执行。在跑完了测试方法之后，实例卸载方法。类中这样连续重复执行所有测试方法。在最后的测试方法卸载后，Xcode执行类卸载方法，然后开始下一个类。这种序列一直重复直到所有的类的所有测试方法跑完。



