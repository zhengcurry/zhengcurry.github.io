---
title: IPC
date: 2021-11-30 11:28:05
tags: Android
categories: Android

---

#### **进程间通信**

实现进程间通信可以通过AIDL、广播、文件、ContentProvider等，而底层则离不开Binder机制。



两个进程对应的是不同的内存区域

- **1.Application对象会创建多次**
- **2.静态成员不共用**
- **3.同步锁失效**
- **4.单例模式失效**
- **5.数据传递的对象必须可序列化**









Android 系统是基于 Linux 内核的，Linux 已经提供了管道、消息队列、共享内存和 Socket 等 IPC 机制。那为什么 Android 还要提供 Binder 来实现 IPC 呢？主要是基于**性能**、**稳定性**和**安全性**几方面的原因。

![img](v2-30dce36be4e6617596b5fab96ef904c6_720w.jpg)











https://juejin.cn/post/6844903589635162126
