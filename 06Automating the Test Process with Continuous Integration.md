# 自动化测试过程的持续集成 #

在开发过程中除了以交互方式运行测试外，还充分采取使用服务器的方式来实现自动化测试。本章将介绍如何使用`OS X Server`和`Xcode Server`的持续集成功能来增强和扩展您的开发测试。

![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/bot_viewer-summary_2x.png)

## 基于服务器的持续集成测试 ##

`Xcode`的测试具有可交互使用的能力,它能确保您的代码在其指定的需求下不偏离正确的轨道，并且开发简单，容易找到并修复`bug`。一系列的快速运行的功能测试不仅可以证明你的工作，还可以以高效自信的方式来确保你应用程序的健壮性。

通常讲,成功的开发项目的范围往往超出单个开发人员实现和维护的能力。如资源管理、服务器上的自动化测试提供的好处,让你的开发工作规模需要扩展至一个团队才能顺利和高效的开展进行。通过`Xcode Server`，`Xcode`支持基于服务器的持续集成的工作流程。`Xcode Server`，在`OS X Server`中也可用，并可以自动构建集成过程,分析,测试和归档你的应用程序。

下面是使用基于服务器测试的一些优点：

- 使用服务器可以进行脱机构建和测试，以缓解开发系统做实施和调试的压力，特别是在全方位测试时，这可能需要很长的时间来执行。
- 开发团队的所有成员都使用相同的方式在服务器上运行相同的测试，从而提高测试的一致性。服务器也使得整个团队可以构建产品,就像构建和测试报告。（这句话翻译的有问题）
- 你可以灵活调整调度项目需求和团队的需求。例如，测试运行时，可以将团队任何成员的提交提交到资源管理系统，也可以是定时，即设定开始时间。测试运行也可以按照需要手动启动。
- 服务器运行测试以完全相同的方式依次进行。来至服务器的报告通过一段时间后生成的图片有效的为你和你的团队提供建设问题，构建警告和测试解决方案。（明白这一句要讲什么，总是找不到合适的语言表达）
- 您的项目可以有更多的目的地进行测试，更具自动性，而且比手动运行测试系统更加经济。例如,您可以有任意数量的`iOS`设备连接到服务器,使用单一的配置,该系统可以构建和测试库、应用程序、所有测试以及`iOS`模拟器的多个版本。

>注意
>当你的项目工作横跨`iOS`和`mac OS X`时，应用就会有两个约束：
>
>`OS X`项目构建和测试时，安装有`OS X`服务器必须正在运行
>
>`iOS`和`OS X`的项目需要各自不同的方案和构造。

## 概述 - 使用Xcode的服务器和持续集成 ##

使用`Xcode Server`和持续集成的工作流可以设计无缝和透明的交互式开发。使用基于服务器持续集成的主要任务是服务器的安装和配置项目,在这里我们简要描述一下。在开始使用`Xcode`服务器和持续集成系统时，只需要三个高级任务。此后，该任务要做的显示正在进行的监测结果和进行不定期的维护。本文简要总结这些任务的详细信息[在Xcode持续集成指南](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/xcode_guide-continuous_integration/000-About_Continuous_Integration/about_continuous_integration.html#//apple_ref/doc/uid/TP40013292)。

### 共享计划，脚本，和集成 ###

首先，了解要讨论的`Xcode Server`所使用的一些关键术语是非常有用的。

**Shared schemes.**相信您应该已经很熟悉`Xcode scheme`的概念了;它是一种你构建项目的计划。它定义了哪些目标是需要构建的、依赖目标需要构建什么,什么样的构建设置将被传递到构建过程中。因为服务器是要建立自己的代码，它需要访问这些信息。要做到这一点，在你的计划中配置方案的设置，并使之成为`shared scheme`，并签入源代码库。你可以手动的在已经定义任何计划中，使用`Scheme Manager`表通过计划列表右边的`Shared`选择复选框为您的项目使用共享计划。
![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/twx-manage-schemes-sheet_2x.png)

>**注意:**在大多数情况下，当你创建一个`bot`时，`Xcode`会为你自动配置共享方案。

**Bots.** 在`Xcode`中进行`Mac`时，您创建的`bots`会运行在单独的服务器上，在那里他们执行这些集成。`Bot`是用于分析，构建，测试，以及按计划归档项目的配置。`Bot`有助于确保您的产品始终处于释放状态，当有故障时，该服务会通知你或更改代码导致失败的那些人。

**Integrations.**`Bot`执行集成,集成是一个单独运行的`Bot`。

### 配置 ###
在开始使用`Xcode`服务器和持续集成系统时，只需要三个高级任务。(翻译的有点别扭)

### 安装和设置Xcode服务器 ###

即使你之前从来没有建立过服务器，你也将会觉得这很简单，很容易完成。您需要在目标机器上安装`OS X Server`和配置`Xcode Server`。当然你还需要在服务器上安装好`Xcode`。

>**注意:**你需要确定的是在计算机上运行的`OS X`，`Mac OS X Server`和`Xcode Server`版本都是兼容的。

安装`OS X Server`和`Xcode`的配置服务器时你可以在["Install OS X Server and Configure the Xcode Service"](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/xcode_guide-continuous_integration/200-Adopting_a_Continuous_Integration_Workflow/adopt_continuous_integration.html#//apple_ref/doc/uid/TP40013292-CH3)得到详细的指示说明。

### 连接Xcode Server到源代码库 ###

你的bots必须能够访问项目的源代码库。`Xcode Server`支持两种流行的源代码控制系统：`Git`和`Subversion`。您可以使用`Git`和托管在远程服务器`Subversion`版本库，你也可以在服务器安装`OS X Server`托管，并使用`Git`仓库。最简单的情况，当你创建一个新的项目时，设立远程存储库就能满足了。在`Xcode`中，选择创建一个`gi`t仓库，并从弹出的新项目设置工作流菜单中的"关于创建`git`仓库"选择服务器选项。

还有其他一些方法来设置您的源代码库，这取决于您正在使用的项目，以其相对应的源代码控制状态，等等。设置源代码存储库的详细说明在["Enable Access to Your Source Code Repositories"](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/xcode_guide-continuous_integration/PublishYourCodetoaSourceRepository/PublishYourCodetoaSourceRepository.html#//apple_ref/doc/uid/TP40013292-CH8)中做了详细的描述。


### 选择 Product > 创建配置bot和运行bots ###

Bots是自动化工作流程的核心;他们使用项目计划去构建和测试产品。在开发系统的Xcode中创建和调度Bots，会定期或在在每次提交源代码时运行。您也可以设定Bot以电子邮件形式通知你集成使用成功与否。

有关创建和运行Bots的详细信息,参见["Configure Bots to Perform Continuous Integrations"](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/xcode_guide-continuous_integration/ConfigureBots/ConfigureBots.html#//apple_ref/doc/uid/TP40013292-CH9)。

## 持续集成工作流程 ##

当你已经完成后三个配置任务后，`Xcode Server`和持续集成系统就已经启动和运行了。它被设计成一个透明的交互式开发工作流程。你想以过去同样的方式执行代码开发以及本地，交互式的测试任务，这取决于你如何配置`bots`执行集成。通常情况下，你会在到达停止点、完成里程碑等等时，提交你的作品到源代码库。

Bots运行在服务器上，以您配置进行集成，并返回报告。在Xcode中可以使用日志导航管理Bots，查看测试结果，阅读整合日志，启动整合，并下载产品构建信息和归档。例如，下面就是使用日志导航进行测试的结果视图：
 ![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/twx-integration_viewer-tests_2x.png)

您还可以使用Web浏览器来查看测试和报告。

![](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/art/twx-integration_page-tests_2x.png)

想要了解所有不同管理bots的功能，查看更多的集成结果，参见 ["Manage and Monitor Bots from the Log Navigator"](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/xcode_guide-continuous_integration/300-Working_with_Bots/view_integration_results.html#//apple_ref/doc/uid/TP40013292-CH4) 和 ["Manage and Monitor Bots from a Web Browser"](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/xcode_guide-continuous_integration/MonitorBotsandDownloadProductsfromaWebBrowser/MonitorBotsandDownloadProductsfromaWebBrowser.html#//apple_ref/doc/uid/TP40013292-CH10)。