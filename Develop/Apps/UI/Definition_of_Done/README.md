---
title: 已完成的定义
permalink: Develop/Apps/UI/Definition_of_Done/
parent: UI
grand_parent: Apps
layout: default
nav_order: 100
---

当你开发了一个新的功能，修复了一个错误，或者对Sailfish OS的用户界面有贡献，那么这个清单可以帮助你让它被接受。

  - 代码已被区域维护者审查和批准，最好是有UI开发背景的人。
  - 任何面向用户的修改都已被Jolla设计部门审查和批准。
  - 代码与设计在像素上完美匹配。
      - 像素的准确性在QML中是很容易的，没有理由偷懒。
      - 图标、背景、边距、颜色、字体与平台风格一致。
      - 实施符合设计和整体平台风格。
  - 实施的用户界面在设备上执行得很顺利。
      - 在交互过程中，没有突然的跳跃、停顿或其他明显的帧率下降。
      - 引入的变化不会影响到应用程序或页面的加载时间。
      - 所有的用户界面状态转换都是流畅的，并有平滑的动画效果。
      - 使用可用的[性能工具]来测量和分析性能。
      - 应用程序的内存消耗保持合理（例如，用[smem](http://www.selenic.com/smem)测量）。
      - UI可以优雅地扩展到大量的数据点。
  - 特征是完整的特征。
      - 没有看起来未完成的状态，也没有突出的不良状态。
      - 没有空的视图、假的或其他形式的临时开发数据。
      - 为用户提供了从可能的错误中恢复的行动，如没有网络或数据损坏。
  - 文本包含工作的翻译钩子。
  - 字体、颜色、边距和其他布局参数来自[Silica](https://sailfishos.org/develop/docs/silica/qml-sailfishsilica-sailfish-silica-theme.html)，以保证可扩展性和一致的平台外观和感觉。
  - 功能有单元测试，测试内部状态和必要的功能。
  - 现有的组件测试通过。
  - 更改不会导致其他地方的回归，由贡献者测试并由审查者验证。
  - 代码已提交到源码控制，并在OBS中成功构建。

## 进一步的建议

  - 在真实设备上开发和测试功能。
  - 预计会有多轮的设计审查，直到体验足够完美。
  - 当不确定一个功能应该如何表现时，最好是检查一下现有的Sailfish应用程序中类似的用例是如何实现的。
  - 当QML不能执行时，要准备好用低级别的C/C++和GLSL OpenGL着色语言来编写有问题的部分。
  - 除非你已经做了阅读所有的[Qt快速文档](http://doc.qt.io/qt-5/qtquick-index.html)，你可以在Qt文档页面找到。
  - 阅读官方的[Qt性能文档](http://doc.qt.io/qt-5/qtquick-performance.html)。明天再读一遍。此外，KDAB Qt合作伙伴也写了一些关于[QML引擎内部结构](http://www.kdab.com/category/blogs/qmlengineseries)的优秀文章。
  - 测量，不要假设，运行[QML profiler](http://doc.qt.io/qtcreator/creator-qml-performance-monitor.html)和收集[控制台上API跟踪](http://blog.qt.io/blog/2012/03/01/debugging-qt-quick-2-console-api)，看看有没有什么愚蠢的事情在幕后发生，例如不必要的QML组件构造或编译、多余的绑定评估、长的嵌套JavaScript函数调用或阻塞的C++函数调用。基本上，任何让主线程停顿几十到几百毫秒的操作都应该被优化或在一个单独的线程上执行。
