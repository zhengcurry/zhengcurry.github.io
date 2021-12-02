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

