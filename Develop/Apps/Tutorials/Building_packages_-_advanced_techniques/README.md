---
title: 构建软件包 - 高级技术
permalink: Develop/Apps/Tutorials/Building_packages_-_advanced_techniques/
parent: 教程
grand_parent: Apps
layout: default
nav_order: 300
---

Sailfish SDK通过Sailfish IDE提供简化的开发人员体验。但是，本机支持仅适用于使用qmake或cmake作为其构建系统的项目，在将现有应用程序移植到Sailfish OS时，尤其是在处理平台组件时，可能不会出现这种情况。这样的项目可以从命令行手动构建，并且通过一个中间步骤，也可以在Sailfish IDE中打开它们，并启用通常的高级编辑功能。对于那些喜欢使用不同的代码编辑环境或希望在持续集成系统的上下文中使用Sailfish SDK的人来说，本文档中描述的技术也很有用。在本文档中，我们使用了一个基于cmake的示例应用程序和一个基于GNUAutomake的库，但对于任何构建设置，要采取的步骤大致相同。

## 准备工作

您需要做的第一件事是安装Sailfish SDK。点击[Sailfish SDK安装](/Tools/Sailfish_SDK/Installation) 页面。接下来，您应该在您的主目录下复制[示例应用程序](https://github.com/sailfishos/cmakesample) 和 [示例库](https://github.com/sailfishos/cmakesample)。 

`sfdk`工具可从Sailfish SDK安装目录的“bin”子目录中获得。对于本指南的其余部分，假设您设置了一个shell别名，以允许在不指定其完整路径的情况下调用“sfdk”。
```nosh
$ alias sfdk=~/SailfishOS/bin/sfdk
```

在这一点上，你应该考虑从它的内置帮助中学习一点`sfdk`，至少通过简单地检查构建软件包的部分。
```nosh
$ sfdk --help-building
```

列出已安装的生成工具并选择要为其生成的生成目标。在上述`sfdk`的内置帮助中了解更多信息。
```
$ sfdk tools list
SailfishOS-4.3.0.12              sdk-provided,latest
├── SailfishOS-4.3.0.12-armv7hl  sdk-provided,latest
└── SailfishOS-4.3.0.12-i486     sdk-provided,latest
```

选择“i486”目标，因为我们将首先在模拟器中运行。
```nosh
$ sfdk config target=SailfishOS-4.3.0.12-i486
```

本指南假定默认情况下不可用的所有构建依赖项都在RPM“spec”文件中声明，以便在构建软件包时自动安装它们。如果需要手动安装更多构建依赖项，可以使用 `sfdk tools package-install` 和相关命令来完成。

## 构建示例应用程序

克隆示例应用程序并使用从单独的构建目录调用的“sfdk build”命令构建它，即执行影子构建。
```nosh
$ git clone https://github.com/sailfishos/cmakesample
$ mkdir cmakesample.build.i486 && cd cmakesample.build.i486
$ sfdk build ../cmakesample
```

构建完成后，您可以在`RPMS` 目录中找到RPM包。

如果您希望单独重复任何构建阶段，可以在构建目录下使用以下命令：
```nosh
$ sfdk cmake ../cmakesample [cmake-options...]
$ sfdk make [make-options...]
$ sfdk build-shell <arbitrary-build-command> [options...]
$ sfdk make-install
$ sfdk package
```

与`sfdk cmake`命令类似，`sfdk qmake` 命令用于基于qmake的项目。与`sfdk make`一起，这些命令允许以仅执行.spec文件的“%build”部分的相应部分的方式运行“rpmbuild”。使用“--help”运行这些命令以了解更多信息。

## 在模拟器中运行示例应用程序

强烈建议您仅以RPM包的形式部署应用程序。以下说明仅适用于这种情况。

列出可用设备以确定与仿真器对应的设备的名称。
```
$ sfdk device list
#0 "Sailfish OS Emulator 4.3.0.12"
    emulator         autodetected  defaultuser@127.0.0.1:2223
    private-key: ~/SailfishOS/vmshare/ssh/private_keys/sdk
```

配置`sfdk`以使用与仿真器对应的设备。
```nosh
$ sfdk config device="Sailfish OS Emulator 4.3.0.12"
```

部署在前面的步骤中构建的包。
```nosh
$ sfdk deploy --sdk
```

这将把从当前项目构建的所有包移动并安装到Sailfish OS模拟器。`deploy`命令接受选择其他部署方法和修改要部署的包列表的选项。有关详细信息，请查看`sfdk deploy--help`。

成功部署后，应用程序图标将出现在菜单中。点击图标启动应用程序。

|<a href="Emulator_Plain.png"><img src="Emulator_Plain.png" alt="Plain emulator" class="md_thumbnail" style="width:30em"/></a>|<a href="Emulator_Installed.png"><img src="Emulator_Installed.png" alt="Emulator with the app installed" class="md_thumbnail" style="width:30em"/></a>|<a href="Emulator_Running.png"><img src="Emulator_Running.png" alt="Emulator with the app running" class="md_thumbnail" style="width:30em"/></a>|
|-|-|-|
|<span class="md_figcaption">Plain emulator</span>|<span class="md_figcaption">Emulator with the app installed</span>|<span class="md_figcaption">运行应用程序的模拟器</span>|

祝贺您，您已成功手动构建并部署了Sailfish OS应用程序。

您还可以从命令行调用应用程序。
```nosh
$ sfdk device exec /usr/bin/cmakesample
```

调试器可以方便地使用
```nosh
$ sfdk debug /usr/bin/cmakesample
```

### 其他部署选项

如果`sfdk`实施的部署方法不适合您的软件包的特定需求，则可以让`sfdk`仅将RPM传输到设备，然后手动部署它们。
```nosh
$ sfdk deploy --manual
```

之后，将RPM复制到设备上的`~/RPMS` 目录中。打开设备的外壳，并使用您喜欢的方法安装RPM。出于本例的目的，我们将简单地使用`rpm -i`.
```nosh
$ sfdk device exec
(device) $ sudo rpm -i RPMS/cmakesample-1.0-1.i486.rpm
```

可替换地，普通`scp` 可用于传输可从除当前构建目录之外的其他位置获得的包。SSH连接参数可以从`sfdk`提供的设备列表中确定。
```
$ sfdk device list
#0 "Sailfish OS Emulator 4.3.0.12"
    emulator         autodetected  defaultuser@127.0.0.1:2223
    private-key: ~/SailfishOS/vmshare/ssh/private_keys/sdk
```

相应的`scp` 命令行为
```nosh
$ scp -P 2223 -i ~/SailfishOS/vmshare/ssh/private_keys/sdk \
    path/to/package.rpm defaultuser@127.0.0.1:
```

## 在设备上运行示例应用程序

首先，您需要使用Sailfish SDK注册您的设备。目前，这只能通过Sailfish OS IDE - [add a new Sailfish OS hardware device](/Develop/Apps/Your_First_App#create-a-connection-to-sailfish-os-hardware-device) 来实现。

然后配置 `sfdk` 以使用已配置的设备。
```
$ sfdk device list
#0 "Sailfish OS Emulator 4.3.0.12"
    emulator         autodetected  defaultuser@127.0.0.1:2223
    private-key: ~/SailfishOS/vmshare/ssh/private_keys/sdk
#1 "Xperia 10 - Dual SIM (ARM)"
    hardware-device  user-defined  defaultuser@192.168.2.15:22
    private-key: ~/.ssh/id_x10

$ sfdk config device="Xperia 10 - Dual SIM (ARM)"
```

如果您从一开始就遵循本指南，那么您之前就构建了用于部署到Sailfish OS模拟器的应用程序。虽然Sailfish OS模拟器是i486目标，但大多数Sailfish OS硬件设备，如本例中使用的Xperia 10，都是ARM目标。因此，需要再次构建应用程序。在[Preparation](#preparation) 部分中，列出已安装的构建工具并选择适当的构建目标，就像我们之前所做的那样。
```nosh
$ sfdk config target=SailfishOS-4.3.0.12-armv7hl
```

为这个目标创建一个构建目录，并再次构建应用程序。
```nosh
$ mkdir cmakesample.build.armv7hl && cmakesample.build.armv7hl
$ sfdk build ../cmakesample
```

在构建目录下发出`sfdk deploy`命令：
```nosh
$ sfdk deploy --sdk
```

现在，您的应用程序已出现在已安装应用程序的列表中，并且已准备好运行。
```nosh
$ sfdk device exec /usr/bin/cmakesample
```

替代部署选项与之前使用模拟器演示的选项基本相同。在硬件设备上，默认情况下不安装`sudo`，可以使用`devel-su` 命令获得超级用户访问权限。

## 使用依赖包

**注意：**本节的部分内容仅适用于SDK\>=3.3

当在一个任务下修改多个包，并且这些包之间存在构建时间依赖关系时，需要确保所需包的更新版本在构建目标下可用。这可以在`sfdk`的 `output-prefix` 和 `search-output-dir` 配置选项的帮助下方便地实现（后者由前者说明）。通过以下配置，生成的RPM将使用一个公共输出目录， `sfdk` 将在解决构建时依赖关系时考虑该目录下的现有软件包。
```nosh
$ mkdir ~/RPMS
$ sfdk config --global --push output-prefix ~/RPMS
```

请参阅`sfdk --help-building` 以了解如何将其与`task` 配置选项相结合，以实现每个任务的输出目录。

稍后，我们将再次使用模拟器，因此，如果您之前切换到ARM目标，请重新使用i486目标。
```nosh
$ sfdk config target=SailfishOS-4.3.0.12-i486
```

克隆并构建示例库。与基于QMake或CMake的项目不同，基于Automake（AutoTools）的项目很少以允许使用`sfdk`. 执行影子构建的方式打包。幸运的是，从这个意义上说，示例库是一个更好的例子，所以让我们练习这个选项。（从`sfdk --help-building`了解有关跟踪构建限制的更多信息。）
```nosh
$ git clone https://github.com/sailfishos/automakesample
$ mkdir automakesample.build.i486 && cd automakesample.build.i486
$ sfdk build ../automakesample
```

现在修改样例应用程序，将构建时依赖项添加到示例库中。

编辑 `main.cpp`，并添加如下代码：
```cpp
#include <automakesample.h>
#include <iostream>
...
std::cout << automakesample::greetings() << std::endl;
```

编辑 `CMakeLists.txt` ，以便在`pkg-config`的帮助下找到并使用`automakesample`库（请参阅此处的`sailfishapp`），并将`BuildRequires:pkgconfig(automakesample)`添加到`rpm/cmakesample.spec`中。

现在构建更新后的示例应用程序。
```nosh
$ cd cmakesample.build.i486
$ sfdk build ../cmakesample
```

您应该在生成输出中的某个位置看到以下消息：
```
The following 2 NEW packages are going to be installed:
  automakesample        0.1-1
  automakesample-devel  0.1-1
```

将示例应用程序和示例库（但不包括开发包）部署到模拟器并运行示例应用程序。应用程序启动后，您应该在控制台输出中看到Hello, World!。
```nosh
$ sfdk deploy --all "-*-devel"
$ sfdk device exec /usr/bin/cmakesample
Hello, World!
```

## 不使用Sailfish IDE编辑代码

**注意：**本节仅适用于SDK\>=3.3

为了成功加载Sailfish项目并为其启用高级代码编辑功能，在特定项目类型的开发环境端必须存在与Sailfish SDK的某种级别的集成。这是必要的，因为构建时涉及的文件系统路径并不总是存在于主机文件系统上-某些路径仅在构建引擎下有效，如`sfdk --help-building`.中详细解释的那样。说到Sailfish IDE，这种集成存在于基于qmake和cmake的项目中。

除了这个特定于Sailfish SDK的问题之外，还存在一个更普遍的问题，即代码编辑器或工具可能无法从特定项目所使用的构建系统中检索所需的信息。这通常是在Clang的JSON编译数据库格式的帮助下解决的，该格式用作可互换的项目描述，适合由工具加载。同样的方法也适用于上面提到的问题，它可以用于任何能够加载编译数据库的开发环境，而不仅仅是Sailfish IDE。

对于在某些时候使用`make` 的构建系统，Sailfish SDK支持使用`sfdk compiledb`命令方便地生成编译数据库。

让我们以上面介绍的基于Automake的示例库为例。在最初构建项目之后（我们之前已经这样做了），生成编译数据库，如果影子构建已经完成，则将结果文件复制（或链接）到源目录中。
```nosh
$ cd automakesample.build.i486
$ sfdk build ../automakesample # not needed if done before
$ sfdk compiledb
$ cp compile_commands.json ../automakesample/
```

现在，基于Automake的项目可以在Sailfish IDE中打开。使用`File > Open File or Project...` 打开源目录下的编译数据库文件（注意不要打开构建目录下的原始文件）。打开项目后，确保

1.  将其配置为使用与构建目标相对应的工具包，该构建目标用于使用“sfdk”构建项目，并且
2.  调整`Projects > Build Settings`下的 `Build directory` 路径。

通过这种配置，还可以直接从Sailfish IDE构建项目，以便更容易导航到构建问题，并且（在应用程序项目的情况下）还可以更新 `Run Settings` 并直接从Sailfish IDE在设备上运行项目。

作为替代编辑环境的示例，请考虑使用安装了YouCompleteMe插件的Vim编辑器。使用YouCompleteMe插件，当正在编辑的文件存在时，Vim会从目录开始在文件系统层次结构中查找`compile_commands.json` 文件，因此此时应已完成所有设置，并且在打开 `lib/automakesample.cpp` 文件后，YouCompleteMe插件提供的所有功能都应可用。

## 编写开发人员友好的RPM规范文件

不需要spec文件中的任何特殊内容来允许使用 `sfdk build` 命令执行（一次）完整的就地构建。需要一些意识来允许更多的开发人员友好的工作流，包括增量（重新）构建和跟踪构建。

1.  构建和安装过程应使用底层构建系统(qmake, CMake, GNU Automake, make, ...) 完全实现。不需要在RPM规范级别执行额外的步骤。这也意味着不应使用`%doc` 宏。
2.  如果构建系统不是qmake或cmake，并且包的构建过程中涉及类似于配置的步骤，则应编写 `%build` 部分，以便在重复的增量构建中跳过此步骤。

下面是编写良好的 `%build` 和 `%install` 下面是编写良好的

使用qmake:
```specfile
%build
%qmake ARGS...
make %{?_smp_mflags}
make %{?_smp_mflags} doc

%install
%qmake_install
```

使用CMake:
```specfile
%build
%cmake ARGS...
make %{?_smp_mflags}
make %{?_smp_mflags} doc

%install
%make_install
```

使用GNU Automake, Autoconf 和其他:
```specfile
%build
if ! test -f Makefile; then
    %reconfigure
fi
make %{?_smp_mflags}
make %{?_smp_mflags} doc

%install
%make_install
```

## 声明依赖关系

如果您的包与其他包之间存在运行时依赖项，则应将这些依赖项存储在包元数据中，以便包管理器在安装您的包时也可以安装这些依赖项。我们可以在.spec文件中控制这些元数据。添加依赖项的简单方法是将requires字段添加到.spec：
```specfile
Requires:   sailfishsilica-qt5 >= 0.10.9
Requires:   libsailfishapp-launcher
```

在许多情况下，这是不必要的：如果您的应用程序链接到Harbour允许的库，或者使用Harbour允许的Python模块，则会自动创建依赖项。但是，此过程并非100%准确，因此可能需要在.spec文件中手动添加或删除依赖项。

如果自动化过程添加了不需要的依赖性，则可以使用 `%define __requires_exclude`为单个依赖性禁用该过程。例如，如果自动依赖生成将不需要的依赖添加到名为 "wrongmodule"的Python模块中，您可以通过编写以下内容来禁用它：
```specfile
%define __requires_exclude python3dist.wrongmodule
```

如果您想完全禁用Python模块的自动依赖生成，您可以添加：
```specfile
%undefine __pythonapp_requires
```

## 正在验证包内容

### 正在运行Harbour校验

如果你想让你的应用程序在[Jolla Store via Harbour](https://harbour.jolla.com/)中可用，它必须通过Harbour验证检查。您可以使用`sfdk`运行Harbour 校验（以及更多）:
```nosh
$ cd cmakesample
$ sfdk build
$ sfdk check
```

也可以验证之前构建的包，即不访问构建树。只需在命令行上传递它们：
```nosh
$ sfdk config target=SailfishOS-4.3.0.12-armv7hl
$ sfdk check ...path/to/harbour-cmakesample-1.0-1.armv7hl.rpm
```
