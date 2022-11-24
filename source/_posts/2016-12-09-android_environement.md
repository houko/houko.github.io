---
author: 小莫
date: 2016-12-09
title: android环境搭建.
tags: 
- android
category: android
permalink: AndroidEnvironment
---
普天同庆，谷歌打算重回中国市场了。不管是angular2的完成，还是昨天developer.google.cn的开通，都是对开发者来说是一件高兴的事情。正好趁此机会研究一下android。
<!-- more -->

### 一、IDE工具下载
[android studio官网](http://www.android-studio.org/) 1.6G，有点大哦。

### 二、 评价
不得不说，看到android studio第一眼就觉得舒服。从IDEA无缝切换到as，除了本身有点大，其他没毛病。

### 三、配置
在安装好之后，启动时会让我们配一下代理，也就是http proxy这东西。配完镜像速度会变快，当然在中国不配就等于用不了。真希望啥时候谷歌能全面回到中国市场。
[配置说明](http://mirrors.neusoft.edu.cn/more.we#android)  mirrors.neusoft.edu.cn:80
![android http proxy](https://image.xiaomo.info/android/httpProxy.png)

### 四、设置SDK
在安装android studio时也装了android SDK,找到我们安装时间的位置。我装的时候没有截图，就不放图了。我放在E盘，默认在C/用户/appData/xxxx。
![android http proxy](https://image.xiaomo.info/android/androidSdk.png)
选中Android/sdk，下一步，完成。

### 五、创建项目
创建项目

首先，先指出Android Studio中的两个概念。 Project 和 Module 。在Android Studio中， Project 的真实含义是工作空间， Module 为一个具体的项目。

在 Eclipse 中，我们可以同时对多个 Eclipse 的 Project 进行同时编辑，这些 Project 在同一个 workspace 之中。在Android Studio中，我们可以同时对多个Android Studio的 Module 进行同时编辑，这些 Module 在同一个 Project 之中。

Eclipse 的 Project 等同于Android Studio的 Module 。
Eclipse 的 workspace 等同于Android Studio的 Project 。

本文中所说到的项目指的是Android Studio的 Module 。Android Studio创建一个项目，首先要先创建 Project 。但是你创建项目的同时， Project 自动创建了，因此很多人容易混淆这两种概念。


![newProject](https://image.xiaomo.info/android/newProject.png)
Application name ：应用程序的名称。它是app在设备上显示的应用程序名称，也是在Android Studio Project 的名称。
Company Domain ：公司域名。影响下面的 Package name 。默认为电脑主机名称，当然你也可以单独设置 Package name 。
Package name ：应用程序包名。每一个app都有一个独立的包名，如果两个app的包名相同，Android会认为他们是同一个app。因此，需要尽量保证，不同的app拥有不同的包名。
Project localtion ： Project 存放的本地目录。

以上内容设置完毕，点击 Next 。


![setSdk](https://image.xiaomo.info/android/setSdk.png)

在这里，你可以你的 Project 中 Module 的类型以及支持的最低版本。
Phone and Tablet ：表示 Module 是一个手机和平板项目。
TV ：表示 Module 是一个Android TV项目。
Wear ：表示 Module 是一个可穿戴设备（例如手表）项目。
Glass ：表示 Module 是一个 Google Glass 项目（不知道 Google Glass 是什么请自行搜索）。

你可以同时选择多个类型，区别就是项目会根据你选择的类型创建一个或多个 Module 。

Minimum SDK 表示的是 Module 支持的Android最低版本。根据不同的用户可以选择不同的版本。你可以点击 Help me choose 来查看当前Android版本分布情况。现在这个时代，如果你的项目支持到 2.2 版本几乎是支持了所有的Android设备。

以上内容设置完毕，点击 Next 。


接下来，我们会看到这个页面（由于我的 Module 类型只选择了 Phone and Tablet ，所以会有这个页面。）。


这个页面是让我们选择是否创建 Activity以及创建 Activity 的类型。你可以选择不创建 Activity （ Add No Activity ）。

如果你选择自动创建 Activity，Android Studio会自动帮你生成一些代码。根据 Activity 类型的不同，生成的代码也是不同的。有时，你能从这些自动生成的代码中，学到很多东西，比如 Fullscreen Activity 。
选择完毕，点击 Next ,然后就创建好了。

### 六、项目目录结构
在Android Studio中，提供了以下几种项目结构类型：project、packages、android等等。我们常用的就是project和android两种，所以就介绍这两种
![projectJiegou](https://image.xiaomo.info/android/projectJiegou.png)

```
app/build/ app模块build编译输出的目录
app/build.gradle app模块的gradle编译文件
app/app.iml app模块的配置文件
app/proguard-rules.pro app模块proguard文件
build.gradle 项目的gradle编译文件
settings.gradle 定义项目包含哪些模块
gradlew 编译脚本，可以在命令行执行打包
local.properties 配置SDK/NDK
MyApplication.iml 项目的配置文件
External Libraries 项目依赖的Lib, 编译时自动下载的
```

![androidJiegou](https://image.xiaomo.info/android/androidJiegou.png)

app/manifests AndroidManifest.xml配置文件目录
app/java 源码目录
app/res 资源文件目录
Gradle Scripts gradle编译相关的脚本

原文  http://www.aswifter.com/2015/07/07/android-studio-project-struct/
