---
title: Develop
permalink: Develop/
nav_exclude: true
layout: default
---

# 把事情做好

如果您热衷于为Sailfish OS进行开发，那么这是正确的起点。

花点时间看看这个页面的内容，你会发现我们将涵盖许多主题：发展领域/角色;与他人合作;开源;关键的Sailfish OS技术，在底部有一些关于常见开发人员任务的指南。

## Sailfish OS中的开发领域或角色

虽然可能会有一些重叠，但将Sailfish OS开发分解为许多不同的领域和/或角色是有帮助的。

### 应用程序开发人员

应用程序扩展了Sailfish OS的功能，通常为用户提供一个新的界面来使用他们的设备。它们的范围从小而简单的独立应用程序到与Sailfish OS和世界其他地方进行复杂交互的复杂套件。Sailfish OS提供了Silica组件（使用QT的QML构建），允许开发人员将原生外观集成到他们的应用程序中。

如果您正在进行这种类型的开发，那么您通常使用Sailfish OS SDK作为您的主要代码开发工具（尽管您可以自由地将您自己的工具与平台开发人员工具箱中的工具结合使用）。

您正在编写的代码可能是开源的，也可能不是开源的，但您肯定会与开源代码、工具和/或项目进行交互。

应用程序开发人员部分将包括：

  - 入门开发者
  - 软件开发工具包
  - Sailfish OS 教程
  - API文档
  - Harbour

阅读有关[应用程序开发]的更多信息(/Develop/Apps).

### UI设计师

为应用程序提供有吸引力且可用的界面是一项挑战。

UI部分包括：

  - 逻辑，一致性和直观的运动
  - 设计原则
  - UX框架	
  - 手势
  - 导航架构
  - App导航架构
  - QML

阅读更多关于[UI开发](/Develop/Apps/UI).

### 平台开发者

如果你正在开发Sailfish操作系统本身，那么这就是平台开发。这种类型的工作更具协作性，您将与开源社区中的其他人一起开发Sailfish OS代码。

这方面的开发通常是标准的Linux命令行开发。

虽然应用程序开发可以相对轻松地进行代码更改，但平台开发有可能影响大量用户和其他开发人员。这意味着接受平台变更的流程非常严格。

阅读更多关于[平台开发](/Develop/Platform)的信息.

### 硬件适配开发者

硬件适配是Sailfish OS满足底层硬件的地方。开发内核、配置、驱动程序等。

HADK（硬件适配开发工具包）中详细记录了硬件适配移植过程。一旦您开始移植过程，您几乎肯定希望与IRC频道上的社区讨论问题。最终，当你有一个成功的端口时，你可能想把它发布给社区-或者你的构建/发布工程师。


阅读更多关于[硬件适配开发](/Tools/Hardware_Adaptation_Development_Kit)的信息.

### 构建/发布工程师

如果您的任务是将所有不同开发分支的工作整合在一起，并交付工作系统映像，那么您就是Sailfish OS构建/发布工程师。

这是一个复杂的领域，将涵盖：

  - OBS项目
  - 推广，测试
  - Bug/Feature管理

阅读更多关于[构建工程师](/Build_Engineering "brokenlink").

### QA工程师

质量问题和测试是确保Sailfish OS设备中所有组件协同工作的关键部分。

除了运行各种测试用例外，您还需要熟悉：

  - 刷机
  - 升级
  - 运行测试
  - Bug重现
  - 测试工具
  - Bug报告

阅读更多关于[QA工程师](/QA_Engineering "brokenlink").

## 协作开发流程

Sailfish OS主要是开源的，几乎所有平台上的开发都是在公共系统上进行的。

Sailfish OS主要是开源的，几乎所有平台上的开发都是在公共系统上进行的。

社区是欢迎和支持的，但礼貌的做法是了解一些事情是如何做的，以避免浪费别人的时间。本节将介绍这些要点：

  - 使用Sailfish论坛处理功能/错误（Feature/Bug）
  - 使用GitHub提交和审查代码
  - 在GIT进行打Tag包/发布
  - 使用OBS构建
  - 使用OBS推广
  - 使用IMG构建QA镜像

阅读更多关于[协作开发流程](/Develop/Collaborate).

## 开源

Sailfish操作系统的大部分是使用源代码构建的，这些源代码在开源许可下可用。

您需要了解一些关于许可证的知识，以及您可以和不可以使用开源代码做什么。它有助于了解“上游”项目如何工作以及它们与Sailfish操作系统的关系。

阅读更多关于[开源](/Develop/Open_Source).

## Sailfish OS关键技术

Sailfish OS使用了大量不同的技术——实际上，浏览一下[Sailfish OS参考手册](/reference)区域，就会看到一长串架构区域、中间件、API、系统和工具。

我们将只关注典型开发人员最常使用的系统和工具技术。

  - Qt
    [Qt](http://qt-project.org/) 是一个功能强大的跨平台应用程序库，非常适合连接设备。在Sailfish OS系统中，它充当主要的图形用户界面，并为大多数其他常用的设备功能提供一致的API。
  - Qt Creator
    QT项目提供了一个复杂的IDE，该IDE已得到增强，可以与Sailfish OS一起工作-您可以在[Qt 开发者网站](http://doc.qt.io/qt-5/topics-app-development.html)上阅读更多关于标准的QT Creator，然后找到有关Sailfish OS扩展的信息。
  - Scratchbox 2
    Scratchbox2解决了交叉编译的问题，它创建了一个看起来像目标系统的虚拟开发环境，同时允许透明地执行与目标兼容和与主机兼容的二进制文件。Scratchbox 2由Sailfish SDK和OBS共同使用。
  - git
    Sailfish OS广泛使用Git来管理变更。有关代码托管的更多信息，请访问[Sailfish OS 源码](/Services/Development/Sailfish_OS_Source)
  - OBS
    开放构建系统用于执行托管在Git服务上的Sailfish OS包的自动清理构建。
  - IMG
    IMG系统使用由OBS创建的包来生成Sailfish OS映像
  - Bugzilla
    Sailfish OS在内部使用Bugzilla来跟踪问题和功能。公共问题跟踪处理请访问<https://forum.sailfishos.org/>

阅读更多关于 [Sailfish OS技术](/Reference).

## 任务

大多数开发人员都需要执行一些常见任务：:

  - 修复核心组件中的错误
  - 创建包
  - 创建映像
  - 设备刷机
  - 诊断

了解更多关于 [通用开发任务](/Develop/Common_Tasks).
