---
title: Hexo Next主题开启字数统计和阅读时长统计
toc: true
date: 2018-08-02 16:37:32
tags: [hexo,next]
categories: [Hexo]
description:
---
Hexo Next主题开启字数统计和阅读时长统计
<!-- more -->
>注：Next主题版本 v6.3.0
## 安装 hexo-symbols-count-time
在博客目录下执行：
```
npm install hexo-symbols-count-time
or
yarn add hexo-symbols-count-time
```
## 配置
1. hexo配置
在博客根目录下的_config.yaml,增加如下配置：
>注：增加该配置需要重启hexo
>hexo clean && hexo s

```shell
symbols_count_time:
  symbols: true # 文章字数
  time: true # 阅读时长
  total_symbols: true # 所有文章总字数
  total_time: true # 所有文章阅读中时长
```
2. next主题配置
在themes/next/_config.yaml，修改如下配置：
>注：无需重启

```shell
symbols_count_time:
  separated_meta: true  # 是否换行显示 字数统计 及 阅读时长
  item_text_post: true  # 文章 字数统计 阅读时长 使用图标 还是 文本表示
  item_text_total: false # 博客底部统计 字数统计 阅读时长 使用图标 还是 文本表示
  awl: 4
  wpm: 275
```

