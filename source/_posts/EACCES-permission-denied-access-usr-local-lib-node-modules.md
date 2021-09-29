---
title: 'EACCES: permission denied, access ''/usr/local/lib/node_modules'''
date: 2021-09-25 10:49:05
tags: hexo
categories: hexo

---

在Mac电脑上，执行`npm install -g hexo`，出现入下的报错：

![image-20210925105028907](image-20210925105028907.png)

提示没有权限；命令前加sudo即可：

```
sudo hexo install -g 
```

