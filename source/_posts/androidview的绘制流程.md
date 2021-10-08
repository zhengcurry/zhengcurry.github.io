---
title: Android view的绘制流程
date: 2021-10-04 13:21:09
tags: Android
categories: Android
 
---

> view的绘制流程分三步：测量measure 布局layout 绘制draw
>
> 测量是得到view的大小
>
> 布局是将view放置合适的位置
>
> 绘制是把view展现出来

具体源码整理如下[流程图](https://www.processon.com/view/link/61600cfbf346fb0e99a6d639)

![源码流程图](源码流程图.png)



**总结：**

view的绘制流程，是从DecorView开始的（加载DecorView的过程，涉及到activity的加载流程），经过measure函数，计算子view、父view的大小，然后执行layout函数，再执行draw。了解view绘制流程，可以从中得知，自定义view的步骤



https://www.jianshu.com/p/5a71014e7b1b

https://blog.csdn.net/sinat_27154507/article/details/79748010

https://www.cnblogs.com/andy-songwei/p/10955062.html

https://blog.csdn.net/Innost/article/details/6172893

https://blog.csdn.net/a553181867/article/details/51477040
