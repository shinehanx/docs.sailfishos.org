---
title: 代码演练
permalink: Develop/Apps/Code_Walkthrough/
parent: Apps
layout: default
nav_order: 400
---

## 代码演练

该模板包含了一个简单的Sailfish OS应用程序的基本内容。我们将通过代码来帮助理解这些要点。

当基于Sailfish OS应用程序模板生成一个新项目时，Qt Creator项目中的一些文件是以项目名称命名的。在下面的讨论中，以项目名称 "myfirstapp "为例。

### 应用程序条目
```
=> src/myfirstapp.cpp
```

每个Sailfish应用程序都必须定义一个简单的Qt C++应用程序项目，创建一个QQuickView，并实例化一个QML文件，将ApplicationWindow作为顶级项目。然而，除了实现QML文件本身之外，你不需要做任何其他事情来完成这个任务。

假设你把你的项目命名为 "myfirstapp"，Sailfish OS应用程序模板会生成源代码文件src/myfirstapp.cpp，为你完成繁重的工作。这个文件实现了你的应用程序的入口，只需将参数计数和参数数组传递给函数SailfishApp::main()。这个函数依次创建所需的QGuiApplication和QQuickView实例并加载你的主QML文件。

请注意，QML文件的名称实际上并没有被传递给SailfishApp::main()。相反，该函数期望QML文件的名称是基于你的TARGET名称。同样，如果你的项目名称是 "myfirstapp"，Qt项目文件将包含声明TARGET=myfirstapp，而main()将加载QML文件qml/myfirstapp.qml。

应用程序模板为你创建了QML文件，但你应该注意的是，如果不更新.pro文件中的TARGET定义，该文件就不能被重命名。
```cpp
#ifdef QT_QML_DEBUG
#include
#endif

#include


int main(int argc, char *argv[])
{
    // SailfishApp::main() will display "qml/myfirstapp.qml", if you need more
    // control over initialization, you can use:
    //
    //   - SailfishApp::application(int, char *[]) to get the QGuiApplication *
    //   - SailfishApp::createView() to get a new QQuickView * instance
    //   - SailfishApp::pathTo(QString) to get a QUrl to a resource file
    //
    // To display the view, call "show()" (will show fullscreen on device).

    return SailfishApp::main(argc, argv);
}
```

### 顶层的QML文件
```
=> qml/myfirstapp.qml
```

所有应用程序使用的QML文件都在目录qml或其子目录下。当应用程序启动时，SailfishApp::main()函数首先加载QML文件qml/myfirstapp.qml。
```qml
import QtQuick 2.0
import Sailfish.Silica 1.0
import "pages"
```

前两条import语句允许应用程序导入我们以后要使用的Qt Quick和Sailfish Silica模块。此外，最后一条import语句使pages目录下的QML文件对myfirstapp.qml可用。
```qml
ApplicationWindow
{
    initialPage: Component { FirstPage { } }
    cover: Qt.resolvedUrl("cover/CoverPage.qml")
}
```

ApplicationWindow是所有Sailfish Silica应用程序的顶级类型。initialPage属性指定了应用程序被打开时要显示的第一页。cover属性设置应用程序被推到后台时要显示的活动封面。

### 应用程序的第一页
```
=> qml/pages/FirstPage.qml

```
```qml
Page {
    id: page
```

在这里，我们首先创建一个Page对象，它只是一个页面内容的容器。为了实现下拉菜单，我们需要创建一个可弹出的项目，把它的孩子放在一个可以被拉动和弹出的表面。我们使用SilicaFlickable组件来做到这一点。Sailfish Silica提供了SilicaFlickable、SilicaListView和SilicaGridView的类型，它们是Qt Quick flickable、list view和grid view类型的风格化版本。
```qml
SilicaFlickable {
        anchors.fill: parent
```

接下来，我们添加一个带有一个菜单项的下拉菜单，标记为 "Show Page 2"。然后我们给MenuItem附加一个onClicked动作，它将把第二页推到pageStack的顶部（由ApplicationWindow提供）。注意，PullDownMenu和PushUpMenu必须总是嵌套在SilicaFlickable、SilicaListView或SilicaGridView内。
```qml
        PullDownMenu {
            MenuItem {
                text: "Show Page 2"
                onClicked: pageStack.push(Qt.resolvedUrl("SecondPage.qml"))
            }
         }
```

我们将SilicaFlickable的高度设置为与它的子项 "column.height"的高度相同（见下段）。
```qml
contentHeight: column.height
```

最后，我们垂直地安排内容。栏目元素定位它的子项目，使它们在垂直方向上对齐，不重叠。我们还使用PageHeader元素提供页面标题/页眉。页面标题总是放置在内容的顶部。我们使用Label元素为我们的页面添加欢迎文字。
```qml
        Column {
            id: column

            width: page.width
            spacing: Theme.paddingLarge
            PageHeader {
                title: "UI Template"
            }
            Label {
                x: Theme.paddingLarge
                text: "Hello Sailors"
                color: Theme.secondaryHighlightColor
                font.pixelSize: Theme.fontSizeExtraLarge
            }
        }
```

列距、标签X、颜色和font.pixelSize属性使用来自Theme类型的值，而不是硬编码的尺寸或颜色。这确保了应用程序适应当前活动的主题，并且不会与系统提供的组件发生冲突。

### 应用程序的第二页
```
=> qml/pages/SecondPage.qml
```

第二个页面非常简单。它声明了SilicaListView，它有一个模型来定义要显示的数据和一个委托delegate来定义数据的每个索引应该如何显示。
```qml
Page {
    id: page
    SilicaListView {
        id: listView
        model: 20
        anchors.fill: parent
        header: PageHeader {
            title: "Nested Page"
        }
        delegate: BackgroundItem {
            id: delegate

            Label {
                x: Theme.paddingLarge
                text: "Item " + index
                anchors.verticalCenter: parent.verticalCenter
                color: delegate.highlighted ? Theme.highlightColor : Theme.primaryColor
            }
            onClicked: console.log("Clicked " + index)
        }
        VerticalScrollDecorator {}
    }
}
```

### 应用程序的封面
```
=> qml/cover/CoverPage.qml
```

封面是显示在运行中的应用程序屏幕上的背景应用程序的可视化表示。我们用一个CoverBackground元素创建一个封面，里面有一个居中的标签。
```qml
CoverBackground {
    Label {
        id: label
        anchors.centerIn: parent
        text: "My Cover"
    }
```

一个封面可以指定一个可以在后台应用程序上执行的动作列表。这个列表是用CoverActionList元素定义的。每个CoverActionList最多可以定义两个CoverAction项目来指定动作。
```qml
CoverActionList {
        id: coverAction

        CoverAction {
            iconSource: "image://theme/icon-cover-next"
        }

        CoverAction {
            iconSource: "image://theme/icon-cover-pause"
        }
    }
```

在上面的片段中，我们使用两个图标在封面上显示下一个和暂停的动作。你也可以给这些动作附加信号处理程序，以执行特定的功能，虽然我们不会在本教程中详细介绍，但你可以尝试在第一个CoverAction中添加onTriggered: console.log("Cover next")。请确保停止、重建并重新运行你的应用程序以查看结果。

### 关于Sailfish Silica组件

值得注意的是，Sailfish Silica QML组件被设计成可以一起使用。虽然它可能不是很明显，但几个Silica组件合作提供了特定平台的功能。

可编辑的文本字段就是其中的一个例子。TextField是Silica组件，它提供了一个单行可编辑文本字段。然而，还有其他几种类型对可编辑文本字段的行为方式有影响。

如果TextField不在Silica页面内，文本字段可能会被虚拟键盘遮住。页面类型确保页面内容被滚动，以便在虚拟键盘显示时保持可编辑的文本字段可见。同样地，如果QML应用程序的根元素不是Silica ApplicationWindow，虚拟键盘的背景就不会被正确地样式化，从而使其难以阅读按键。

如果你遇到你的应用程序的行为与其他Sailfish OS应用程序不同的情况，你的第一个行动应该是验证你是否在无意中使用了标准的Qt Quick组件，而更适用的Silica组件是可用的。如果你要把现有的QML应用程序移植到Sailfish OS，这很可能是一个问题。

### 本项目中的其他文件和目录

你会注意到，除了.qml和.cpp文件外，还有一些我们到目前为止还没有接触过的文件。在你开始探索SDK之前，让我们快速浏览一下它们。

`myfirstapp.desktop`是一个标准的Linux桌面配置文件，它描述了一个特定的程序如何被启动，如何出现在菜单中，等等。例如，这个文件指定了你的应用程序的名称和图标，因为它们出现在启动器中。关于桌面入口规范的更多信息可以在standards.freedesktop.org找到。

`myfirstapp.png`是应用程序的启动器图标。.desktop文件中的Icon声明（例如Icon=myfirstapp一行）指的是这个图像文件。应用程序模板负责将图标部署到正确的位置，在.desktop文件中的Icon声明应该总是以它的基本名称而不是后缀来指代该文件。与主QML文件类似，图标的名字是基于项目文件中的TARGET声明。因此，除非TARGET声明也被更新，否则图标文件的名称不应该被改变。

`myfirstapp.pro`是一个项目文件，将源代码和生成的应用程序二进制文件联系在一起。该文件描述了如何通过创建Makefile到适当的构建目录来构建应用程序，然后执行该文件来实际构建应用程序。项目文件中的CONFIG += sailfishapp声明使你的项目与libsailfishapp库链接，该库提供了本教程开头讨论的SailfishApp::main()函数的实现。CONFIG声明还确保应用程序的二进制文件和数据文件被部署到模拟器和设备上的适当位置。关于项目文件格式的更多信息，请参见qmake手册。

qml目录包含了应该作为你的应用程序的一部分而部署的文件。除了QML文件外，应用程序使用的音频、图像和JavaScript文件也应该放在这个目录或其中的一个子目录中。基本上，当你的应用程序被安装在模拟器或设备上时，你放在qml目录中的任何东西都会被部署。

### 总结

创建直观的Sailfish Silica用户界面应用程序是很简单的。事实上，它们是基于QML，一种声明性的UI语言，使这个过程快速而简单。它带来了丰富的用户界面元素和大量的可能性。现在你可以探索和学习更多关于如何在你的应用程序中使用它们。请查看SDK中提供的媒体库示例代码，以获得更多的想法。

接下来，[打包应用程序](/Develop/Apps/Packaging)。
