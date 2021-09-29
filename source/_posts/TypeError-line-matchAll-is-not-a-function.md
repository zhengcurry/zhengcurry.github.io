---
title: 'TypeError: line.matchAll is not a function'
date: 2021-09-25 11:14:32
tags: hexo
categroies: hexo
---

执行`hexo server`，报错：

![image-20210925111618240](image-20210925111618240.png)



此问题是node版本较老导致的；查看node版本，是v10.15.3，因此需要进行升级；

**node升级（MAC）**

1. 安装node版本管理模块

   ```java
   npm -g i -n
   //mac电脑，若无权限，需执行：
   sudo npm -g i -n
   ```

   

2. 执行升级命令

   ```java
   n stable  //安装稳定版本
   n latest  //安装最新版本
   n (指定版本号)  //n 14.17.6
   ```

   

