---
title: hexo多电脑部署
date: 2021-09-25 10:33:30
tags: hexo
categories: hexo

---

> 当在办公室或家里都想输出文章，怎么办呢？

在github已有仓库（xxx.github.io）中，新建分支（比如：命名为build分支），把已经创建的本地文件全部上传；

这里可以把build作为默认分支（`新建的文章默认是提交到master分支上的`），这样每次有变更后，就如同提交代码一样，把本地文件提交到build上。

在新的电脑上拉取build分支即可同步已经配置好的相关内容；

clone到本地后，要确认相关环境是否都配置过，然后需要在clone的本地文件中执行 `npm install` 和 `npm install -g hexo` 

这样就可以继续输出文章啦~
