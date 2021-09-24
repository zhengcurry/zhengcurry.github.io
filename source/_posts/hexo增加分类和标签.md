---
title: hexo增加分类和标签
date: 2021-09-24 14:58:15
tags: hexo
categories: hexo
---

#### **创建“分类”选项**

在hexo目录下，执行：

```
hexo new page categories
```

hexo\source目录下会生成categories文件夹；编辑categories\index.md，添加	type: "categories"；添加后保存并关闭文件。

```
---
title: 分类
date: 2021-09-24 14:43:00
type: categories
---
```

**给文章添加“categories”属性**

打开需要添加分类的文章，为其添加categories属性。

下方的categories:hexo表示这篇文章添加到到“hexo”这个分类。注意：一篇文章只会添加到一个分类中，如果是多个默认放到第一个分类中。

```
---
title: hexo增加分类和标签
date: 2021-09-24 14:43:00
categories: hexo
---
```



#### **创建“标签”选项**（与添加分类操作类似）

在hexo目录下，执行：

```
hexo new page tags
```

hexo\source目录下会生成tags文件夹；编辑tags\index.md，添加	type: "tags"；添加后保存并关闭文件。

```
---
title: 标签
date: 2021-09-24 14:43:00
type: tags
---
```

**给文章添加“tags”属性**

打开需要添加标签的文章，为其添加tags属性。

```
---
title: hexo增加分类和标签
date: 2019-04-24 15:40:24
categories: hexo
tags: hexo
---
```

**参考：**

https://juejin.cn/post/6844903830216261645

