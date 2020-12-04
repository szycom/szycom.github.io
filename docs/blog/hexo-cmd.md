---
title: hex常用命令
date: 2020-05-31 15:01:19
tags: [hexo]
categories: [博客搭建]
---
# hexo命令
使用hexo中使用到命令
<!--more-->

```
# 从代码库拉取工作目录
git pull origin hexo
# 创建新的文章
hexo new "post"
# 将md转成静态资源，并开启本地服务
hexo clean && hexo g && hexo s
# 将md转成静态资源，并发布到git
hexo clean && hexo g && hexo d
# 推送工作目录到git
git push origin hexo
```
