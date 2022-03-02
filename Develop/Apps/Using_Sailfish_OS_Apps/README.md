---
title: 使用Sailfish OS应用程序
permalink: Develop/Apps/Using_Sailfish_OS_Apps/
parent: Apps
layout: default
nav_order: 600
---

## 使用Sailfish OS应用程序

Sailfish OS是一个基于触摸的用户界面--我们将快速解释如何在模拟器中使用一些手势--完整的解释见UX框架页面。在Sailfish OS中，有两种手势--"拉 "是在屏幕内的手势；"推 "是在屏幕外的手势。想象一下通过推或拉来移动屏幕的边缘可能会有帮助。

> 提示：在模拟器中用鼠标推是有点麻烦的，因为它们实际上是从窗口的最边缘开始的--有些用户喜欢在VirtualBox中禁用鼠标集成（按Host + "I "或使用机器菜单），以使从边缘推更容易。


### 滑动菜单

在用户界面中，注意到顶部边缘的发光点。它表示一个滑动菜单，可以通过主窗口的下拉手势显示出来。

<a href="Screenshot_01.png" style="width:30em;display:block">
    <img src="Screenshot_01.png"
         alt="Screenshot_01.png"
         class="md_thumbnail" style="max-width:100%"/>
</a>

### 嵌套页

滑动菜单项 "Show Page 2"带你到下一页。该菜单项可以通过两种方式激活。

1.  把滑动菜单往下拉，直到该菜单项被高亮显示；然后松开激活，或
    <a href="Screenshot_02.png" style="width:30em;display:block">
    <img src="Screenshot_02.png"
         alt="Screenshot_02.png"
         class="md_thumbnail" style="max-width:100%"/>
</a>
2.  将滑动菜单一直往下拉，露出整个菜单，然后点击菜单项目激活
    <a href="Screenshot_03.png" style="width:30em;display:block">
    <img src="Screenshot_03.png"
         alt="Screenshot_03.png"
         class="md_thumbnail" style="max-width:100%"/>
</a>

在第二页，注意左上角的视觉指示器。它表明这是一个堆叠的页面。

要移回第一页，你可以。

1. 在屏幕的任何地方从左到右拉动即可
2. 从左到右拉动页眉（如果屏幕有其他触摸功能，则很有用）。
3. 触摸左上角发光的页面指示灯
    <a href="Screenshot_04.png" style="width:30em;display:block">
    <img src="Screenshot_04.png"
         alt="Screenshot_04.png"
         class="md_thumbnail" style="max-width:100%"/>
</a>

### 活动封面

你可以进入 "主页 "区域，通过从侧面推动应用程序，看到你刚刚编译的应用程序的活动封面。

这个应用程序显示的是 "我的封面 "作为视觉表现，并提供播放和暂停图标来描述可能的操作。

从左到右拉动封面，可以进入左边的动作；从另一边拉动，可以进入右边的动作。

###停止应用程序

在主屏幕上按住应用程序，直到盖子淡出并提供一个关闭按钮 - 然后点击关闭按钮

你也可以在Sailfish IDE（Qt Creator）中通过使用停止按钮直接关闭该应用程序。

接下来， [代码演练](/Develop/Apps/Code_Walkthrough).
