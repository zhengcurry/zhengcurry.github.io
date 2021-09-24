---
title: T.()->Unit
date: 2021-09-24 13:57:58
tags: Kotlin
categories: Kotlin
---

T.()->Unit相当于是给类T定义了一个扩展函数，该函数没有形参，没有返回值；

```kotlin
typealias CallBack<T> = TestCallBack<T>.()->Unit

fun test(callback:CallBack<T>){
    val test = TestCallBack<T>()
    test.callback()
}
```

这里callback就相当于一个TestCallBack的扩展函数，所以可以test.callback()

