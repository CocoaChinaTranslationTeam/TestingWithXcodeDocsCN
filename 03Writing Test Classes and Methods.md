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


##编写测试方法
你通过测试方法把测试写到测试类中，一个测试方法是_test_开发头的测试类的实例方法，没有参数，返回`void`，比如`testColorIsRed`.测试方法调用工程中的代码，如果得不到预期的结果会用一系列的断言API报错。比如，
一个函数返回值可能与预期值相比不同，或者你的测试方法使用了某个类里不合适的方法都将会抛出异常。[ “XCTest Assertions”](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/testing_3_writing_test_classes/testing_3_writing_test_classes.html#//apple_ref/doc/uid/TP40014132-CH4-SW34)描述了这些情况。

为了测试方法正常的访问，引入正确的头文件到测试类中很重要。

当Xcode运行测试，它调用的每个测试方法都是独立的。因此每个方法需要准备和清理干净附注的变量、结构和对象等，他们会和子类的API相互影响。如果类中所有测试方法的代码是相同的，你可以直接加到`setUp`和`tearDown`的实例方法中，详见[ “Test Class Structure.”](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/testing_3_writing_test_classes/testing_3_writing_test_classes.html#//apple_ref/doc/uid/TP40014132-CH4-SW2)

下面是个测试方法的模型:

     - (void)testColorIsRed {
        ...     // Set up, call test subject API. (Code could be shared in setUp method.)
        ...     // Test logic and values, assertions report pass/fail to testing framework.
        ...     // Tear down. (Code could be shared in tearDown method.
     }



这里有一个简单的测试方法例子，检查`CalcView`实例是否成功的为SampleCalc创建，详见[ “Quick Start” ](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/testing_1_quick_start/testing_1_quick_start.html#//apple_ref/doc/uid/TP40014132-CH2-SW1)章节：



    - (void) testCalcView {
       // setup
       app = [NSApplication sharedApplication];
       calcViewController = (CalcViewController*)[NSApplication sharedApplication] delegate];
       calcView             = calcViewController.view;
     
       XCTAssertNotNil(calcView, @"Cannot find CalcView instance");
       // no teardown needed
    }    
    For further examples of test methods implemented in a project, see the Testing Apps and Frameworkssample code project.


进一步的例子可以在_Testing Apps and Frameworkssample_工程中找到代码


##XCTest断言

测试方法使用的断言呈现在Xcode的测试结果中，由XCTest框架提供。所有的断言有一个相似的格式:项目比较或逻辑表达式，一个失败结果格式的字符串，和插入到字符串中的参数。


    注意：所有断言的最后一个参数是`format...`，格式字符串和变量参数列表。XCTest提供了默认的失败结果字符串，可以将参数传递到断言里。`format`字符串提供了可选的额外的失败说明，那样就可以进一步的选择提供的描述。这个参数是可选的，所以可以完全忽略。
    
    
比如，[Quick Start](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/testing_1_quick_start/testing_1_quick_start.html#//apple_ref/doc/uid/TP40014132-CH2-SW1)里`testAddition`方法中的断言：

     XCTAssertEqualObjects([calcViewController.displayField stringValue], @"10", @"Part 2 failed.");

阅读这段语句，是说：“指出一个错误，当字符串根据视图控制器展示区域创建，不同于参数值‘8’”。如果有失败断言，Xcode在测试导航发出失败信号，然后Xcode会在问题导航、资源编辑器和其他地方标出失败描述。下面是自愿编辑器中典型的断言结果：

![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/twx-test_failure_assert_result_2x.png)

测试类可以包含多个断言，如果任何断言包括失败报告Xcode都发出的测试失败信号。

断言分为五种类型：无条件失败，相等测试，为空测试，布尔测试，和异常测试。

##用类别区分断言

下面的区域列出了XCTest断言。你可以使用`XCTestAssertions.h`来包含更多信息在XCTest断言里面。

###非条件失败
__XCTFail__.生成一个无条件的失败。

     XCTFail(format...)


###相等测试
__XCTAssertEqualObjects__.当_expression1_不等于_expression2_将产生失败（或者一个对象为空，另一个不为空）。

     XCTAssertEqualObjects(expression1, expression2, format...)

__XCTAssertNotEqualObjects__. 当_expression1_等于_expression2_将产生失败

     XCTAssertNotEqualObjects(expression1, expression2, format...)


__XCTAssertEqual__. 当_expression1_不等于_expression2_将产生失败，这个测试用于C语言的标量。

     XCTAssertEqual(expression1, expression2, format...)

__XCTAssertNotEqual__.  当_expression1_等于_expression2_将产生失败，这个测试用于C语言的标量。

     XCTAssertNotEqual(expression1, expression2, format...)


__XCTAssertEqualWithAccuracy__. 当_expression1_不同于_expression2_的部分高于 _精度_ 将产生失败。这种标量用于floats和doubles这些有细微不同但能保证结果基本相等的地方，但是对所有的标量都有效。

     XCTAssertEqualWithAccuracy(expression1, expression2, accuracy, format...)

__XCTAssertNotEqualWithAccuracy__. 当_expression1_不同于_expression2_的部分低于 _精度_ 将产生失败。这种标量用于floats和doubles这些有细微不同但能保证结果基本相等的地方，但是对所有的标量都有效。


     XCTAssertNotEqualWithAccuracy(expression1, expression2, accuracy, format...)


###nil(空)测试

__XCTAssertNil__.  当_expression_参数不为空产生错误。

    XCTAssertNil(expression, format...)

__XCTAssertNotNil__. 当_expression_参数不为空产生错误。

    XCTAssertNotNil(expression, format...)
    
    
###布尔测试


__XCTAssertTrue__. 当_expression_计算为_false_产生错误。

    XCTAssertTrue(expression, format...)

__XCTAssert__. 当_expression_计算为_false_产生错误，与_XCTAssertTrue_相同。

     XCTAssert(expression, format...)
     

__XCTAssertFalse__.  当_expression_计算为_true_产生错误。

     XCTAssertFalse(expression, format...)


###异常测试

__XCTAssertThrows__.当_expression_不抛出异常产生错误。

     XCTAssertThrows(expression, format...)
     
__XCTAssertThrowsSpecific__.当指定的类的_expression_不抛出异常产生错误。

     XCTAssertThrowsSpecific(expression, exception_class, format...)

__XCTAssertThrowsSpecificNamed__. 当_expression_指定的类中指定的名字不抛出异常产生错误。对于AppKit框架或基础框架非常有用，可以用通用的`NSException`抛出指定名字的异常(NSInvalidArgumentException 等)。

    XCTAssertThrowsSpecificNamed(expression, exception_class, exception_name, format...)


__XCTAssertNoThrow__. 当_expression_抛出异常产生错误。

     XCTAssertNoThrow(expression, format...)


__XCTAssertNoThrowSpecific__. 当指定的类的_expression_抛出异常则产生错误。任意其他的异常都可以；就说，他不产生失败。

    XCTAssertNoThrowSpecific(expression, exception_class, format...)


__XCTAssertNoThrowSpecificNamed__. 当_expression_指定的类中指定的名字抛出异常则产生错误。对于AppKit框架或基础框架非常有用，可以用通用的`NSException`抛出指定名字的异常(NSInvalidArgumentException 等)。


     XCTAssertNoThrowSpecificNamed(expression, exception_class, exception_name, format...)


