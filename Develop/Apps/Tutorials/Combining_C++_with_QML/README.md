---
title: 将C++与QML结合起来
permalink: Develop/Apps/Tutorials/Combining_C++_with_QML/
parent: 教程
grand_parent: Apps
layout: default
nav_order: 100
---

QML是为Sailfish OS开发应用程序的首选方式。然而，在很多情况下，这本身是不够的，有必要将其放入本地代码。常见的原因包括性能，利用现有的C/C++库等等。本教程描述了如何创建一个结合QML前端和C++后端的应用程序。我们将创建一个简单的应用程序，显示一个不寻常的动物列表。点击任何动物都会使它移到列表的顶部。

|<a href="Screenshot.png" style="width:30em;display:block"><img src="Screenshot.png" alt="Screenshot" class="md_thumbnail" style="max-width:100%"/></a>|
|-|
|<span class="md_figcaption">屏幕截图</span>|

结合C++和QML需要三个不同的步骤。首先，我们需要创建一个数据模型，然后我们需要把它暴露给QML引擎，最后我们需要对它调用方法。让我们详细了解一下这些步骤。这个例子的全部源代码可以从[这个资源库](https://github.com/sailfishos/cppqml-sample)查到。

## 创建模型

将数据从C++暴露在QML中是通过[Qt Model/View框架](http://doc.qt.io/qt-5/model-view-programming.html)完成的。我们的模型是一个简单的字符串数组。Qt为这种使用情况提供了一个[QStringListModel](http://doc.qt.io/qt-5/qstringlistmodel.html)，但出于教育目的，我们将通过继承[QAbstractListModel](http://doc.qt.io/qt-5/qabstractlistmodel.html)提供我们自己的模型。该模型头文件的相关部分看起来像这样。
```cpp
class DemoModel : public QAbstractListModel
{
    Q_OBJECT
public:
    enum DemoRoles {
        NameRole = Qt::UserRole + 1,
    };

    explicit DemoModel(QObject *parent = 0);

    virtual int rowCount(const QModelIndex&) const { return backing.size(); }
    virtual QVariant data(const QModelIndex &index, int role) const;

    QHash<int, QByteArray> roleNames() const;

    Q_INVOKABLE void activate(const int i);

private:
    QVector<QString> backing;
};
```

有几件事情需要注意。第一个是 "backing "变量，它保存了我们想要显示给用户的动物列表。第二个是我们想从QML调用的自定义方法`activate'。为了使它可以被调用，我们需要用`Q_INVOKABLE`宏来标记它。在实现文件中，我们首先要告诉Qt我们的数据元素是什么样子的。这很简单，因为我们只有一个数据要显示，就是动物的名字。
```cpp
QHash<int, QByteArray> DemoModel::roleNames() const {
    QHash<int, QByteArray> roles;
    roles[NameRole] = "name";
    return roles;
}
```

如果我们的数据更复杂，我们会在这里定义更多的角色。例如，一个显示人名的列表可以有两个不同的角色：一个给定的名字角色和一个姓氏角色。获得数据显示的后半部分是返回给定角色数据的函数。这是由Qt的模型系统指定的，它看起来像这样。
```cpp
QVariant DemoModel::data(const QModelIndex &index, int role) const {
    if(!index.isValid()) {
        return QVariant();
    }
    if(role == NameRole) {
        return QVariant(backing[index.row()]);
    }
    return QVariant();
}
```

这很简单。如果`index`是有效的，并且角色是正确的，只需返回一个`QVariant`中的名字。否则返回一个空的`QVariant'。我们的最后一个方法是在一个项目被激活时被调用的。
```cpp
void DemoModel::activate(const int i) {
    if(i < 0 || i >= backing.size()) {
        return;
    }
    QString value = backing[i];

    // Remove the value from the old location.
    beginRemoveRows(QModelIndex(), i, i);
    backing.erase(backing.begin() + i);
    endRemoveRows();

    // Add it to the top.
    beginInsertRows(QModelIndex(), 0, 0);
    backing.insert(0, value);
    endInsertRows();
}
```

这个操作非常简单，我们只是把项目从给定的位置移除，然后插入到顶部。然而，我们需要在修改的过程中调用begin和end方法。这些调用通知Qt，我们的模型的状态将会改变，它应该更新所有显示这个模型数据的视图。我们不需要在QML方面做任何事情来更新显示，Qt会照顾所有的细节。

## 将模型暴露给QML

一个C++模型本身并不是很有用，还需要有一种方法来从QML创建一个模型。这是通过将新的对象类型暴露给QML引擎来实现的，这可以在应用程序启动时轻松完成。
```cpp
int main(int argc, char *argv[])
{
    // Set up QML engine.
    QScopedPointer<QGuiApplication> app(SailfishApp::application(argc, argv));
    QScopedPointer<QQuickView> v(SailfishApp::createView());

    // If you wish to publish your app on the Jolla harbour, it is recommended
    // that you prefix your internal namespaces with "harbour.".
    //
    // For details see:
    // https://harbour.jolla.com/faq#1.5.0
    qmlRegisterType<DemoModel>("com.example", 1, 0, "DemoModel");

    // Start the application.
    v->setSource(SailfishApp::pathTo("qml/cppqml.qml"));
    v->show();
    return app->exec();
}
```

这里我们创建了一个应用程序和一个[QQuickView](http://doc.qt.io/qt-5/qquickview.html)。我们将它们存储在[QScopedPointers](http://doc.qt.io/qt-5/qscopedpointer.html)中，以确保它们的资源被适当地释放。下一行将进行实际的输出。它将`DemoModel`暴露在1.0版本的`com.example`命名空间下。这允许QML页面实例化`DemoModel`组件，就像它们是本地数据类型一样。完成我们应用程序的最后一步是创建一个页面来显示模型的内容。

## 在一个视图中显示数据

实例化和显示一个`DemoModel'是很简单的。
```qml
import QtQuick 2.0
import Sailfish.Silica 1.0
import com.example 1.0

Page {
    id: page

    SilicaListView {

        anchors.fill: parent

        model: DemoModel {
            id: dmodel
        }

        header: PageHeader {
            id: header
            title: "Unusual animals"
        }

        delegate: BackgroundItem {
            Label {
                x: Theme.horizontalPageMargin
                text: name
            }
            onClicked: dmodel.activate(index)
        }
    }
}
```

这里我们创建了一个 "SilicaListView"，并将 "DemoModel "作为其数据模型。所有的实际工作都在委托中完成。它是一个简单的文本标签，只是从模型中获取内容。这是由 "text: name "这行代码完成的，它告诉QML获得当前项目的名称角色。这导致了对上述数据方法的调用，并自动填充了索引和角色。最后，为了使被点击的项目移动到顶部，我们设置了`onClicked`属性。当点击时，这将指示QML引擎调用给定索引的`DemoModel`的激活方法。委托的`index'变量是由QML自动提供的。

## 读者的练习

现在你应该有一个结合了C++和QML的应用程序，你应该能够创建你自己的自定义模型。目前，代码通过添加和删除相同的元素来改变支持的模型。Qt提供了一种更平滑的方式来实现同样的事情：移动行。这使得例如视图可以更好地对过渡进行动画处理。试着将这段代码转换为使用移动操作来代替。你应该完全不需要改变QML文件。
