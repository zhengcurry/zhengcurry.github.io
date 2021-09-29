---
title: Github+Hexo搭建笔记
date: 2021-09-18 16:51:31
tags: hexo
categories: hexo
---

#### **一. Github**

1. gihub中创建仓库，仓库名为：<github昵称>.github.io

   ![image-20210918165516634](image-20210918165516634.png)

   >2021年8月13日之后github不在支持密码登录，使用**personal access token**替代
   >
   >在github的Settings -- Developer settings -- personal access token 中配置

#### **二. Hexo**

1. 安装hexo

   ```javascript
   npm install -g hexo
   ```

2. hexo初始化

   ```javascript
   hexo init
   ```

3. 本地运行命令

   ```javascript
   hexo s // hexo server
   ```

4. 部署到github，打开hexo文件夹中的_config.xml，文末添加deploy的对应内容，参考如下：

   ```
   # Deployment
   ## Docs: https://hexo.io/docs/one-command-deployment
   deploy:
     type: git
     repo: https://github.com/zhengcurry/zhengcurry.github.io
     branch: master
   ```

   然后执行

   ```javascript
   npm install hexo-deployer-git --save
   ```

   部署：

   ```undefined
   hexo clean
   
   hexo deploy
   ```

5. 新建文章

   ```
   hexo new xxxx
   ```

6. 添加图片

   ```undefined
   npm install hexo-asset-image --save
   ```

   hexo文件夹中的_config.xml，把post_asset_folder值改为false

   打开/node_modules/hexo-asset-image/index.js文件，替换如下

   ```bash
   'use strict';
   var cheerio = require('cheerio');
   
   // http://stackoverflow.com/questions/14480345/how-to-get-the-nth-occurrence-in-a-string
   function getPosition(str, m, i) {
     return str.split(m, i).join(m).length;
   }
   
   var version = String(hexo.version).split('.');
   hexo.extend.filter.register('after_post_render', function(data){
     var config = hexo.config;
     if(config.post_asset_folder){
           var link = data.permalink;
       if(version.length > 0 && Number(version[0]) == 3)
          var beginPos = getPosition(link, '/', 1) + 1;
       else
          var beginPos = getPosition(link, '/', 3) + 1;
       // In hexo 3.1.1, the permalink of "about" page is like ".../about/index.html".
       var endPos = link.lastIndexOf('/') + 1;
       link = link.substring(beginPos, endPos);
   
       var toprocess = ['excerpt', 'more', 'content'];
       for(var i = 0; i < toprocess.length; i++){
         var key = toprocess[i];
    
         var $ = cheerio.load(data[key], {
           ignoreWhitespace: false,
           xmlMode: false,
           lowerCaseTags: false,
           decodeEntities: false
         });
   
         $('img').each(function(){
           if ($(this).attr('src')){
               // For windows style path, we replace '\' to '/'.
               var src = $(this).attr('src').replace('\\', '/');
               if(!/http[s]*.*|\/\/.*/.test(src) &&
                  !/^\s*\//.test(src)) {
                 // For "about" page, the first part of "src" can't be removed.
                 // In addition, to support multi-level local directory.
                 var linkArray = link.split('/').filter(function(elem){
                   return elem != '';
                 });
                 var srcArray = src.split('/').filter(function(elem){
                   return elem != '' && elem != '.';
                 });
                 if(srcArray.length > 1)
                   srcArray.shift();
                 src = srcArray.join('/');
                 $(this).attr('src', config.root + link + src);
                 console.info&&console.info("update link as:-->"+config.root + link + src);
               }
           }else{
               console.info&&console.info("no src attr, skipped...");
               console.info&&console.info($(this));
           }
         });
         data[key] = $.html();
       }
     }
   });
   ```

   然后即可添加图片，参考[Typora](#Typora)

   

7. 删除文章

   删除本地对应文件，然后执行如下命令

   ```
   hexo g
   hexo d
   ```

####  **Typora**

配置如下，将图片拖拽到md文档中，即可复制到指定目录下：

![image-20210923104949941](image-20210923104949941.png)

在md文档中，直接使用图片名称，预览或发布后即可看到图片

![image-20210923105008730](image-20210923105008730.png)











**参考：**

https://www.jianshu.com/p/390f202c5b0e

https://zhuanlan.zhihu.com/p/155996962

https://www.jianshu.com/p/f72aaad7b852

