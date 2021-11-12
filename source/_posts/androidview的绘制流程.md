---
title: Android View绘制的完整流程及知识点
date: 2021-10-04 13:21:09
tags: Android
categories: Android

---

具体源码整理如下[流程图](https://www.processon.com/view/link/61600cfbf346fb0e99a6d639)

![源码流程图](源码流程图.png)

梳理过源码后，再看相关的知识点：

1. 首次 View 的绘制流程是在什么时候触发的？
2. ViewRootImpl 创建的时机？
3. ViewRootImpl 和 DecorView 的关系是什么？
4. DecorView 的布局是什么样的？
5. DecorView 的创建时机？
6. setContentView 的流程
7. LayoutInflate 的流程
8. Activity、PhoneWindow、DecorView、ViewRootImpl 的关系？
9. PhoneWindow 的创建时机？
10. 如何触发重新绘制？
11. requestLayout 和 invalidate 的流程
12. requestLayout 和 invalidate 的区别
13. 简单介绍下MeasureSpec
14. MeasureSpec的确定
15. 子View的MeasureSpec由父View根据自身的MeasureSpec和子View的LayoutParams来共同确定子View的MeasureSpec，注意，即使确定了子View的MeasureSpec并不一定决定了子View的大小，自定义View可以根据需要修改这个值，最终通过setMeasuredDimension（width,height）设置最终大小。



![img](https:////upload-images.jianshu.io/upload_images/6989232-799ff8ac91e38316?imageMogr2/auto-orient/strip|imageView2/2/w/1080/format/webp)

**5、View执行onMeasure,onLayout的次数**

分析ViewRootImpl的源码，scheduleTraversales()内部会执行postCallBack触发mTraversalRunnable重新走一遍performTraversals(),第二次执行performTraversals()就会触发performDraw()。所以performTraversals()走了两次，那么肯定会走2次measure方法，但不一定走2次onMeasure()，读过View measure方法源码的都应知道measure方法做了2级测量优化：

- 1.如果flag不为forceLayout或者与上次测量规格（MeasureSpec）相比未改变，那么将不会进行重新测量（执行onMeasure方法），直接使用上次的测量值；
- 2.如果满足非强制测量的条件，即前后二次测量规格不一致，会先根据目前测量规格生成的key索引缓存数据，索引到就无需进行重新测量;如果targetSDK小于API 20则二级测量优化无效，依旧会重新测量，不会采用缓存测量值。

**6、getWidth()和getMeasuredWidth()的区别**

getMeasuredWidth()、getMeasuredHeight()必须在onMeasure之后使用才有效）getMeasuredWidth() 的取值最终来源于 setMeasuredDimension() 方法调用时传递的参数, getWidth()返回的是，mRight - mLeft，mRight、mLeft 变量分别表示 View 相对父容器的左右边缘位置，getWidth()与getHeight()方法必须在layout(int l, int t, int r, int b)执行之后才有效

**7、如何在onCreate中拿到View的宽度和高度**

- **View.post(runnable)**



```java
view.post(new Runnable() {            
            @Override
            public void run() {
                int width = view.getWidth();
                int measuredWidth = view.getMeasuredWidth();
                Log.i(TAG, "width: " + width);
                Log.i(TAG, "measuredWidth: " + measuredWidth);
            }
        });      
```

利用Handler通信机制，发送一个Runnable到MessageQueue中，当View布局处理完成时，自动发送消息，通知UI进程。借此机制，巧妙获取View的高宽属性，代码简洁，相比ViewTreeObserver监听处理，还不需要手动移除观察者监听事件。

- **ViewTreeObserver.addOnGlobalLayoutListener()**

监听View的onLayout()绘制过程，一旦layout触发变化，立即回调onLayoutChange方法。
 注意，使用完也要主要调用removeOnGlobalListener()方法移除监听事件。避免后续每一次发生全局View变化均触发该事件，影响性能。



```java
ViewTreeObserver vto = view.getViewTreeObserver();       
       vto.addOnGlobalLayoutListener(new OnGlobalLayoutListener() {
            @Override
            public void onGlobalLayout() {
                view.getViewTreeObserver().removeGlobalOnLayoutListener(this);
                Log.i(TAG, "width: " + view.getWidth());
                Log.i(TAG, "height: " + view.getHeight());
            }
        });
```

**8、invalidate和postInvalidate区别**

二者都会出发刷新View，并且当这个View的可见性为VISIBLE的时候，View的onDraw()方法将会被调用，invalidate()方法在 UI 线程中调用，重绘当前 UI。postInvalidate() 方法在非 UI 线程中调用，通过Handler通知 UI 线程重绘。

**9、requestLayout()的作用**

requestLayout()也可以达到重绘view的目的，但是与前两者不同，它会先调用onLayout()重新排版，再调用ondraw()方法。当view确定自身已经不再适合现有的区域时，该view本身调用这个方法要求parent view（父类的视图）重新调用他的onMeasure、onLayout来重新设置自己位置。特别是当view的layoutparameter发生改变，并且它的值还没能应用到view上时，这时候适合调用这个方法requestLayout()。

**10、onDraw() 和dispatchDraw()的区别**

- 绘制View本身的内容，通过调用View.onDraw(canvas)函数实现
- 绘制自己的孩子通过dispatchDraw（canvas）实现

draw过程会调用onDraw(Canvas canvas)方法，然后就是dispatchDraw(Canvas canvas)方法, dispatchDraw()主要是分发给子组件进行绘制，我们通常定制组件的时候重写的是onDraw()方法。值得注意的是ViewGroup容器组件的绘制，当它没有背景时直接调用的是dispatchDraw()方法, 而绕过了draw()方法，当它有背景的时候就调用draw()方法，而draw()方法里包含了dispatchDraw()方法的调用。因此要在ViewGroup上绘制东西的时候往往重写的是dispatchDraw()方法而不是onDraw()方法，或者自定制一个Drawable，重写它的draw(Canvas c)和 getIntrinsicWidth()方法，然后设为背景。





https://www.jianshu.com/p/5a71014e7b1b

https://blog.csdn.net/sinat_27154507/article/details/79748010

https://www.cnblogs.com/andy-songwei/p/10955062.html

https://blog.csdn.net/Innost/article/details/6172893

https://blog.csdn.net/a553181867/article/details/51477040

https://juejin.cn/post/6872140986579943438

https://www.jianshu.com/p/c5d200dde486
