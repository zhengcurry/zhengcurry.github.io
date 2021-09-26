---
title: 'git@github.com: Permission denied (publickey).'
date: 2021-09-25 12:00:56
tags: git
categories: git
---

#### **问题描述**

想通过命令删除远程分支 `git push origin --delete hexo` 时，出现

```java
curry@MacBook-Pro blog % git push origin --delete hexo       
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

#### **解决方法**

1. `git config --global --list` 通过此命令查看全局邮箱是否与github注册的一致；

   > 我这里不一致，因此需要执行第二部；若一直可以跳过

2. `git config --global user.email "邮箱"` 通过此命令配置全局邮箱；

   > 若用户名也想更改，执行`git config --global user.name "xxx"`

3. `ssh-keygen -t rsa -C "邮箱"` 通过此命令生成ssh；

   > 输入命令，一直回车即可；命令中会提示生成的文件路径；
   >
   > 例：Your public key has been saved in /Users/curry/.ssh/id_rsa.pub

4. 将第三步骤中生成的id_rsa.pub文件其内容全部粘贴到github - Settings - SSH and GPG keys - New SSH Key中

   ![image-20210925123231078](image-20210925123231078.png)

5. 执行过以上步骤后，通常可以解决问题！

6. 可以通过 `ssh -T git@github.com `  验证是否成功；若不成功，则执行（未出现，待验证）

   ```
   ssh-agent -s
   ssh-add ~/.ssh/id_rsa
   ```
