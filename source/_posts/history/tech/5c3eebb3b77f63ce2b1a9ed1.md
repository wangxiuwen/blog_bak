---
title: flutter MacOS 开发环境搭建
date: 2019-01-16 04:30:43
tags: ["tech","技术"]
author: baipeng
categories: ["技术"]
layout: post
---

```
1.下载安装包
https://flutter.io/docs/get-started/install/macos
2.mkdir ~/development && cd ~/development  
将下载安装包移动到这个目录  mv 下载的文件夹 /flutter.**.zip ~/development
3.设置环境变量 export PATH=$PATH:`pwd`/flutter/bin
4.执行检查 flutter doctor 查看结果~~👇
```

```
*******:development ******$ flutter doctor
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel stable, v1.0.0, on Mac OS X 10.14.2 18C54, locale zh-Hans-CN)
[✗] Android toolchain - develop for Android devices
    ✗ Unable to locate Android SDK.
      Install Android Studio from: https://developer.android.com/studio/index.html
      On first launch it will assist you in installing the Android SDK components.
      (or visit https://flutter.io/setup/#android-setup for detailed instructions).
      If Android SDK has been installed to a custom location, set $ANDROID_HOME to that location.
      You may also want to add it to your PATH environment variable.

[!] iOS toolchain - develop for iOS devices (Xcode 10.1)
    ✗ libimobiledevice and ideviceinstaller are not installed. To install with Brew, run:
        brew update
        brew install --HEAD usbmuxd
        brew link usbmuxd
        brew install --HEAD libimobiledevice
        brew install ideviceinstaller
    ✗ ios-deploy not installed. To install with Brew:
        brew install ios-deploy
    ✗ CocoaPods not installed.
        CocoaPods is used to retrieve the iOS platform side's plugin code that responds to your plugin usage on the Dart side.
        Without resolving iOS dependencies with CocoaPods, plugins will not work on iOS.
        For more info, see https://flutter.io/platform-plugins
      To install:
        brew install cocoapods
        pod setup
[!] Android Studio (not installed)
[!] IntelliJ IDEA Ultimate Edition (version 2018.1.3)
    ✗ Flutter plugin not installed; this adds Flutter specific functionality.
    ✗ Dart plugin not installed; this adds Dart specific functionality.
[!] VS Code (version 1.30.2)
[!] Connected device
    ! No devices available
```

```
7.发现没有安装Android SDK 安装，so !! 安装它  地址：http://www.android-studio.org/ 
找到	android-studio-ide-*****-mac.dmg  下载安装它。
安装完执行 flutter doctor --android-licenses  OK 搞定。。

执行剩下的命令。没有安装brew 先安装它 有直接执行下面的命令~
        brew install --HEAD usbmuxd
        brew link usbmuxd
        brew install --HEAD libimobiledevice
        brew install ideviceinstaller
		brew install ios-deploy
		brew install cocoapods
        pod setup



```