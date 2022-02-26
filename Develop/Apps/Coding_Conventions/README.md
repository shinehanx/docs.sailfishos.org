---
title: 编码公约
permalink: Develop/Apps/Coding_Conventions/
parent: Apps
layout: default
nav_order: 300
---

# 通用

作为一个经验法则，遵循你想贡献的开源项目的编码惯例。大多数大型项目，如GStreamer、oFono和Telepathy，都有自己的编码约定，在贡献这些框架时，应该尊重这些约定。

# 基于Qt的应用程序和中间件的编码约定

当为Sailfish应用程序开发功能时，请遵循Qt C++（遵循标准C++）和QML的编码惯例。Sailfish在编码风格问题上历来比较灵活，将执行公约的决定权留给各个团队和开发人员。编码规则引起了很多意见，但最终并不那么重要，只要开发者写出清晰、简洁、可读、可测试的高质量代码，他们就可以为之自豪。

## Qt 编码约定

  - [Qt编码风格](http://qt-project.org/wiki/Qt_Coding_Style)
  - [Qt C++ 编码约定](http://qt-project.org/wiki/Coding-Conventions)
  - [Qt API设计原则](http://qt-project.org/wiki/API-Design-Principles)

## QML 编码约定

  - [QML编码约定](http://qt-project.org/doc/qt-5.0/qtquick/qml-codingconventions.html)
  - [QML最佳实践](http://qt-project.org/doc/qt-4.8/qml-best-practices-coding.html)

## 额外的Sailfish Qt编码约定

来源：Sailfish OS中间件公约

### 倾向于Qt5信号/插槽连接语法

推荐
```cpp
QObject::connect(&sender, &Sender::signalName, &receiver, &Receiver::slotName);
```

不推荐
```cpp
QObject::connect(&sender, SIGNAL(signalName()), &receiver, SLOT(slotName()));
```

理由：确保编译器能够检测到参数不匹配的问题，并在运行时提供一个小的性能优势。

### 倾向于C++11 Ranged-For

推荐
```cpp
for (const QString &str : someStringList) { ... }
```

或推荐
```cpp
for (const auto &str : someStringList) { ... }
```

不推荐
```cpp
for (int i = 0; i < someStringList.size(); ++i) {
    const QString &str(someStringList[i]);
    ...
}
```

理由：一般来说，在平台编译器支持的情况下，我们应该更喜欢C++11的特性。

### 使用CamelCase命名空间名称

至于类的名字，更喜欢驼峰格式(camel-cased)的命名空间名称。

推荐
```cpp
namespace AppNamespace { /* ... */ }
```

不推荐
```cpp
namespace appnamespace { /* ... */ }
```

理由：提供了与Qt编码风格所规定的类命名的一致性。

## 额外的Sailfish QML编码惯例

来源：Qt Quick实例，Qt Components公约

### 省略JavaScript函数行后面的";"

推荐
```qml
x = a + b
```

不推荐
```qml
x = a + b;
```

理由：代码较少，基本上两种风格的方式都可以，但选择了一种。

### 在条件语句、代码和大括号之间加上空格

推荐
```qml
if (a) {
```

不推荐
```qml
if(a){
```

也不推荐
```qml
if ( a ) {
```

理由：标准主要是审美方面的，因此需要论证。总的来说，增加空间可以改善视觉分组（但前提是不要过度使用），并有助于可读性。

### 在一行中编写简短的单行函数

推荐
```qml
onClicked: if (a) doSomething()
```

不推荐
```qml
onClicked: {
    if (a) {
        doSomething()
    }
}
```

理由：需要的行数较少，两种形式同样可读。

### 省略多余的属性指定

推荐
```qml
property bool propertyName
property int propertyName
```

不推荐
```qml
property bool propertyName: false
property int propertyName: 0
```

理由：更少的代码，QML开发者应该已经知道默认值。

### 如果未使用ID，则不要为元素定义ID

推荐
```qml
Image {}
```

不推荐
```qml
Image {
    id: background
}
```

如果你不需要id.

理由：可能减少代码。就像属性一样，只有在你真正需要的时候才定义一些东西。

### 在单行条件中使用大括号

推荐
```qml
if (a) {
    doSomething()
}
```

不推荐
```qml
if (a)
    doSomething()
```

理由：与团队达成一致。违反了QT编码规范，但通常被认为更安全。

### 当分组属性表单可用时，将属性分组在一起：font {}, anchors {}, border {}, 等。

推荐
```qml
anchors {
    horizontalCenter: parent.horizontalCenter
    horizontalCenterOffset: Theme.paddingSmall
}
```

不推荐
```qml
anchors.horizontalCenter: parent.horizontalCenter
anchors.horizontalCenterOffset: Theme.paddingSmall
```

原理：较少的代码，分组通信关系。

### 避免不必要的负面影响

代码块更具可读性
```qml
if (a) .. else ..
```

好于
```qml
if (not a) .. else ..
```

以及
```cpp
#ifdef USE_SQL
..
#endif
```

不推荐
```cpp
#ifdef NO_SQL
..
#endif
```

理由：否定需要更多的精力来破译。避免双重否定，如`!NO_SOMETHING`.

### 保持相似的属性彼此靠近

对此没有一个明确的规则，分组通常是上下文相关的：基本元素属性与专用属性，可视属性与非可视属性。

基本原理：接近意味着关系，当您对特定行为/功能感兴趣时，代码越不需要在行与行之间跳转，代码就越容易阅读。

需要考虑的一些粗略的一般规则：
```
id declaration
property declarations
signal declarations
property bindings, functions and signal handlers
 - Same type items grouped together, javascript functions and signals handlers close to each other.
 - Attached properties after bindings.
 - The most important details that define the component first.
   For example if an interactive item sets some text label property, the bindings define
   the item more than a javascript expression blob.
child items, visual and non-visual grouped together
states
transitions
```

### 将私有API标记为private

你不能阻止开发人员阅读你的代码，如果他们不能区分公共符号和私有符号，他们的代码最终将依赖于以后版本中发生的变化。QML不支持私有成员，但是有几个约定可以用来避免符号的干扰。最简单且通常最快的方法是在前面加下划线：
```qml
Item {
    property int iAmPublic
    property int _iAmPrivate

    Text {
        text: "Hello"
    }
}
```

## 测试约定

Sailfish应用开发遵循Qt惯例，自动测试也不例外。

  - [用Qt Test Framework测试Qt C++类](http://doc.qt.io/qt-5/qtest-tutorial.html)
  - [用Qt Quick Test Framework测试Qt QML元素](http://doc.qt.io/qt-5/qttest-qmlmodule.html)

下面突出显示了几个您应该避免的常见反例。

### 尽可能选择compare()而不是verify()

推荐
```qml
compare(value, 10)
```

不推荐
```qml
verify(value == 10)
```

基本原理：Compare生成更多描述性错误消息，并执行更多类型检查。

### 使用signalSpy代替wait()

推荐
```qml
SignalSpy { id: valueSpy; target: object; signalName: "onValueChanged" }
function test_case() {
    signalSpy.clear()
    signalSpy.wait()
    compare(object.value, newValue)
}
```

不推荐
```qml
function test_case() {
    wait(100)
    compare(object.value, newValue)
}
```

理由：太短的任意等待时间可能会失败，在最坏的情况下会毫无规律的失败。等待时间过长会不必要地延迟自动测试的完成。

# Sailfish项目和部署约定

## 应用程序项目层次结构

Sailfish OS系统应用程序的项目层次结构。

| Path                                        | 描述                                                                                                                                |
| ------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| **sailfish-appname**                        | 在空格通常的位置使用破折号"-"                                                                                           |
| **sailfish-appname/sailfish-appname.pro**   | 将Pro文件名与文件夹名匹配
                                                                                               |
| **sailfish-appname/appname.cpp**            | 加载顶级QML文档的C++应用程序存根，不命名为main.cpp，以避免搜索索引中有十几个main.cpp文件 |
| **sailfish-appname/appname.qml**            | 顶级文件夹仅包含一个小写的QML文件                                                                                    |
| **sailfish-appname/pages**                  | 仅包含页面组件的文件夹，这是一种更易于自动测试的约定
                                                                |
| **sailfish-appname/pages/folder/\*.qml**    | 页面使用的子组件
                                                                                                            |
| **sailfish-appname/cover**                  | 包含应用程序的活动封面UI的文件夹
                                                                                            |
| **sailfish-appname/tests/auto/tst_\*.qml**  | 应用程序的自动测试
                                                                                                              |
| **sailfish-appname/tests/benchmarks**       | 测试基准                                                                                                                                 |

## 应用程序部署文件夹

This applies to Sailfish OS system applications.

| Path                                                 | 描述                                                                                                                                           |
| ---------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| **/usr/bin/sailfish-appname**                        | 打开应用程序时执行的二进制文件
                                                                                              |
| **/usr/share/sailfish-appname**                      | QML文档和其他资源，不要将QML组件烘焙成二进制文件，以允许在不重新编译二进制文件的情况下更新单个组件|
| **/usr/share/applications/sailfish-appname.desktop** | 桌面文件                                                                                                                                          |

## Module project hierarchy

| Path                                                   | 描述                                                                     |
| ------------------------------------------------------ | ------------------------------------------------------------------------------- |
| **sailfish-modulename**                                | 在空格通常的位置使用破折号“-”                                                   |
| **sailfish-modulename/sailfish-modulename.pro**        | 将pro文件名与文件夹名匹配                                                       |
| **sailfish-modulename/sailfish-modulename.qmlproject** | 可选                                                                            |
| **sailfish-modulename/modulename**                     | 模块公开的QML组件和C++元素                                                      |
| **sailfish-modulename/modulename/modulename.pro**      | 模块的编译和部署规则                                                            |
| **sailfish-modulename/modulename/qmldir**              | QML模块定义文件                                                                 |
| **sailfish-modulename/modulename/\*.qml**              | qmldir中提到的公共QML组件                                                       |
| **sailfish-modulename/modulename/private**             | 内部非公共组件                                                                  |
| **sailfish-modulename/modulename/src**                 | C++代码在这里                                                                   |
| **sailfish-modulename/modulename/src/src.pri**         | 在此列出C++头文件和源文件                                                       |
| **sailfish-modulename/modulename/src/\*.h/cpp**        | 向QML公开的C++类                                                                |
| **sailfish-modulename/modulename/src/modulename.cpp**  | 不要使用plugin.CPP，以避免在搜索索引中出现大量plugin.cpp文件                    |
| **sailfish-modulename/applications**                   | 展示模块提供的组件的示例应用程序                                                |
| **sailfish-modulename/tests/auto/tst_\*.qml**          | 模块的自动测试                                                                  |
| **sailfish-modulename/tests/benchmarks**               | 模块的基准测试                                                                  |
| **sailfish-modulename/doc**                            | 文档                                                                            |

## Module deployment folders

| Path                                                            | 描述                                       |
| --------------------------------------------------------------- | ------------------------------------------ |
| **/usr/lib/qt5/qml/Sailfish/Modulename**                        | 使用Java样式的名称间距                     |
| **/usr/lib/qt5/qml/Sailfish/Modulename/qmldir**                 | QML模块定义文件                            |
| **/usr/lib/qt5/qml/Sailfish/Modulename/\*.qml**                 | 公共QML组件                                |
| **/usr/lib/qt5/qml/Sailfish/Modulename/private**                | 内部非公共组件                             |
| **/usr/lib/qt5/qml/Sailfish/Modulename/libmodulenameplugin.so** | 向QML公开C++元素的二进制插件               |
