---
title: Linux命令总结
toc: true
date: 2018-10-25 23:42:19
tags: [linux,timeout,sudo]
categories: [技术]
description:
---

常用Linux命令总结

<!--more-->
1. timeout 命令

timeout默认单位是秒，后缀”s”代表秒(默认值)，”m”代表分，”h”代表小时，”d”代表天。
```shell
timeout 10 top //代表top命令运行10s后自动结束
```

2. sudo
sudo -E : 代表使用当前的环境变量
