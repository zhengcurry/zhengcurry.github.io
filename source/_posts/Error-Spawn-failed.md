---
title: 'Error: Spawn failed'
date: 2021-09-25 11:53:45
tags: hexo
categories: hexo

---

执行`hexo deploy`出现如下报错：

```javascript
fatal: in unpopulated submodule '.deploy_git'
FATAL {
  err: Error: Spawn failed
      at ChildProcess.<anonymous> (/Volumes/curry/blog/node_modules/hexo-util/lib/spawn.js:51:21)
      at ChildProcess.emit (events.js:400:28)
      at Process.ChildProcess._handle.onexit (internal/child_process.js:277:12) {
    code: 128
  }
} Something's wrong. Maybe you can find the solution here: %s https://hexo.io/docs/troubleshooting.html
```

**解决方式**

```java
//删除git提交内容文件夹
rm -rf .deploy_git/

//依次执行
git config --global core.autocrlf false
hexo clean 
hexo generate   
hexo deploy
```

> 执行过以上命令后，可能需要尝试多次`hexo deploy`（实际操作中我尝试了两次 0.0）
