---
title: Apps
permalink: Develop/Apps/
has_children: true
layout: default
nav_order: 100
---

# 开始

开始使用SDK是一个相当简单的过程：

## 安装

[下载最新SDK](/Tools/Sailfish_SDK) 安装程序并安装。阅读有关[SDK安装](/Tools/Sailfish_SDK/Installation).

## 运行

运行SDK并使用它来创建应用程序。SDK附带了一个方便的Sailfish OS应用程序模板，为您提供了一种快速创建您的第一个Sailfish OS应用程序的方法。只需转到IDE中的 File -\> New File or Project。

了解更多关于创建 [第一个SDK应用程序](/Develop/Apps/Your_First_App)

## 探索

通过我们的SDK提供的[文档](/develop/apps#building-blocks)了解Sailfish用户界面的构建模块。

[加入Sailfish论坛](https://forum.sailfishos.org) 获取更新和支持。

## 提交

当您的应用程序准备就绪时，请将其带到Harbour，我们将确保它正常工作，与Sailfish OS兼容，并帮助您为Jolla设备启动它。之后，您可以跟踪仪表板上的开发并进行任何更正。

了解更多关于[Harbour](/Develop/Apps/Harbour)

# 软件开发工具包

[Sailfish SDK](/Tools/Sailfish_SDK)是用于开发Sailfish OS应用程序的工具集合。它包括：

  - Sailfish IDE: 基于QT Creator的集成开发环境（IDE）
  - 用于交叉编译的Sailfish OS构建引擎
  - Sailfish OS模拟器
  - sfdk命令行工具，用于不使用Sailfish IDE进行开发
  - 教程、设计和API文档
  - 扩展库和开放源代码的存储库

## Sailfish IDE

Sailfish IDE基于QT Creator，这是根据QT开发人员的需求定制的跨平台集成开发环境（IDE），扩展了对使用Sailfish Silica组件进行Sailfish UI应用程序开发的支持。它提供了一个具有版本控制、项目和构建管理系统集成的复杂代码编辑器。有关QT Creator的更多信息，请访问[www.qt.io](https://www.qt.io)

## Sailfish OS构建引擎

Sailfish OS构建引擎是一个包含Sailfish OS开发工具链和工具的虚拟机（VM）。它还包括用于构建和运行Sailfish和QML应用程序的Sailfish OS目标。目标挂载为共享文件夹，以允许Sailfish IDE访问编译目标。此外，您的主目录是共享的，并挂载在虚拟机中，因此可以访问您的源代码进行编译。

构建引擎还支持额外的构建目标和交叉编译工具链。可以使用Sailfish SDK安装程序/维护工具安装其他构建目标。然后，可以在Sailfish IDE中使用Options \> Sailfish OS \> Build Engine \> Manage Build Targets 来进一步管理已安装的构建目标。

## 模拟器

模拟器是一个x86虚拟机映像，其中包含目标设备软件的精简版本。它模拟了运行Sailfish操作系统的目标设备的大部分功能，如手势、任务切换和环境。

## sfdk命令行工具

是一个命令行工具，它允许您在不使用Sailfish IDE的情况下构建软件包。您可以在Sailfish SDK安装的bin目录中找到SFDK可执行文件。在Windows上，我们建议在msys2 shell中使用它，以获得最佳体验。在MacOS上，建议使用自制程序中的“bash”和“bash-completion@2”的最新版本。

## 编译模块

Sailfish OS应用程序利用Sailfish OS堆栈，允许快速开发美观、内容丰富的应用程序，这些应用程序可以在各种不同的外形上完美运行，只需进行最小的调整。本节介绍应用程序所基于的Sailfish OS堆栈中的层。

### Qt 5

Sailfish OS应用程序基于QT 5，使开发人员能够以比以往更快的速度为多个目标开发具有直观用户界面的应用程序。QT 5使解决触摸屏和平板电脑所需的最新UI模式转变变得更加容易。有关QT的更多信息，请访问[QT Project网站](http://doc.qt.io/qt-5).

### Qt Quick2

QT Quick 2是下一代的QT Quick，它是一种高级UI技术，允许开发人员和UI设计人员协同工作，以创建动画、支持触摸的UI和轻量级应用程序。更多信息请参阅[QT Quick2 文档](http://doc.qt.io/qt-5/qtquick-index.html).

### Wayland

在Sailfish OS的当前版本中，在图形管道中使用了Wayland而不是X11，从而改善了用户体验。Sailfish OS提供了一个功能齐全的合成器，负责窗口管理和将图形输出到屏幕。更多关于Wayland的信息，请访问[Wayland主页](https://wayland.freedesktop.org).

### Sailfish Silica

Sailfish Silica是一个QML模块，为应用程序提供Sailfish UI组件。它们的外观和感觉符合Sailfish的视觉风格和行为，并支持独特的Sailfish UI应用程序功能，如滑轮菜单和应用程序封面。更多信息请参见[Silica 文档](https://sailfishos.org/develop/docs/silica/).

### 平台APIs

Sailfish OS使用开放开发和移动优化的核心，用于其自身的大部分核心组件。您可以实时访问 <https://github.com/sailfishos/>中使用的开放源代码

我们正在积极工作，以确定我们可以通过兼容性承诺正式支持的平台API集。同时，您可以尝试在SDK中使用您感兴趣的开发头文件扩展Sailfish OS目标。

### 开放源码

当然，我们提供了此版本中使用的开源代码。您可以在releases.sailfishos.org上找到。如果二进制文件的源代码未与二进制文件一起提供给您，您也可以通过向我们提交书面请求来获得物理介质上的源代码副本。源代码的书面报价中提供了更多信息。

# Sailfish OS教程

在本节中，您将找到我们为帮助您开发Sailfish OS应用程序而准备的教程。我们将在这里添加更多可用的教程。

## C++与QML的结合

在本节中，您将找到我们为帮助您开发Sailfish OS应用程序而准备的教程。我们将在这里添加更多可用的教程。([Read more…](/Develop/Apps/Tutorials/Combining_C++_with_QML))

## 用Python创建应用程序

除了C++之外，Python是一种完全受支持的用于开发Sailfish OS应用程序的语言。它特别适合于那些对资源要求不高的应用程序。如果您的应用程序需要进行繁重的处理，我们建议使用C++而不是Python。在本教程中，我们将创建一个简单的Python应用程序来显示颜色列表。([Read more…](/Develop/Apps/Tutorials/Creating_an_application_in_Python))

## 构建包 - 高级技术

Sailfish SDK通过Sailfish IDE提供简化的开发人员体验。但是，本机支持仅适用于使用qmake或cmake作为其构建系统的项目，在将现有应用程序移植到Sailfish OS时，尤其是在处理平台组件时，可能不会出现这种情况。这样的项目可以从命令行手动构建，并且通过一个中间步骤，也可以在Sailfish IDE中打开它们，并启用通常的高级编辑功能。对于那些喜欢使用不同的代码编辑环境或希望在持续集成系统的上下文中使用Sailfish SDK的人来说，本文档中描述的技术也很有用。 ([Read more…](/Develop/Apps/Tutorials/Building_packages_-_advanced_techniques))

## 使用Qt-qmlive进行QML实时编码

在QT Automotive Suite的QT QMLLive工具的帮助下，为Sailfish OS创建QT Quick应用程序可以更加有效。QT QMLLive支持具有两个基本功能的实时编码。首先，它允许发布源代码修改，消除了重新部署应用程序以查看效果的需要。其次，它可以指示您的应用程序应该加载哪个特定的QML组件，而不是“主”组件，以便每个组件都可以独立工作。([Read more...](/Develop/Apps/Tutorials/QML_Live_Coding_With_Qt_QmlLive))

# 指导方针

## 编码约定

Sailfish应用程序是用QT和QML编写的，应遵循QT编码约定。有关详细信息，请参阅 [编码约定](/Develop/Apps/Coding_Conventions) 页面.

## 完成的UI定义

[完成的UI定义](/Develop/Apps/UI/Definition_of_Done) 是为平台的功能贡献而强制执行的，但也为应用程序开发提供了一个很好的检查表。

## 常见的坑

[常见的坑页面](https://sailfishos.org/develop/docs/silica/sailfish-application-pitfalls.html/) 列出了Sailfish应用程序开发中常见的反模式。

# API文档

在本节中，您将找到开发Sailfish OS应用程序时可以参考的相关文档。我们将在这里添加更多可用的API文档。

## Sailfish Silica

Sailfish SDK包括Sailfish Silica，这是一个用于开发您自己的Sailfish应用程序的QML模块。Sailfish应用程序是用QML和C++代码的组合编写的。QML是QT框架提供的一种声明性语言，可以轻松创建具有平滑过渡和动画的时尚自定义用户界面。基于QML的用户界面可以连接到基于C++的应用程序后端，该后端实现更复杂的应用程序功能或访问第三方C++库。

阅读 [Silica API文档](https://sailfishos.org/develop/docs/silica)，了解有关如何开发Sailfish风格应用程序的更多信息。

## Sailfish Pickers

Sailfish Pickers模块提供了一组Picker组件，用于在开发您自己的Sailfish应用程序时选择内容。

阅读 [Sailfish Pickers API 文档](https://sailfishos.org/develop/docs/sailfish-components-pickers/)了解更多信息。

## libsailfishapp

为了简化Sailfish应用程序的开发，确保正确设置应用程序的路径，并加快应用程序启动时间，Sailfish OS上的第三方应用程序应使用libsailfishapp。这个库提供了一些方便的函数来设置项目，将所有文件安装到正确的目录中，并在运行时通过方便的方法获取重要的路径。

阅读 [libsailfishapp 文档](https://sailfishos.org/develop/docs/libsailfishapp) 了解有关如何使用该库的更多信息。

## Sailfish图标参考手册

Sailfish图标包提供了数百个平台风格的矢量图标，可供Sailfish应用程序使用。平台图标分为五个大小类别：通用大小变体（小、中、大），以及用于覆盖操作和通知的特殊类别

您可以在最新的[Sailfish Icon参考文档](https://sailfishos.org/develop/docs/jolla-ambient). 中浏览可用图标。或者在墙上打印3号[(outdated)一页](https://sailfishos.org/content/uploads/2018/11/icon_reference.png).

## 配置API的设定

配置模块提供从QML访问存储在DCONF中的配置设置的类型。这些类型提供了一个基于属性的API，支持异步读取和写入配置值和更改通知，使Dconf能够在QML属性绑定中无缝使用。

阅读 [配置API文档](https://sailfishos.org/develop/docs/nemo-qml-plugin-configuration) 了解更多信息。

## DBus API

Nemo Mobile D-Bus QML插件允许您访问系统和会话总线上的服务，以及提供您自己的服务。D-BUS用于进程间通信。几个系统服务在D-BUS上公开了一个可由第三方软件和其他中间件使用的接口。

阅读 [DBus API 文档](https://sailfishos.org/develop/docs/nemo-qml-plugin-dbus) 了解更多信息。

## 消息通知API

QML插件通知QML插件提供C++类和QML类型，允许发布通知。通知将具有特定的类别，并且根据其类别将为用户触发各种图形（横幅、事件视图弹出窗口等）和非图形（声音和振动）反馈。

阅读 [Notifications API 文档](https://sailfishos.org/develop/docs/nemo-qml-plugin-notifications) 了解更多信息。

## MDM API

移动设备管理是许多使用情形的重要要求，允许以各种方式或响应于某些事件（例如，在设备丢失或被盗的情况下）来限制或控制特定设备或设备群的功能。Sailfish OS提供Sailfish OS MDM框架，允许MDM应用程序在设备上应用限制性策略或手动启用、禁用或触发特定功能。MDM Framework提供的第二方API仍在积极开发中，在一定程度上可能会发生变化。

阅读 [MDM API 文档](https://sailfishos.org/develop/docs/sailfish-mdm/) 了解更多信息。
