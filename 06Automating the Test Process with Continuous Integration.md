# 自动化测试过程的持续集成 #

在开发过程中除了以交互方式运行测试外，还可以充分采取使用Xcode Server进行自动化测试。本章将介绍如何使用`OS X Server`和`Xcode Server`的持续集成功能来增强和扩展你的开发测试。

![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/bot_viewer-summary_2x.png)

## 基于服务器的持续集成测试 ##

`Xcode`测试具有交互性，它能确保您的代码在其指定的需求下不偏离正确的轨道，并且可以很简单地找到并修复`bug`。一系列快速运行的功能测试不仅可以校正你的开发，还可以让你高效率自信地构建一款健壮的应用程序。
 
这就是说,成功的开发项目往往超出单个开发人员实现和维护的能力范围。像源码管理，服务器上的自动化测试可让你顺利高效地根据团队的需要进行开发工作。通过`Xcode Server``Xcode`支持基于服务器的持续集成工作流。`Xcode Server`适用于 `OS X Server`，可以自动化应用程序的构建、分析、测试以及归档的一体化过程。

下面是使用基于服务器测试的一些优点：
1. 使用服务器可以进行脱机构建和测试，以缓解开发系统做实施和调试的压力，特别是在全方位测试时可能需要很长的时间来执行。
 
2. 开发团队的所有成员使用相同的scheme可在服务器上运行相同的测试，从而提高测试的一致性，整个团队也可以构建产品,就像构建和测试报告。
 
3. 你可以灵活调整调度项目需求和团队的需求。比如，当团队中任意一个成员向源码管理系统提交新工作或者在设定的时间定期提交时测试运行就可以开始了。测试运行也可以按照需要手动启动。
 
4. 服务器以同样的方式反复运行测试。随着时间的推移，服务器的报告可以让你和你的团队对构建过程中的问题、警告以及测试解决方案有个整体的轮廓。
 
5. 你的项目可以有更多的目的地进行测试，更具自动性，而且比手动运行测试系统更加经济。例如,您可以有任意数量的iOS设备连接到服务器,使用单一的配置,该系统可以构建和测试库、应用程序、所有测试以及iOS模拟器的多个版本。
 
你的项目可以基于多个目的进行测试，另外它的自动性比手动运行测试更经济。比如你可以有任意数量的连接到服务器的iOS设备，通过单一配置让，系统可以构建和测试库、应用程序、所有测试以及并联iOS模拟器的多个版本

>注意
>当你的项目工作横跨`iOS`和`mac OS X`时，应用就会有两个约束：
>
>`OS X`项目构建和测试时，安装有`OS X`服务器必须处于运行当中。
>
>`iOS`和`OS X`的项目需要各自不同的`scheme`和`bot`。

## 概述 - 使用Xcode的服务器和持续集成 ##

使用`Xcode Server`和持续集成的工作流可以设计无缝和透明的交互式开发。使用基于服务器持续集成的主要任务是服务器的安装和配置项目,在这里我们简要描述一下。在开始使用`Xcode`服务器和持续集成系统时，只需要三个高级任务。此后，该任务要做的显示正在进行的监测结果和进行不定期的维护。本文简要总结这些任务的详细信息[Xcode Continuous Integration Guide](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/xcode_guide-continuous_integration/000-About_Continuous_Integration/about_continuous_integration.html#//apple_ref/doc/uid/TP40013292)。

### 共享计划，脚本，和集成 ###

首先，了解要讨论的`Xcode Server`所使用的一些关键术语是非常有用的。

**Shared schemes.**相信您应该已经很熟悉`Xcode scheme`的概念了;它是构建项目的计划。它定义了需要构建哪些目标，依赖目标需要构建什么，什么样的构建设置将被传递到构建过程中。因为服务器是要建立自己的代码，它需要访问这些信息。要做到这一点，在你的计划中配置`scheme`的设置，并使之成为`shared scheme`，并签入源代码库。你可以手动地在你为项目定义的`scheme`中，选择列表中`scheme`右边的`Shared`复选框，使用`Scheme Manager`。
![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/twx-manage-schemes-sheet_2x.png)

>**注意:**在大多数情况下，当你创建一个`bot`时，`Xcode`会为你自动配置共享scheme。

**Bots.** 在`Xcode`中进行`Mac`开发时，你创建的`bots`会运行在单独的服务器上，它们在那里执行这些集成。`Bot`是用于分析、构建、测试以及按计划归档项目的配置。`Bot`有助于确保您的产品始终处于可释放状态，当有故障时，该服务会通知你或那些代码发生更改导致失败人。

**Integrations.**`Bot`执行集成,集成是一个单独运行的`Bot`。

### 配置 ###
在开始使用Xcode服务器和持续集成系统前，需要完成三个高级任务。

### 安装和设置`Xcode Server`###

即使你之前从来没有建立过服务器，你也将会觉得这很简单很容易完成。您需要在目标机器上安装`OS X Server`和配置`Xcode Server`。当然你还需要在服务器上安装好`Xcode`。

>注意:你要确定运行在目标机上的`OS X`，`Mac OS X Server`和`Xcode Server` 版本都是兼容的。。

关于安装`OS X Server`和配置`Xcode Server`的详细信息，请参阅["Install OS X Server and Configure the Xcode Service"](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/xcode_guide-continuous_integration/200-Adopting_a_Continuous_Integration_Workflow/adopt_continuous_integration.html#//apple_ref/doc/uid/TP40013292-CH3)得到详细的指示说明。

### 连接Xcode Server到源代码库 ###

你的 bots 必须能够访问项目的源代码库。`Xcode Server` 支持两种流行的源代码控制系统：`Git`和`Subversion`。你可以使用`Git`和托管在远程服务器上的`Subversion` 版本库，你可以在安装了`OS X Server`的服务器上托管和使用`Git`仓库。
 
最简单的情况，当你创建一个新的项目时，设立远程存储库就能满足了。在`Xcode`中，你可以选择选项来创建一个`git` 仓库，并在新项目设置工作流中，从弹出菜单 “Create git repository” 中选择服务器。
 
还有其他一些方法来设置源代码库，这取决于您正在使用的项目，以其相对应的源代码控制状态等等。关于设置源代码存储库的详细说明，请查阅 "[Enable Access to Your Source Code Repositories](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/xcode_guide-continuous_integration/PublishYourCodetoaSourceRepository/PublishYourCodetoaSourceRepository.html#//apple_ref/doc/uid/TP40013292-CH8)"。

### 选择 Product > 创建配置bot和运行bots ###

Bots 是自动化工作流程的核心；他们使用项目`schemes`去构建和测试产品。在`Xcode`中，你在自己的开发系统上创建 bots，并使之定期运行或者在你每次提交源码的时候运行。你也可以配置 Bot 以电子邮件报告形式通知你集成使用成功与否。
 
有关创建和运行Bots的详细信息,参见["Configure Bots to Perform Continuous Integrations"](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/xcode_guide-continuous_integration/ConfigureBots/ConfigureBots.html#//apple_ref/doc/uid/TP40013292-CH9)。

## 持续集成工作流程 ##

当你已经完成后三个配置任务后，`Xcode Server`和持续集成系统就已经启动和运行了。它被设计成一个易懂的交互式开发工作流程。根据你配置`bots` 和实现集成的方式，你可以像过去那样以同样的方式来执行代码开发以及本地的、交互式的测试任务。通常情况下，你会在到达停止点、完成特定任务等时，提交你的工作到源代码库。

Bots运行在服务器上，执行你配置的集成，并返回报告。在 _Xcode_中可以使用log navigator管理Bots，查看测试结果，阅读集成志，启动整合，并下载产品构建信息和归档。例如，下面就是使用log navigator得到的可用的测试结果视图：
 ![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/twx-integration_viewer-tests_2x.png)

您还可以使用Web浏览器来查看测试和报告。

![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/twx-integration_page-tests_2x.png)

想要了解所有管理bots的不同功能，并查看更多的集成结果，请参阅，参见 ["Manage and Monitor Bots from the Log Navigator"](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/xcode_guide-continuous_integration/300-Working_with_Bots/view_integration_results.html#//apple_ref/doc/uid/TP40013292-CH4) 和 ["Manage and Monitor Bots from a Web Browser"](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/xcode_guide-continuous_integration/MonitorBotsandDownloadProductsfromaWebBrowser/MonitorBotsandDownloadProductsfromaWebBrowser.html#//apple_ref/doc/uid/TP40013292-CH10)。
