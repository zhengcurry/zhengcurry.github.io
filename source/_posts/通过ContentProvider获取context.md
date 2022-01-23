---
title: 通过ContentProvider获取context
date: 2022-01-09 21:21:53
tags: Android
Categories: Android

---



```kotlin
/**
  * 获取Context的无侵入方法
  * 如果应用的 ContentProvider 过多，会增加应用的启动时间；
  *
  * @see ActivityThread.handleBindApplication
  * @see ActivityThread.installContentProviders
  */
internal abstract class ContextProvider : ContentProvider(){

    override fun onCreate(): Boolean {
        context?.let { init(it) }
        return true
    }
}

private lateinit var application : Context

fun init(context : Context){
    application= context
}

fun context() : Context{
    return application
}
```



