---
title: 允许的APIs
permalink: Develop/Apps/Harbour/Allowed_APIs/
parent: Harbour
grand_parent: Apps
layout: default
nav_order: 200
---

这些信息从Sailfish OS 4.3.0版本开始有效。

你可以随时从[验证器配置文件](https://github.com/sailfishos/sdk-harbour-rpmvalidator)查看最新的列表。

## 允许的库

你的应用程序可以与以下库链接。

### Qt 5核心库

  - libQt5Core.so.5
  - libQt5Quick.so.5
  - libQt5Qml.so.5
  - libQt5Network.so.5
  - libQt5Gui.so.5

### Sailfish Silica库，应用程序库+booster helper library

  - libsailfishapp.so.1
  - libmdeclarativecache5.so.0
  - libsailfishsilica.so.1

### Sailfish WebView库

  - libqt5embedwidget.so.1
  - libsailfishwebengine.so.1

### Amber网络授权框架

  - libamberwebauthorization.so.1

### OpenGL ES 1.1, 2.0 和 EGL

  - libEGL.so.1
  - libGLESv1_CM.so.1
  - libGLESv2.so.2

### 标准系统和C/C++运行时库

  - ld-linux.so.2
  - ld-linux-armhf.so.3
  - ld-linux-aarch64.so.1
  - libpthread.so.0
  - libstdc++.so.6
  - libm.so.6
  - libgcc_s.so.1
  - libc.so.6
  - librt.so.1
  - libdl.so.2
  - libz.so.1
  - libresolv.so.2

### Nemo库

  - libnemonotifications-qt5.so.1
  - libnemothumbnailer-qt5.so.1
  - libkeepalive.so.1

### 额外的 Qt 5 模块

  - libQt5Concurrent.so.5
  - libQt5Multimedia.so.5
  - libQt5Sql.so.5
  - libQt5Svg.so.5
  - libQt5XmlPatterns.so.5
  - libQt5Xml.so.5
  - libQt5DBus.so.5
  - libQt5WebKit.so.5
  - libQt5Sensors.so.5
  - libQt5Positioning.so.5
  - libQt5WebSockets.so.5

### 各种有用的附加库

  - libmlite5.so.0
  - libpng16.so.16
  - libdbus-1.so.3
  - libcurl.so.4
  - libfontconfig.so.1
  - libssl.so.1.1
  - libcrypto.so.1.1
  - liblzma.so.5
  - libxml2.so.2
  - libbz2.so.1
  - libexpat.so.1
  - libsqlite3.so.0

### GLib

  - libgio-2.0.so.0
  - libglib-2.0.so.0
  - libgmodule-2.0.so.0
  - libgobject-2.0.so.0
  - libgthread-2.0.so.0

### 低级别的 PulseAudio 和 Audio APIs

  - libpulse.so.0
  - libpulse-simple.so.0
  - libaudioresource.so.1

### 低级别的 Wayland 协议 APIs

  - libwayland-client.so.0
  - libwayland-cursor.so.0
  - libwayland-egl.so.1

### 多媒体

  - libogg.so.0
  - libvorbis.so.0
  - libvorbisenc.so.2
  - libvorbisfile.so.3
  - libsndfile.so.1

### SDL2

  - libSDL2-2.0.so.0
  - libSDL2_gfx-1.0.so.0
  - libSDL2_image-2.0.so.0
  - libSDL2_mixer-2.0.so.0
  - libSDL2_net-2.0.so.0
  - libSDL2_ttf-2.0.so.0

## 允许的 QML 进口

你的应用程序不允许有符合以下模式的 QML 导入。

### 不允许的QML导入

  - Bluetooth.*
  - Meego.*
  - Mer.*
  - Nemo.*
  - NemoMobile.*
  - Sailfish.*
  - Qt*
  - org.nemomobile.*
  - org.sailfishos.*
  - com.jolla.*
  - com.nokia.*
  - com.meego.*
  - org.kde.bluezqt

这条规则的例外情况是以下的导入。

### Sailfish API

  - Sailfish.Silica 1.0
  - Sailfish.Pickers 1.0
  - Sailfish.Share 1.0
  - Sailfish.WebView 1.0
  - Sailfish.WebView.Controls 1.0
  - Sailfish.WebView.Pickers 1.0
  - Sailfish.WebView.Popups 1.0
  - Sailfish.WebEngine 1.0

### Amber Web授权框架

  - Amber.Web.Authorization 1.0

### Qt APIs

  - QtQml 2.0
  - QtQml 2.1
  - QtQml 2.2
  - QtQuick 2.0
  - QtQuick 2.1
  - QtQuick 2.2
  - QtQuick 2.3
  - QtQuick 2.4
  - QtQuick 2.5
  - QtQuick 2.6
  - QtQuick.Layouts 1.0
  - QtQuick.Layouts 1.1
  - QtQuick.LocalStorage 2.0
  - QtQuick.Particles 2.0
  - QtQuick.Window 2.0
  - QtQuick.Window 2.1
  - QtQuick.Window 2.2
  - QtQuick.XmlListModel 2.0

### 额外的QML模块

  - QtMultimedia 5.0
  - QtMultimedia 5.1
  - QtMultimedia 5.2
  - QtMultimedia 5.3
  - QtMultimedia 5.4
  - QtMultimedia 5.5
  - QtMultimedia 5.6
  - QtWebKit 3.0
  - QtWebSockets 1.0
  - QtWebSockets 1.1
  - QtSensors 5.0
  - QtSensors 5.1
  - QtSensors 5.2
  - QtGraphicalEffects 1.0
  - QtPositioning 5.2
  - QtPositioning 5.4
  - QtQml.Models 2.1
  - QtQml.Models 2.2
  - QtQml.Models 2.3

### QtFeedback 还没有被宣布为稳定的，但我们允许有限制的部分

  - QtFeedback 5.0

### ContextKit

  - org.freedesktop.contextkit 1.0

### Python支持

  - io.thp.pyotherside 1.0
  - io.thp.pyotherside 1.1
  - io.thp.pyotherside 1.2
  - io.thp.pyotherside 1.3
  - io.thp.pyotherside 1.4
  - io.thp.pyotherside 1.5

### Nemo QML模块

  - Nemo.Notifications 1.0
  - Nemo.DBus 2.0
  - Nemo.Configuration 1.0
  - Nemo.Thumbnailer 1.0
  - Nemo.KeepAlive 1.2

## 允许的软件包依赖性

通常情况下，你不应该向你的软件包手动添加库的依赖性或python模块的依赖性，因为这些依赖性是自动生成的。你的rpm包可以要求以下内容。

### 核心库

  - libc.so.6
  - libpthread.so.0
  - librt.so.1
  - libm.so.6
  - libdl.so.2
  - ld-linux.so.2
  - ld-linux-armhf.so.3
  - ld-linux-aarch64.so.1
  - libz.so.1
  - libgcc_s.so.1

### C++标准库

  - libstdc++.so.6

### 其他库

  - libpng16.so.16

### PulseAudio

  - libpulse.so.0
  - libpulse-simple.so.0

### Sailfish Silica QML API

  - libpng15.so.15

### 在Sailfish OS 4.0.1中已被弃用

  - libssl.so.10
  - libcrypto.so.10

## 废弃的QML输入法

以下的QML导入已经被重新命名。旧的导入不应该再被用于新的代码中。在未来的版本中，它们将被从允许的导入中删除。

  - org.nemomobile.notifications 1.0
    - 重命名为 "Nemo.Notifications 1.0"。
  - org.nemomobile.dbus 2.0
    - 重命名为 "Nemo.DBus 2.0"。
  - org.nemomobile.configuration 1.0
    - 重命名为 "Nemo.Configuration 1.0"。
  - org.nemomobile.thumbnailer 1.0
    - 重命名为 "Nemo.Thumbnailer 1.0"。
