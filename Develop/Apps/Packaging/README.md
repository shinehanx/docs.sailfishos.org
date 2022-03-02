---
title: 打包
permalink: Develop/Apps/Packaging/
parent: Apps
layout: default
nav_order: 500
---

## 包装应用程序

Sailfish OS的应用程序在提交给Jolla Harbour之前必须被打包成一个安装包，以便在Jolla Store中提供。本文简要介绍了可用的部署方法，并解释了为应用程序项目创建安装包的步骤。

### 部署方法

将应用程序部署到Sailfish OS模拟器或设备上是将应用程序二进制文件和任何所需的资源文件（如QML文件、图像、桌面集成文件等）转移到目标执行环境的过程。有两种部署应用程序的主要方法：直接复制二进制文件和创建一个包含文件的RPM安装包。你可以通过使用Sailfiish IDE中的项目套件选择器来选择部署方法。

<a href="QtC_Deployment_Method.png" style="width:30em;display:block">
    <img src="QtC_Deployment_Method.png"
         alt="QtC_Deployment_Method.png"
         class="md_thumbnail" style="max-width:100%"/>
</a>

无论应用程序是在模拟器上还是在设备上运行，这两种部署方法都有效。然而，每种方法都以不同的方式进行部署，并有不同的优点和缺点。

顾名思义，复制二进制文件通过简单地将文件复制到目标环境中的正确位置来传输二进制和资源文件。这种方法很快速，因为它不需要其他步骤，对环境的改变也很小。二进制拷贝部署是日常开发的一个好选择。

使用RPM包部署方法，首先创建一个包含应用程序二进制文件和资源文件的RPM包。之后，RPM包被传输到目标环境，并使用本地RPM安装工具进行安装。

这种方法比二进制拷贝方法稍慢，因为它涉及到中间步骤，但有几个好处。除了简单地包含要安装的文件外，RPM包可以描述包的依赖关系。这允许在你的应用程序安装时自动安装你的应用程序所依赖的任何包。

由于在向Jolla Harbour提交应用程序时没有模拟器或设备可以安装，因此RPM部署是唯一可用的选择。此外，RPM部署方法比二进制拷贝更接近于模仿最终用户的安装体验。正因为如此，最好不时地测试将你的应用程序作为RPM包进行部署，以确保任何包的依赖性得到正确解决。

还请注意，工具包的选择决定了软件包可以在哪里部署。X86软件包在模拟器上运行，而只有ARM软件包可以安装在设备上。用于Jolla Harbour的软件包显然是要安装在真实设备上的，因此必须使用`SailfishOS-<version>-armv7hl`套件进行编译。

本文的其余部分说明了为你的应用项目创建RPM安装包时发生的过程。

### 安装包创建概述

除了标准的Qt项目文件外，其他各种文件在描述进入RPM包的文件以及包在安装到目标环境时应该如何表现方面起着作用。下图显示了创建一个RPM包所涉及的文件。

<a href="RPM_Creation.png" style="width:30em;display:block">
    <img src="RPM_Creation.png"
         alt="RPM_Creation.png"
         class="md_thumbnail" style="max-width:100%"/>
</a>

如果你在Sailfish IDE中使用Sailfish OS Qt Quick应用程序模板来创建你的项目，所有这些文件都会自动为你创建。大多数文件是由新项目向导生成的，但有些文件是在构建或部署项目时生成的。下面的章节使用一个用Sailfish OS Qt Quick应用程序模板创建的名为 "myfirstapp "的项目来解释这些文件的作用。

### .pro和.prf文件

使用Sailfish IDE新项目向导创建了一个项目后，`myfirstapp.pro`文件看起来像这样：
```qmake
TARGET = myfirstapp
CONFIG += sailfishapp
SOURCES += src/myfirstapp.cpp
OTHER_FILES += qml/myfirstapp.qml \
 qml/cover/CoverPage.qml \
 qml/pages/FirstPage.qml \
 qml/pages/SecondPage.qml \
 rpm/myfirstapp.spec \
 rpm/myfirstapp.yaml \
 myfirstapp.desktop
```

Qt项目文件列出了构成你的应用程序的文件。当然，项目文件中的许多项目，如编译的目标`myfirstapp`和所有的QML文件，应该成为安装包的一部分。但是，如果你认为项目文件本身没有任何东西似乎表明哪些文件应该被安装在哪里，你是对的。

因为所有基于Sailfish OS Qt Quick应用程序模板的项目都有一个相同的结构，指导安装的项目文件变量声明已经被分离到一个共享的Qt特性文件中。

`CONFIG += sailfishapp`声明指示qmake将Qt特性文件`sailfishapp.prf`中的声明纳入该项目文件。

Qt特性文件（`.prf`文件）与项目包含（`.pri`文件）文件类似，它们允许将一组共享的声明包含在多个项目文件中。Qt特性文件，顾名思义，是用来为所有基于Qt的项目提供模块化特性的，而项目包含文件则更常用于将一个大项目分割成更小、更容易管理的部分。还要注意的是，对于Qt特性，你只需要知道你想使用的特性的名称（这里我们说明我们的项目是一个Sailfish OS应用程序，因此我们要所有适用于特性`sailfishapp`的声明）。特性的名称总是特性文件的主干，即`.prf`后缀之前的所有内容。对于项目包含文件，你通常会给出你要包含的项目包含文件的路径和名称。

如果你对`sailfishapp.prf`特性文件的内容感到好奇，对于模拟器目标来说，它可以在以下目录中找到。
```
# Linux和OSX
~/SailfishOS/mersdk/targets/SailfishOS-i486/usr/share/qt5/mkspecs/features

# Windows
C:\SailfishOS\mersdk\targets\SailfishOS-i486\usr\share\qt5\mkspecs\features
(假设你在默认位置安装了Sailfish SDK)。
```

该文件看起来像这样:
```qmake
QT += quick qml

target.path = /usr/bin

!sailfishapp_no_deploy_qml {
    qml.files = qml
    qml.path = /usr/share/$${TARGET}

    INSTALLS += qml
}

desktop.files = $${TARGET}.desktop
desktop.path = /usr/share/applications

icon.files = $${TARGET}.png
icon.path = /usr/share/icons/hicolor/86x86/apps

INSTALLS += target desktop icon

CONFIG += link_pkgconfig
PKGCONFIG += sailfishapp
INCLUDEPATH += /usr/include/sailfishapp

QMAKE_RPATHDIR += /usr/share/$${TARGET}/lib

OTHER_FILES += $$files(rpm/*)
```

这些声明是所有Sailfish OS Qt Quick应用程序所共有的。`INSTALLS`声明列出了项目中需要安装的部分，这里是编译后的目标，整个`qml`目录，桌面集成文件，以及应用程序的图标。

`INSTALLS`声明右边的值指的是内置目标安装配置和上面定义的三个自定义安装配置。例如，对于桌面集成文件，声明了一个名为 desktop 的安装集。
```qmake
desktop.files = $${TARGET}.desktop
desktop.path = /usr/share/applications
```

安装集的 `files` 成员列出了要安装的文件。这可以是一个或多个文件或目录，顺序不限。这里要安装的文件名是`myfirstapp.desktop`，因为`myfirstapp.pro`定义了目标的名称为myfirstapp。安装目标的路径成员定义了`files`成员所列出的文件被复制的`路径`。

对于一个桌面Qt项目来说，安装通常是通过运行`qmake`和make install来进行的。这对Sailfish OS项目来说也是如此，但如果你在部署或运行一个项目后看一下Sailfish IDE的编译输出窗格，你会发现实际的命令是`make install INSTALL_ROOT=/home/deploy/installroot`。

路径`/home/deploy/installroot`指的是构建引擎虚拟机中的一个位置。这不是最终的安装位置，而是RPM构建步骤寻找文件的位置。然而，"INSTALL_ROOT "路径下的文件和目录是完全按照".prf "文件中的安装集来组织的。

现在RPM包的创建已经准备就绪，`.yaml`文件将在下一步的过程中发挥作用。

###关于Qt特性文件的路径

请注意，虽然你可以检查Sailfish SDK安装目录下的`.prf`文件，但这些文件实际上并不是你构建项目时使用的特性文件。因为构建是由构建引擎虚拟机执行的，`.prf`文件实际上在虚拟机的Scratchbox 2环境中。然而，特征文件本身在这两个地方是相同的。

要看到这一点，请进入构建目标，在`/usr/share/qt5/mkspecs/features`目录下找到特性文件。
``nosh
$ sfdk tools exec <target> ls /usr/share/qt5/mkspecs/features
```

### .yaml文件

要创建一个RPM包，你需要一个`.spec`文件，提供关于被打包的软件的信息。对于Sailfish OS项目，你通常不会直接创建或修改`.spec`文件。取而代之的是一个更容易被人阅读的中间元数据文件，即`.yaml'文件，被使用。`.spec`文件本身是在构建过程中从`.yaml`文件自动生成的。

当你第一次在Sailfish IDE下使用Sailfish OS Qt Quick应用程序模板创建项目时，会生成一个初始的`.yaml'文件。你可以从安装包创建概述部分的图中看到，`.pro`文件作为`.yaml`文件的数据源发挥作用。由于这个原因，每次Qt项目文件改变时，`.yaml`文件的部分都会重新生成。

示例项目的初始`.yaml`文件看起来像这样:
```yaml
Name: MyFirstApp
Summary: My Sailfish OS Application
Version: 0.1
Release: 1
Group: Qt/Qt
URL: http://example.org/
License: LICENSE
Sources:
- '%{name}-%{version}.tar.bz2'
Description: |
  Short description of my Sailfish OS Application
Configure: none
Builder: qmake5
PkgConfigBR:
- sailfishapp >= 0.0.10
- Qt5Core
- Qt5Qml
- Qt5Quick
Requires:
- sailfishsilica-qt5 >= 0.10.0
Files:
- '%{_datadir}/icons/hicolor/86x86/apps/%{name}.png'
- '%{_datadir}/applications/%{name}.desktop'
- '%{_datadir}/%{name}/qml'
- '%{_bindir}'
```

除了作为生成的".spec "文件的来源，".yaml "用于声明如何解决项目的构建时和运行时的依赖性。这些依赖关系是由下面描述的关键字控制的。

有几个关键字可以控制`.spec`文件的生成方式。接下来将介绍其中最重要的几个。

#### Name（必选）

你的项目的名称。注意，这不是应用程序的名称，因为它显示在用户面前。相反，它是用来形成`.yaml`文件中其他地方的文件名的基本名称。例如，应用程序的图标被称为`%{name}.png`。因此，Name的值应该总是与Qt项目文件中的`TARGET`声明相匹配。

#### Summary (必选)

对软件包的简短描述。

#### Version (必选)

包的版本号。

当设置为`0`（零）且项目位于Git工作目录下时，实际的版本号将由当前分支上的最新Git标签的名称以编程方式确定。此外，如果当前的HEAD、索引或工作树与标签所表示的树不同，由当前分支名称、时间戳和提交SHA1组成的后缀将被添加到包的版本中。如果git-state不干净，将创建一个git-stash，其SHA1将被用来代替HEAD。

#### Group (必选)

安装的应用程序应该出现在系统的应用程序启动器中的组。对于Sailfish OS的Qt Quick应用程序，该值应始终为`Qt/Qt`。

#### License (必选)

软件包所遵守的许可证名称。

#### Description (可选)

对软件包的较长形式的描述。描述 "中的管道字符。|"表示换行符在后面的描述文本中很重要，不应折叠成空格。

#### Builder (可选)

构建此项目应使用的工具。有效值的例子是`qmake5`和`cmake`。

#### PkgConfigBR (可选)

这个关键字是定义构建你的项目需要哪些包的方法之一。为*PkgConfigBR*关键字列出的值是软件包配置的名称，或者说是`.pc`文件。与Qt特性文件类似，如果你知道软件包配置文件的名称，你就不需要知道提供功能的软件包的确切名称。更详细的讨论请参见下面的关于PkgConfigBR和PkgBR关键字。

#### Requires (可选)

*Requires*关键字指定了应用程序在运行时需要的包。如果这些包在目标环境中不存在，它们将在安装应用程序的RPM包时自动安装。

通常，*Requires*列出的包提供了可导入的QML模块，例如，`sailfishsilica-qt5`包使应用程序的QML文件通过`import Sailfish.Silica 1.0`语句使用Sailfish Silica模块。

找出哪个包提供了哪个QML模块可能需要一点侦查工作，请看技巧和窍门部分，了解一些关于如何找到你需要的包的提示。

#### Files（可选）

*Files* 关键字列出了安装软件包时被复制到系统中的文件和目录。本节中列出的每个文件和路径都是指`sailfish.prf`文件中`INSTALLS`声明中的一个项目。

注意，`.prf`文件使用Qt项目变量，`.yaml`文件使用`.spec`文件宏。在`.prf`文件中对项目的qml目录的声明：
```qmake
qml.files = qml
qml.path = /usr/share/$${TARGET}
```

对应于行 - `'%{_datadir}/%{name}/qml'`在`.yaml`文件中。

在*Files*部分的项目数量必须与`INSTALLS`声明中的项目数量相匹配。如果不是这样，你要么试图安装一些首先没有生成的东西，要么忘记安装一些已经生成的东西，即你在*Files*部分的行数多于`INSTALLS`声明中的项目，或者反过来。这两种情况实际上都是错误，导致在试图部署项目时出现错误信息。

注意，你应该总是使用宏，例如`%{_bindir}`代表`/usr/bin`，在`.yaml`文件中引用标准平台位置。如果你不这样做，会在编译输出中打印一个警告。

看起来很奇怪，路径在Qt特性文件中写出来，但在`.yaml`中却用一个宏来引用。但这两个文件实际上都是由SDK提供的--Qt特性文件是SDK的一部分，而`.yaml`文件中的那一行是由Sailfish IDE生成的。因此，如果标准路径发生变化，这些路径也不会脱节，因为SDK更新时，这两个路径都会发生变化。当你修改`.yaml`文件时，使用宏可以确保你的项目在标准路径改变的情况下继续工作，不需要做任何修改。

#### 关于PkgConfigBR和PkgBR关键字

除了*PkgConfigBR*之外，*PkgBR*关键字也被用来管理构建时的依赖关系。对于这两者来说，"BR "代表的是构建需要。不同的是，使用*PkgConfigBR*时，你要给出软件包配置的名称，而使用*PkgBR*时，你必须知道提供上述配置的软件包的确切名称。例如，下面两个`.yaml`片段都为`sailfishapp`配置建立了一个构建时间要求。
```yaml
PkgBR:
- libsailfishapp-devel >= 0.0.10
```

```yaml
PkgConfigBR:
- sailfishapp >= 1.0.0
```

大于或等于符号可以用来建立一个最小的版本要求。请注意*PkgBR*使用软件包的版本，而*PkgConfigBR*指的是软件包配置的版本，即由`.pc`文件本身中的版本字段指定的版本号。

在适用的情况下，最好使用*PkgConfigBR*，因为它使你的项目不受可能的软件包命名的影响。即使提供配置的软件包被重新命名，也可以用相同的名称来引用配置。

### .spec文件

.spec "文件包含了构建项目和将生成的二进制文件打包成RPM包的说明。该文件是在构建过程中从`.yaml`文件自动生成的，通常不需要手动修改`.spec`。每个影响生成文件的值通常都可以在其他地方设置，例如通过`.yaml`文件。

如果你看一下`.spec`文件（它可以在项目至少建立一次后在rpm目录中找到），你会发现它包含了许多在`.yaml`中设置的值，比如包的*Version*和*Requires*声明。

如果源文件`.yaml`发生变化，大部分`.spec`文件会在构建过程中被覆盖。因此，除了明确标记为可编辑的部分外，手工编辑`.spec`没有什么意义。

`.spec`的可编辑部分用`# >>`和`# <<`标记。对于构建过程的每个步骤，都有可定制的部分的标记。例如，标记的内容有
```specfile
# >> build pre
# << build pre
```

```specfile
# >> build post
# << build post
```

表示在项目编译前后执行的自定义命令的点。

对于大多数Sailfish OS Qt Quick应用程序，没有必要以任何方式改变生成的".spec "文件。生成的文件可以满足绝大多数应用程序的需要，但对于少数可能需要自定义步骤的特殊情况，则可以选择自定义。

### 技巧和窍门

本节显示了一些终端命令，这些命令可能对追踪特定的软件包或名称很有用，因为它们应该出现在`.yaml`文件的*PkgBR*、*PkgConfigBR*和*Requires*部分。所有的命令都应该在使用`sfdk tools exec`构建目标下运行。

注意`zypper`软件包管理器在模拟器和真实设备的默认配置中都不可用。在仿真器虚拟机或设备上的对应命令是`pkcon`。这两个命令的选项略有不同，其中一个可能不支持另一个的所有功能，但两者可以在一个系统上共存 - `zypper`可以用`pkcon install zypper`来安装。

列出所有可用的软件包配置（用于*PkgConfigBR*关键字）。
``nosh
$ pkg-config --list-all
```

列出已安装的软件包配置文件。
``nosh
$ ls -l /usr/lib/pkgconfig/
```

查看全部细节，如版本、包含路径和链接的库，例如`sailfishapp`包的配置。
```nosh
$ cat /usr/lib/pkgconfig/sailfishapp.pc
```

找出哪个软件包提供了`Qt5Positioning`配置。
``nosh
zypper what-provides 'pkgconfig(Qt5Positioning)' 。
```

显示所有软件包（状态栏中的'i'表示该软件包目前已安装）。
``nosh
$ zypper packages
```

显示已安装的软件包`qt5-positioning-devel`中包含的文件。
``nosh
$ rpm -ql qt5-qtpositioning-devel
```
