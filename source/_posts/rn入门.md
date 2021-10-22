title: rn入门
tags: RN
categories: RN
date: 2021-10-13 09:43:44
---
### **环境搭建**

```
FAILURE: Build failed with an exception.

* What went wrong:
Could not determine the dependencies of task ':app:compileDebugJavaWithJavac'.
> Failed to install the following Android SDK packages as some licences have not been accepted.
     patcher;v4 SDK Patch Applier v4
     emulator Android Emulator
     platforms;android-30 Android SDK Platform 30
     build-tools;30.0.2 Android SDK Build-Tools 30.0.2
     tools Android SDK Tools
  To build this project, accept the SDK license agreements and install the missing components using the Android Studio SDK Manager.
  Alternatively, to transfer the license agreements from one workstation to another, see http://d.android.com/r/studio-ui/export-licenses.html

  Using Android SDK: C:\android_sdk\platform-tools

* Try:
Run with --stacktrace option to get the stack trace. Run with --info or --debug option to get more log output. Run with --scan to get full insights.

* Get more help at https://help.gradle.org

BUILD FAILED in 28s

error Failed to install the app. Please accept all necessary Android SDK licenses using Android SDK Manager: "$ANDROID_HOME/tools/bin/sdkmanager --licenses".
Error: Command failed: gradlew.bat app:installDebug -PreactNativeDevServerPort=8081

```

经检查是环境变量配置有问题，导致找不到对应的路径；
可以参考[官方文档](https://www.react-native.cn/docs/environment-setup)