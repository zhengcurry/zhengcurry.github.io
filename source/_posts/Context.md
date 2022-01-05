---
title: Context
date: 2022-01-04 10:20:07
tags: Android
categories: Android

---

Context 

applicationContext

- Context到底是什么?

> 有关应用程序环境的全局信息的接口。 这是一个抽象类，其实现由Android系统提供。 它允许访问特定于应用程序的资源和类，以及调用应用程序级操作，例如启动活动、广播和接收意图等

- 一个应用程序有几个Context？

​		activity的个数+service的个数+application的个数

- Context能干什么？

​		基类；抽象类；

- Context作用域

- Context是何时怎样创建的？

  以activity的context为例，追踪源码，结合activity的启动流程可以找到

  ```java
  /**
   * ActivityThread
   */
  private Activity performLaunchActivity(ActivityClientRecord r, Intent customIntent) {
          ...
  
          ContextImpl appContext = createBaseContextForActivity(r);
          
      	...
  
          return activity;
      }
  
  /**
   * ContextImpl
   */
  @UnsupportedAppUsage
  static ContextImpl createActivityContext(ActivityThread mainThread,
              LoadedApk packageInfo, ActivityInfo activityInfo, IBinder activityToken, int displayId,
              Configuration overrideConfiguration) {
         ...
  
          ContextImpl context = new ContextImpl(null, mainThread, packageInfo, null,
                  activityInfo.splitName, activityToken, null, 0, classLoader, null);
          ...
          return context;
      }
  ```

  

