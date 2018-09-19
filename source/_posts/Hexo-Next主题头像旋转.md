---
title: Hexo Next主题头像旋转
toc: true
date: 2018-08-02 16:32:39
tags: [hexo,next]
categories: [Hexo]
description:
---
Hexo Next主题将头像显示成圆形，鼠标放上去有旋转效果。
<!-- more -->
将头像显示成圆形，鼠标放上去有旋转效果。
找到/themes/next/source/css/_common/components/sidebar/sidebar-author.styl
替换其中的site-author-img和site-author-image:hover属性

```python
# 头像显示圆形
.site-author-image {
  display: block;
  margin: 0 auto;
  padding: $site-author-image-padding;
  max-width: $site-author-image-width;
  height: $site-author-image-height;
  border: site-author-image-border-color;
  /* start*/
  border-radius: 50%
  webkit-transition: 1.4s all;
  moz-transition: 1.4s all;
  ms-transition: 1.4s all;
  transition: 1.4s all;
  /* end */
}
# 头像旋转事件
/* start */
.site-author-image:hover {
  background-color: #55DAE1;
  webkit-transform: rotate(360deg) scale(1.1);
  moz-transform: rotate(360deg) scale(1.1);
  ms-transform: rotate(360deg) scale(1.1);
  transform: rotate(360deg) scale(1.1);
}
/* end */
```