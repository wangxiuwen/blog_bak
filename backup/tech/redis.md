---
tags: ['rdm', 'redis']
date: 2019-08-06 06:01:34
---

# redis 笔记

## 编译 rdm

编译步骤：

<http://docs.redisdesktop.com/en/latest/install/#build-from-source>

::: tip 注意
直接按照官方的编译步骤不成功，需要按照以下修改 `rmd.pro` 工程文件
:::

```shell
git diff src/rdm.pro
```

```C++
diff --git a/src/rdm.pro b/src/rdm.pro
index de0794bd..bdec2933 100644
--- a/src/rdm.pro
+++ b/src/rdm.pro
@@ -79,20 +79,20 @@ unix:macx { # OSX
     QT += svg
     CONFIG += c++11

-    debug: CONFIG-=app_bundle
+    #debug: CONFIG-=app_bundle

     release: DESTDIR = ./../bin/osx/release
-    debug:   DESTDIR = ./../bin/osx/debug
+    #debug:   DESTDIR = ./../bin/osx/debug

     #deployment
     QMAKE_INFO_PLIST =  $$PWD/resources/Info.plist
     ICON = $$PWD/resources/rdm.icns

-    release {
-        CRASHREPORTER_APP.files = $$DESTDIR/crashreporter
-        CRASHREPORTER_APP.path = Contents/MacOS
-        QMAKE_BUNDLE_DATA += CRASHREPORTER_APP
-    }
+    #release {
+        #CRASHREPORTER_APP.files = $$DESTDIR/crashreporter
+        #CRASHREPORTER_APP.path = Contents/MacOS
+        #QMAKE_BUNDLE_DATA += CRASHREPORTER_APP
+    #}
 }

 unix:!macx { # ubuntu & debian
(END)
```

打包：

```shell
macdeployqt rdm.app -qmldir=/Users/williamwang/workspace/rdm/src/qml -dmg
```
