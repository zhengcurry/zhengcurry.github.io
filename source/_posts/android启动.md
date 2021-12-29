---
title: android启动
date: 2021-12-28 15:42:48
tags: Android
categories: Android

---

Activity启动之前的一些事情
init进程：init是所有linux程序的起点，是Zygote的父进程。解析init.rc孵化出Zygote进程。

Zygote进程：Zygote是所有Java进程的父进程，所有的App进程都是由Zygote进程fork生成的。

SystemServer进程：System Server是Zygote孵化的第一个进程。SystemServer负责启动和管理整个Java framework，包含AMS，PMS等服务。

Launcher：Zygote进程孵化的第一个App进程是Launcher。

1.init进程是什么？
Android是基于linux系统的，手机开机之后，linux内核进行加载。加载完成之后会启动init进程。
init进程会启动ServiceManager，孵化一些守护进程，并解析init.rc孵化Zygote进程。

2.Zygote进程是什么？
所有的App进程都是由Zygote进程fork生成的，包括SystemServer进程。Zygote初始化后，会注册一个等待接受消息的socket，OS层会采用socket进行IPC通信。

3.为什么是Zygote来孵化进程，而不是新建进程呢？
每个应用程序都是运行在各自的Dalvik虚拟机中，应用程序每次运行都要重新初始化和启动虚拟机，这个过程会耗费很长时间。Zygote会把已经运行的虚拟机的代码和内存信息共享，起到一个预加载资源和类的作用，从而缩短启动时间。











基于API 30  Android 11

```java
ActivityTaskManager 
IActivityTaskManager
    
ActivityManager
```























https://juejin.cn/post/6844904114170626061

https://blog.csdn.net/u012267215/article/details/91406211

https://www.jianshu.com/p/a22b2c436e8b
