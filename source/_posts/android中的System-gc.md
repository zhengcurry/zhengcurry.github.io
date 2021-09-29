---
title: android调用System.gc()有用么？
date: 2021-09-27 10:43:43
tags: Android
categories: Android

---

#### **结论**

在Android中，仅调用System.gc(）并不起作用；

若有需求，可采用方法：

1. 直接调用`Runtime.getRuntime().gc()`
2. 调用`System.runFinalization()`和`System.gc()`

> 即使没有显式调用gc方法，虚拟机也会根据需要在单独的线程中自动执行此回收过程



#### **源码分析**

```java
/**
 *	System.gc(）源码
 *	由此可见，真正起作用的是Runtime.getRuntime().gc()，
 *	且仅当shouldRunGC=true时，才会执行；
 *	而shouldRunGC的赋值取决于justRanFinaljuization
 */
public static void gc() {
        boolean shouldRunGC;
        synchronized (LOCK) {
            shouldRunGC = justRanFinaljuization;
            if (shouldRunGC) {
                justRanFinalization = false;
            } else {
                runGC = true;
            }
        }
        if (shouldRunGC) {
            Runtime.getRuntime().gc();
        }
    }
```

```java
/**
 *	justRanFinaljuization仅在runFinalization（）方法中赋值为true;
 *	且runGC也仅在gc()方法中赋值为true
 * 	因此代码中仅调用System.gc()无用
 */
public static void runFinalization() {
        boolean shouldRunGC;
        synchronized (LOCK) {
            shouldRunGC = runGC;
            runGC = false;
        }
        if (shouldRunGC) {
            Runtime.getRuntime().gc();
        }
        Runtime.getRuntime().runFinalization();
        synchronized (LOCK) {
            justRanFinalization = true;
        }
    }
```
