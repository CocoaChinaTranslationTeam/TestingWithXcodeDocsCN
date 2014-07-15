本章由CocoaChina翻译小组成员dada翻译 [github地址](https://github.com/sherlockdan)

#编写测试类与方法
当你使用测试导航面板往项目中添加测试目标时，Xcode会在测试导航面板里展示出测试类与方法。在测试目标里是包含测试方法的测试类。本章节讲述怎样创建测试类和编写测试方法。


##测试目标，测试包，测试导航
在学习创建测试类前，有必要看看测试导航面板。它对创建和运行测试工作极为重要。

测试导航面板罗列了测试包里的所有组件内容，并在一个层次列表里展示出测试类和测试方法。下面是一个工程的测试导航面板视图，包含了多个测试目标，展示了测试包、测试类以及测试方法的嵌套层级。

![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/twx-testnav-bundle_hierarchy_2x.png)

测试包可包含多个测试类。你可以使用测试类把测试分到相关的组群中，或者按照功能分，或者按照组织目的分。比如，对于本章中的计算器示例工程，你可以创建`BasicFunctionsTests`, `AdvancedFunctionsTests`以及 `DisplayTests classes`三个类，但他们都属于`Mac_Calc_Tests`测试包。

![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/twx-testnav-three_test_classes_2x.png)

一些测试类型可能会共享某些类型的安装和卸载需求，把这些测试整合到类里面会更加合理，这样可以最小化每个测试方法所需编写的代码。

##创建测试类
可以使用加号按钮（+）和测试导航面板中的“New Test Class”命令创建新测试类。 

![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/twx-testnav-new_test_class_2x.png)

基于你在配置页面键入的测试类名，你添加的每个类都会使得一个名为TestClassName.m的文件被添加到项目中。

![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/twx-test_class_config_sheet_2x.png)

>注意：所有测试类都是`XCTest`框架`XCTestCase`类的子类。

尽管Xcode会默认的把测试类添加到为工程测试目标所创建的组中，你还是可以在项目中组织自己选择的文件。当你按下Next按钮，标准的Xcode添加文件页面如下所示：

![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/twx-file_handling_sheet_2x.png)

你可以用相同的方法在工程导航面板中添加文件。具体使用方法详见[Adding an Existing File or Folder](https://developer.apple.com/library/mac/recipes/xcode_help-structure_navigator/articles/Adding_an_Existing_File_or_Folder.html#//apple_ref/doc/uid/TP40009934-CH17)

>注意：当你用Xcode5及以上版本创建工程时，一个测试目标和相关的测试包都会默认的被创建，名字根据工程名获得。比如创建名为`MyApp`的项目，则会自动生成名为`MyAppTests`的测试包，以及一个名为`MyAppTests`的测试类,关联在`MyAppTests.m`实例文件中。

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

测试类用Objective-C实现。注意实现里包含了方法，比如setup和teardown的基本实例方法；这些方法是必须的。如果类中所有的测试方法都需要相同的代码，你可以定制setUp和tearDown来包含这些代码。你添加的代码在每一个测试方法的之前和之后执行。你可以有选择性的添加定制的设置(+ (void)setUp) 和卸载 teardown (+ (void)tearDown) 方法，他们在类里所有测试方法的之前和之后执行。

Xcode执行测试可以明确这些方法的使用，这正是我们接下来的内容要讨论的

##测试执行的流程
在执行测试的过程中，XCTest找到所有继承于`XCTestCase`（它是测试类）的类，为每一个类运行它们的测试方法。

对于每个类来说，测试开始于运行类setup方法。对于每个测试方法来说，一个新的类实例被创建，它的实例setup方法就会执行。在跑完了测试方法之后，实例卸载方法。类中这样连续重复执行所有测试方法。在运行的类卸载了最后的测试方法后，Xcode会执行类卸载方法，并开始下一个类。这种序列一直重复直到跑完所有测试类的所有测试方法。


##编写测试方法
你通过编写测试方法把测试写到测试类中，一个测试方法是以_test_开头的测试类的实例方法，没有参数，返回`void`，比如`testColorIsRed`.测试方法调用工程中的代码，如果代码没有产生预期的结果，那么会用一系列的断言API报错。比如，
一个函数返回值可能与预期值相比不同，或者你的测试方法使用了某个类里不适当的方法都将会抛出异常。[ “XCTest Assertions”](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/testing_3_writing_test_classes/testing_3_writing_test_classes.html#//apple_ref/doc/uid/TP40014132-CH4-SW34)描述了这些情况。

为了测试方法正常访问即将被测试的代码，引入正确的头文件到测试类中很重要。

当Xcode运行测试时，它调用的每个测试方法都是独立的。因此每个方法需要准备和清理辅助变量、结构以及与主题API进行交互的对象等。如果类中所有测试方法的代码是相同的，你可以直接把它添加到必需的`setUp`和`tearDown`的实例方法中，详见[ “Test Class Structure.”](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/testing_3_writing_test_classes/testing_3_writing_test_classes.html#//apple_ref/doc/uid/TP40014132-CH4-SW2)

下面是个测试方法的模型:

     - (void)testColorIsRed {
        ...     // Set up, call test subject API. (Code could be shared in setUp method.)
        ...     // Test logic and values, assertions report pass/fail to testing framework.
        ...     // Tear down. (Code could be shared in tearDown method.
     }



这里有一个简单的测试方法例子，检查是否成功地为SampleCalc创建了`CalcView`实例，详见[ “Quick Start” ](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/testing_1_quick_start/testing_1_quick_start.html#//apple_ref/doc/uid/TP40014132-CH2-SW1)章节：



    - (void) testCalcView {
       // setup
       app = [NSApplication sharedApplication];
       calcViewController = (CalcViewController*)[NSApplication sharedApplication] delegate];
       calcView             = calcViewController.view;
     
       XCTAssertNotNil(calcView, @"Cannot find CalcView instance");
       // no teardown needed
    }    
    For further examples of test methods implemented in a project, see the Testing Apps and Frameworkssample code project.


更深一点的例子可以在_Testing Apps and Frameworkssample_工程中找到代码


##XCTest断言

你的测试方法使用XCTest框架提供的断言来展示Xcode展示的测试结果。所有断言都有一个类似的形式：项目比较或逻辑表达式，一个失败结果字符串格式，和插入到字符串格式中的参数。


>注意：所有断言的最后一个参数是`format...`，格式字符串和变量参数列表。XCTest为所有断言提供了默认的失败结果字符串，可使用参数传递到断言里。`format`字符串提供了可选的额外的失败自定义描述，就可以进一步选择提供的描述。这个参数是可选的，也可以完全被忽略。
    
    
比如，[Quick Start](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/testing_1_quick_start/testing_1_quick_start.html#//apple_ref/doc/uid/TP40014132-CH2-SW1)里`testAddition`方法中的断言：

     XCTAssertEqualObjects([calcViewController.displayField stringValue], @"10", @"Part 2 failed.");

阅读这段简单明了的语句，是说：“Indicate a failure when a string created from the value of the controller’s display field is not the same as the reference string ‘8’” 。如果断言失败，那么Xcode在测试导航面板发出失败信号，然后Xcode会在issues导航面板、源码编辑器以及其他地方展示失败的描述。下面是源代码编辑器中典型的断言结果：

![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/twx-test_failure_assert_result_2x.png)

测试类可以包含多个断言，如果任何断言包含失败报告，那么Xcode都发出的测试失败信号。

断言分为五种类型：无条件失败、等价测试、nil测试、Boolean测试以及异常测试。

##用类别区分断言

下面的区域列出了XCTest断言。你可以在Xcode中使用Quick Help查看`XCTestAssertions.h`来引用来获得更多关于XCTest断言的信息。。

###绝对失败
__XCTFail__.生成一个绝对失败。

     XCTFail(format...)


###相等测试
__XCTAssertEqualObjects__.当_expression1_不等于_expression2_时报错（或者一个对象为空，另一个不为空）。

     XCTAssertEqualObjects(expression1, expression2, format...)

__XCTAssertNotEqualObjects__. 当_expression1_等于_expression2_时报错

     XCTAssertNotEqualObjects(expression1, expression2, format...)


__XCTAssertEqual__. 当_expression1_不等于_expression2_时报错，这个测试用于C语言的标量。

     XCTAssertEqual(expression1, expression2, format...)

__XCTAssertNotEqual__.  当_expression1_等于_expression2_时报错，这个测试用于C语言的标量。

     XCTAssertNotEqual(expression1, expression2, format...)


__XCTAssertEqualWithAccuracy__. 当_expression1_和_expression2_之间的差别高于 _accuracy_时报错。这种测试适用于floats和doubles这些标量，两者之间的细微差异导致它们不完全相等，但是对所有的标量都有效。

     XCTAssertEqualWithAccuracy(expression1, expression2, accuracy, format...)

__XCTAssertNotEqualWithAccuracy__. 当_expression1_和_expression2_之间的差别低于 _accuracy_ 时报错。这种测试适用于floats和doubles这些标量，两者之间的细微差异导致它们不完全相等，但是对所有的标量都有效。 


     XCTAssertNotEqualWithAccuracy(expression1, expression2, accuracy, format...)


###Nil(空)测试

__XCTAssertNil__.  当_expression_参数非nil时报错。

    XCTAssertNil(expression, format...)

__XCTAssertNotNil__. 当_expression_参数为nil时报错。

    XCTAssertNotNil(expression, format...)
    
    
###布尔测试


__XCTAssertTrue__. 当_expression_计算结果为_false_时报错。

    XCTAssertTrue(expression, format...)

__XCTAssert__. 当_expression_计算结果为_false_时报错，与_XCTAssertTrue_同义。

     XCTAssert(expression, format...)
     

__XCTAssertFalse__.  当_expression_计算结果为_true_时报错。

     XCTAssertFalse(expression, format...)


###异常测试

__XCTAssertThrows__.当_expression_不抛出异常时报错误。

     XCTAssertThrows(expression, format...)
     
__XCTAssertThrowsSpecific__.当_expression_针对指定类不抛出异常时报错。

     XCTAssertThrowsSpecific(expression, exception_class, format...)

__XCTAssertThrowsSpecificNamed__.当_expression_针对特定类和特定名字不抛出异常时报错。对于AppKit框架或Foundation框架非常有用，抛出带有特定名字的NSException(NSInvalidArgumentException等等)。

    XCTAssertThrowsSpecificNamed(expression, exception_class, exception_name, format...)


__XCTAssertNoThrow__. 当_expression_抛出异常时报错。

     XCTAssertNoThrow(expression, format...)


__XCTAssertNoThrowSpecific__. 当_expression_针对指定类抛出异常时报错。任意其他异常都可以；也就是说它不会报错。

    XCTAssertNoThrowSpecific(expression, exception_class, format...)


__XCTAssertNoThrowSpecificNamed__.当_expression_针对特定类和特定名字抛出异常时报错。对于AppKit框架或Foundation框架非常有用，抛出带有特定名字的NSException(NSInvalidArgumentException等等)。


     XCTAssertNoThrowSpecificNamed(expression, exception_class, exception_name, format...)


