---
title: 第一个应用程序
permalink: Develop/Apps/Your_First_App/
parent: Apps
layout: default
nav_order: 700
---

## 你的第一个应用

如果您尚未安装和运行SDK，请按照[安装指南](/Tools/Sailfish_SDK/Installation)进行操作。

### 启动Sailfish IDE

您可以从系统菜单中的“Sailfish IDE”条目启动（如果您是Linux终端用户，则可以从“~/sailfishos/bin/qtcreator”启动）。

举个例子，在Ubuntu上，打开破折号，输入“Sailfish”。单击“Sailfish IDE”图标以启动IDE。

<a href="Ubuntu_dash_QtC.png" style="width:30em;display:block">
    <img src="Ubuntu_dash_QtC.png"
         alt="Ubuntu_dash_QtC.png"
         class="md_thumbnail" style="max-width:100%"/>
</a>

### 创建Sailfish UI项目

SDK附带了一个Sailfish UI模板项目，可以非常轻松地开始使用。

1\. 在IDE中，单击 **File→New File** 或者 **Project**.

<a href="QtC_New_Project.png" style="width:30em;display:block">
    <img src="QtC_New_Project.png"
         alt="QtC_New_Project.png"
         class="md_thumbnail" style="max-width:100%"/>
</a>

2\. 选择 **Applications→Sailfish OS Qt Quick Application** 然后点击 **Choose**.

<a href="QtC_Choose_Template.png" style="width:30em;display:block">
    <img src="QtC_Choose_Template.png"
         alt="QtC_Choose_Template.png"
         class="md_thumbnail" style="max-width:100%"/>
</a>

3\. 为您的项目命名。确保它是在您的主目录下的某个位置创建的，然后单击 **Next**.

<a href="QtC_Template_01.png" style="width:30em;display:block">
    <img src="QtC_Template_01.png"
         alt="QtC_Template_01.png"
         class="md_thumbnail" style="max-width:100%"/>
</a>

4\. 您可以编辑项目的简短描述，或者只需单击 **Next**.

<a href="QtC_Template_03.png" style="width:30em;display:block">
    <img src="QtC_Template_03.png"
         alt="QtC_Template_03.png"
         class="md_thumbnail" style="max-width:100%"/>
</a>

5\. 选择生成系统。默认（qmake）是一个不错的选择，但我们也支持cmake，因此如果您更熟悉它，您可能想尝试它。

<a href="Buildsystem.png" style="width:30em;display:block">
    <img src="Buildsystem.png"
         alt="Buildsystem.png"
         class="md_thumbnail" style="max-width:100%"/>
</a>

6\. 选择用于生成项目的工具包。为32位ARM设备选择`SailfishOS-<version>-armv7hl`, 为64位ARM设备（例如Sony Xperia 10 II）选择`SailfishOS-<version>-aarch64`，或者给模拟器选择`SailfishOS-<version>-i486`

<a href="Kits.png" style="width:30em;display:block">
    <img src="Kits.png"
         alt="Kits.png"
         class="md_thumbnail" style="max-width:100%"/>
</a>

6\. 单击 **Finish**.

<a href="QtC_Template_04.png" style="width:30em;display:block">
    <img src="QtC_Template_04.png"
         alt="QtC_Template_04.png"
         class="md_thumbnail" style="max-width:100%"/>
</a>

7\. 项目模板将导入到项目中并在编辑器中打开。

<a href="QtC_Open_Project.png" style="width:30em;display:block">
    <img src="QtC_Open_Project.png"
         alt="QtC_Open_Project.png"
         class="md_thumbnail" style="max-width:100%"/>
</a>

### 启动构建引擎和模拟器

Sailfish SDK使用构建引擎（虚拟机或Docker容器）来编译代码，并使用另一个虚拟机来运行模拟器。当您尝试构建或部署应用程序时，如果这些应用程序未运行，系统将要求您启动它们。

> 注意：构建引擎需要访问您的源代码来编译它，并且默认情况下，您的主目录是共享的—这就是为什么项目应该在您的主目录中。

当Sailfish OS项目打开时，SDK会自动在左侧工具栏中显示两个控制按钮，用于启动/停止构建引擎和模拟器。

<a href="Toolbar_Icons.png" style="width:30em;display:block">
    <img src="Toolbar_Icons.png"
         alt="Toolbar_Icons.png"
         class="md_thumbnail" style="max-width:100%"/>
</a>

1. 单击 ![Build_Engine_Icon.png](/Tools/Sailfish_SDK/FAQ/Build_Engine_Icon.png "Build_Engine_Icon.png") 图标以启动生成引擎.\
   构建引擎在后台启动，图标将变为灰色，直到构建引擎启动。

1. 单击![Emulator_Icon.png](/Tools/Sailfish_SDK/FAQ/Emulator_Icon.png "Emulator_Icon.png") 图标启动模拟器\
   注意：此图标仅在 `SailfishOS-<version>-i486` 套件处于活动状态时可用。您可以从菜单 **Build → Open Build and Run Kit Selector…** （打开构建和运行工具包选择器）中激活`SailfishOS-<version>-i486` 工具包.

将打开一个新的VirtualBox窗口并启动模拟器。

#### 连接成功

当QT创建器可以成功连接到模拟器和构建引擎时，图标将更新，如下所示。

连接前：
  - <a href="Toolbar_Icons_Start.png" style="width:30em;display:block">
      <img src="Toolbar_Icons_Start.png"
           alt="Toolbar_Icons_Start.png"
           class="md_thumbnail" style="max-width:100%"/>
    </a>

已建立连接：
  - <a href="Toolbar_Icons_Stop.png" style="width:30em;display:block">
      <img src="Toolbar_Icons_Stop.png"
           alt="Toolbar_Icons_Stop.png"
           class="md_thumbnail" style="max-width:100%"/>
    </a>

### 创建到Sailfish OS硬件设备的连接

Sailfish SDK还可以将应用程序部署到Sailfish OS硬件设备。此功能要求有效的Sailfish OS硬件设备设置为与计算机的USB或WLAN连接，并确保可以使用密码通过SSH连接到该设备。在SDK中使用Sailfish OS硬件设备作为开发设备时，需要选择有效的工具包（例如 `SailfishOS-armv7hl` 目标）。

Sailfish OS硬件设备设置是使用QT Creator的设备设置完成的。根据您的主机环境，可从菜单 **Tools→Options→Devices** 或 **Qt Creator→Preferences→Devices**中找到。在此设置视图中，单击 **Add…** 开始创建设备的设置。

除非使用一些自定义配置，否则这些默认值可以正常工作。如果您在PC上使用SSH连接时遇到超时，您也可以在创建设备后修改超时设置。现在连接设备，输入您的SSH密码，然后按对话框中所述的“Test Connection”。成功完成后，您可以单击“Next”继续。

<a href="HW_Select.png" style="width:30em;display:block">
    <img src="HW_Select.png"
         alt="HW_Select.png"
         class="md_thumbnail" style="max-width:100%"/>
</a>

在下一个对话框中，您可以查看并进一步调整与连接相关的配置。单击 **Next** 继续。

<a href="HW_Configure.png" style="width:30em;display:block">
    <img src="HW_Configure.png"
         alt="HW_Configure.png"
         class="md_thumbnail" style="max-width:100%"/>
</a>

在下一个对话框中，只需单击**Next** ，除非您想中止设备创建。在这种情况下，单击**Cancel**.

然后，QT Creator应创建设备配置，将SSH密钥部署到设备，最后测试设置。在这些对话框中，用户只能单击**Close** 进入下一阶段。

经过测试和验证后，QT Creator会显示一个视图，其中显示了创建的设备。请注意，配置文件尚未更新，因此如果您不按**OK**或**Apply**，则不会保存更改。

就这样了。现在，QT Creator可以将您的应用程序部署到设备。

### 将ARM套件设置为部署到设备

默认情况下，ARM Kit将创建RPM二进制文件，它甚至不会尝试部署到设备。已选择部署选项**Build RPM Package for Manual Deployment** (构建用于手动部署的RPM包)。这可以从QT Creator的主视图中使用部署选项进行更改，选择**Deploy as RPM Package** (部署为RPM包) 或 **Deploy by Copying Binaries** (通过复制二进制文件部署).

<a href="ARM_Kit_Select_Deploy.png" style="width:30em;display:block">
    <img src="ARM_Kit_Select_Deploy.png"
         alt="ARM_Kit_Select_Deploy.png"
         class="md_thumbnail" style="max-width:100%"/>
</a>

祝贺！现在，您可以构建并部署到Sailfish ARM设备。

### 构建和部署应用

按下 ![QtC_Run_Button.png](QtC_Run_Button.png "QtC_Run_Button.png")
按钮在模拟器上编译和运行项目。

就是它！您刚刚运行了第一个Sailfish OS应用程序。它应该在模拟器中运行，如下所示。

<a href="Emulator_Screenshot_01.png" style="width:30em;display:block">
    <img src="Emulator_Screenshot_01.png"
         alt="Emulator_Screenshot_01.png"
         class="md_thumbnail" style="max-width:100%"/>
</a>

#### 构建用于手动部署的RPM包

默认部署选项是仅构建RPM包。在这种情况下，Run和Debug按钮被替换为单个Deploy按钮，该按钮可用于创建RPM包。或者，菜单**Build → Deploy Project “projectname”** 可用于触发包创建。

后续步骤：探索如何 [使用这个应用程序](/Develop/Apps/Using_Sailfish_OS_Apps).
