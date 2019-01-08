---
title: Jenkins HTML报告样式无法显示问题解决
toc: true
date: 2018-10-11 12:52:45
tags: [持续集成,Jenkins,HTML]
categories: [持续集成]
description:
---

在部署jenkins持续集成的过程中经常会用到HTML Publisher Plugin插件来显示测试报告。
为了测试报告样式的美观以及易读，测试报告中经常会搭配CSS和JS实现自定义的样式或动画效果。
Jenkins中在访问有自定义样式或动画效果的测试报告时，展示的样式往往会出乎你的想象，面目全非，样式错乱，动画无法展示等等问题。打开浏览器开发工具控制台，可以看到CSS、JS加载失败。
<!--more-->

## 问题
![](https://ws4.sinaimg.cn/large/006tNbRwly1fw46y2sfysj30iv0cyjum.jpg)

## 原因
Jenkins为了避免受到恶意HTML/JS文件的攻击，会默认将安全策略CSP设置为：
sandbox; default-src 'none'; img-src 'self'; style-src 'self';

在此配置下，只允许加载：

1. Jenkins服务器上托管的CSS文件
2. Jenkins服务器上托管的图片文件

以下形式的内容都会被禁止：

- JavaScript
- plugins (object/embed)
- HTML中的内联样式表（Inline style sheets），以及引用的外站CSS文件
- HTML中的内联图片（Inline image definitions），以及外站引用的图片文件
- frames
- web fonts
- XHR/AJAX

## 解决
### 方法一
修改CSP(Content Security Policy)的默认配置，到Jenkins系统管理->脚本命令行，执行以下Groovy命令，然后点击运行。配置完成后，重新构建原有项目，HTML页面即可正常显示。
```shell
System.setProperty("hudson.model.DirectoryBrowserSupport.CSP", "")
```
![](https://ws4.sinaimg.cn/large/006tNbRwly1fw471l5bsvj30u0097ab2.jpg)
然而当你重新启动jenkins时，你会发现，HTML页面再次面目全非，CSP恢复为默认配置，因此这个办法只是临时方案

### 方法二
利用jenkins的以下两个插件可以巧妙解决这个问题
1. Startup Trigger: 可实现在Jenkins节点(master/slave)启动时触发构建；
2. Groovy plugin: 可实现直接执行Groovy代码。

解决步骤如下：
1. Jenkins中新建一个Job，该Job专用Jenkins启动时执行的配置命令。
2. 在“构建触发器”模块，选择“Build when job nodes start”选项。

![](https://ws3.sinaimg.cn/large/006tNbRwly1fw472m709uj30le07qaar.jpg)

3. 在“构建”模块，选择“Execute system Groovy script”，执行如下Groovy命令：
```shell
System.setProperty("hudson.model.DirectoryBrowserSupport.CSP", "")
```

![](https://ws1.sinaimg.cn/large/006tNbRwly1fw4737k557j30so0b1jsg.jpg)

立即构建此Job，返回原项目重新构建，再次访问HTML页面即可发现问题已解决。
