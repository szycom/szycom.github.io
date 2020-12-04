---
title: getScriptPath
date: 2020-08-19 23:39:59
tags: ssh
categories: linux
---
```
# 获取脚本工作目录
basedir=$(readlink -f $BASH_SOURCE)
basedir=${basedir%/*}
# 或则
# basedir=$(dirname ${basedir})
```
