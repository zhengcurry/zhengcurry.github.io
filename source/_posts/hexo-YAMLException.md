---
title: hexo YAMLException
date: 2021-09-24 15:00:22
tags: hexo
categories: hexo

---

保存准备发布文章时，提示如下报错：

```
ERROR {
  err: YAMLException: can not read a block mapping entry; a multiline key may not be an implicit key at line 5, column 1:
...
}
```

问题原因：**缺少空格**！

```kotlin
//错误示例（categories:hexo）
title: hexo YAMLException
date: 2021-09-24 15:00:22
tags: hexo
categories:hexo
```

```kotlin
//正确示例（categories: hexo）
title: hexo YAMLException
date: 2021-09-24 15:00:22
tags: hexo
categories: hexo
```



[官方说明](https://hexo.io/zh-cn/docs/troubleshooting.html#YAML-%E8%A7%A3%E6%9E%90%E9%94%99%E8%AF%AF)

![image-20210924151457951](image-20210924151457951.png)

