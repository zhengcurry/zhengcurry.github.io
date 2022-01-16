---
title: StartUp
date: 2022-01-09 14:30:52
tags: Andoid
categories: Android

---

#### **介绍：**

> JetPack成员之一
>
> 官方说法：App Startup 库提供了一种在应用程序启动时初始化组件的简单、高效的方法。

**目的：**

​	解决初始化时，用ContentProvider来获取context，造成的启动速度减慢的情况。

**相关知识点：**

​	[ContentProvider获取Context]()

​	[Android启动优化]()

**[使用方法：](https://developer.android.google.cn/topic/libraries/app-startup)**

1. 依赖

   ```groovy
   dependencies {
       implementation("androidx.startup:startup-runtime:1.1.0")
   }
   ```

2. 实现

   ```kotlin
   class MyInitializer : Initializer<Unit> {
       override fun create(context: Context) {
         //TODO：传入context的实例化
       }
       /**
        * 解决组件间初始化的依赖顺序
        * 当前的Initializer是否还依赖于其他的Initializer，
        * 
        * 如果有的，就在这里进行配置
        * App Startup会保证先初始化依赖的WorkManagerInitializer，
        * 然后才会初始化当前的MyInitializer
        *
        * 如果没有，就是emptyList()
        */
       override fun dependencies(): List<Class<out Initializer<*>>> {
           return listOf(WorkManagerInitializer::class.java)
       }
   }
   
   class WorkManagerInitializer : Initializer<WorkManager> {
       override fun create(context: Context): WorkManager {
           val configuration = Configuration.Builder().build()
           WorkManager.initialize(context, configuration)
           return WorkManager.getInstance(context)
       }
       override fun dependencies(): List<Class<out Initializer<*>>> {
           return emptyList()
       }
   }
   ```

3. 配置

   ```java
   <provider
       android:name="androidx.startup.InitializationProvider"
       android:authorities="${applicationId}.androidx-startup"
       android:exported="false"
       tools:node="merge">
       <meta-data
           android:name="com.curry.advance.startup.MyInitializer"
           android:value="androidx.startup" />
   </provider>
   ```
   
   >	- 组件名必须是`androidx.startup.InitializationProvider`；
   >	- 需要声明`android:exported="false"`，以限制其他应用访问此组件；
   >	- 要求`android:authorities`在整个手机唯一，通常使用${applicationId}作为前缀；
   >	- 需要声明`tools:node="merge"`，确保`manifest merger tool`能够正确解析冲突的节点；

​		`这里不需要为WorkManagerInitializer添加<meta-data>，因为WorkManagerInitializer是MyInitializer的依赖项（即出现在MyInitializer#dependencies()方法中）; 否则有多少Initializer就需要多少<meta-data>`

4. 其他实现方式：手动初始化

   1. 禁用单个组件的自动初始化，在<meta-data>标签里增加tools:node="remove"

      禁用全部组件的自动初始化，在<provider>标签里增加tools:node="remove"

      ```java
      <provider
          android:name="androidx.startup.InitializationProvider"
          android:authorities="${applicationId}.androidx-startup"
          android:exported="false"
          tools:node="merge">
          <meta-data
              android:name="com.curry.advance.startup.MyInitializer"
              tools:node="remove" />
      </provider>
      ```

   2. 代码实现

      ```kotlin
      AppInitializer.getInstance(context).initializeComponent(MyInitializer::class.java)
      ```

**原理：**

​	此库的源码很少，只有5个类：

- Initializer（对外接口）

- StartupException（自定义异常）

- StartupLogger（日志）

- InitializationProvider（自定义的ContentProvider，仅onCreate中实现了`AppInitializer.getInstance(getContext()).discoverAndInitialize();` ）

- AppInitializer（核心，单例模式）

  ```java
  /**
   *  核心方法
   *  component：自定义的Initializer类（自动通过manifest中添加的<meta-data>-<name>或手动传入的类）
   *  initializing: HashSet集合，第一次遍历为空集合
   */
  <T> T doInitialize(
              @NonNull Class<? extends Initializer<?>> component,
              @NonNull Set<Class<?>> initializing) {
          synchronized (sLock) {
              ...
              try {
                ...
                  //这里是判断是否重复初始化
                  if (initializing.contains(component)) {
                      String message = String.format(
                              "Cannot initialize %s. Cycle detected.", component.getName()
                      );
                      throw new IllegalStateException(message);
                  }
                  Object result;
                  //mInitialized是以component为key，以实现Initializer接口传入的类型为value的map集合
                  if (!mInitialized.containsKey(component)) {
                      initializing.add(component);
                      try {
                          // 获取实现Initializer接口后，dependencies方法返回的list
                          Object instance = component.getDeclaredConstructor().newInstance();
                          Initializer<?> initializer = (Initializer<?>) instance;
                          List<Class<? extends Initializer<?>>> dependencies =
                                  initializer.dependencies();
                          if (!dependencies.isEmpty()) {
                              for (Class<? extends Initializer<?>> clazz : dependencies) {
                                  if (!mInitialized.containsKey(clazz)) {
                                      doInitialize(clazz, initializing);
                                  }
                              }
                          }
                        	...
                          // mContext是AppInitializer.getInstance(context)时传入的
                          result = initializer.create(mContext);
                          ...
                          initializing.remove(component);
                          mInitialized.put(component, result);
                      } catch (Throwable throwable) {
                          throw new StartupException(throwable);
                      }
                  } else {
                      result = mInitialized.get(component);
                  }
                  return (T) result;
              } finally {
                  Trace.endSection();
              }
          }
      }
  ```

  因此，通过源码的分析，这个库其实是起到了一个集合的作用；当一个应用，需要初始化多个通过ContentProvider获取context的库时，若相关的库都集成了startup，那么整个应用只会通过InitializationProvider来获取context。

**PS：**

​	自我理解：此库只是适用于 `通过ContentProvider获取Context进行初始化的sdk或lib` 使用；



-----

**记录：**

​	基于App StartUp实现的[Android-StartUp](https://juejin.cn/post/6899748777721905159)

> 提供一种在应用启动时能够更加简单、高效的方式来初始化组件。开发人员可以使用它来简化启动序列，并显式地设置初始化顺序与组件之间的依赖关系。 与此同时支持同步与异步等待、线程优化与多进程，并通过有向无环图拓扑排序的方式来保证内部依赖组件的初始化顺序。

------



**相关参考：**

https://developer.android.google.cn/topic/libraries/app-startup

[Jetpack新成员，App Startup一篇就懂](https://guolin.blog.csdn.net/article/details/108026357)

[Android | App Startup可能比你想象中要简单](https://www.jianshu.com/p/98d3623fa2d4)

[我为何弃用Jetpack的App Startup?](https://juejin.cn/post/6859500445669752846)
