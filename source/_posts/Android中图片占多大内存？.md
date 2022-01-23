---

title: Android中图片占多大内存？
date: 2022-01-17 23:04:23
tags: Android
categories: Android

---

#### **问题引入：**

```kotlin
/**
 * 引起问题的代码
 */
GlideApp.with(imageView.context)
                .asBitmap()
                .load(finalResource)
                .into(object : CustomTarget<Bitmap?>() {
                    override fun onLoadFailed(errorDrawable: Drawable?) {
                        super.onLoadFailed(errorDrawable)
                        Trace.i(TAG, "图片加载失败")
                		}
                		override fun onResourceReady(resource: Bitmap,transition: Transition<in Bitmap?>?) {
                    		imageView.setImageBitmap(resource)
                		}
                		override fun onLoadCleared(placeholder: Drawable?) {
                }
            })
```

上述代码，加载了一张4501*8001，大小为856kb的图片；出现如下问题：Canvas: trying to draw too large(144050004bytes) bitmap`

```java
com.ecovacs.admin.handle.CrashManager	crashInfo=java.lang.RuntimeException: Canvas: trying to draw too large(144050004bytes) bitmap.
	at android.view.DisplayListCanvas.throwIfCannotDraw(DisplayListCanvas.java:260)
	at android.graphics.Canvas.drawBitmap(Canvas.java:1415)
	at android.graphics.drawable.BitmapDrawable.draw(BitmapDrawable.java:528)
	at android.widget.ImageView.onDraw(ImageView.java:1316)
	at android.view.View.draw(View.java:17201)
	at android.view.View.updateDisplayListIfDirty(View.java:16183)
	at android.view.View.draw(View.java:16967)
	at android.view.ViewGroup.drawChild(ViewGroup.java:3727)
	at android.view.ViewGroup.dispatchDraw(ViewGroup.java:3513)
	...
	at com.android.internal.policy.DecorView.draw(DecorView.java:754)
	at android.view.View.updateDisplayListIfDirty(View.java:16183)
	at android.view.ThreadedRenderer.updateViewTreeDisplayList(ThreadedRenderer.java:648)
	at android.view.ThreadedRenderer.updateRootDisplayList(ThreadedRenderer.java:654)
	at android.view.ThreadedRenderer.draw(ThreadedRenderer.java:762)
	at android.view.ViewRootImpl.draw(ViewRootImpl.java:2800)
	at android.view.ViewRootImpl.performDraw(ViewRootImpl.java:2608)
	at android.view.ViewRootImpl.performTraversals(ViewRootImpl.java:2215)
	at android.view.ViewRootImpl.doTraversal(ViewRootImpl.java:1254)
	at android.view.ViewRootImpl$TraversalRunnable.run(ViewRootImpl.java:6338)
	at android.view.Choreographer$CallbackRecord.run(Choreographer.java:874)
	at android.view.Choreographer.doCallbacks(Choreographer.java:686)
	at android.view.Choreographer.doFrame(Choreographer.java:621)
	at android.view.Choreographer$FrameDisplayEventReceiver.run(Choreographer.java:860)
	at android.os.Handler.handleCallback(Handler.java:755)
	at android.os.Handler.dispatchMessage(Handler.java:95)
	at android.os.Looper.loop(Looper.java:154)
	at android.app.ActivityThread.main(ActivityThread.java:6121)
	at java.lang.reflect.Method.invoke(Native Method)
	at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:912)
	at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:802)
```

#### **问题分析：**

1. 首先分析crash的原因，很明显是图片太大造成的，而重点是 `144050004bytes`是指什么的大小。
2. 为什么glide这样使用会造成这样的问题。

因为这张图片是运维工程师上传的，当了解这张图的（4501*8001，856kb）信息是，我们会发现，144050004不是图片的大小（存储大小），而是等于4501x8001x4；为什么是这样的呢，就需要掌握android中图片占内存多少。

#### **android中图片占内存多少**





https://www.jianshu.com/p/51c3a2d19300

https://juejin.cn/post/6865492162445606925
