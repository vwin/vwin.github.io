---
title: Bootstrap后台管理系统框架收集
toc: true
date: 2018-09-18 20:36:21
tags: [后台关系,bootstrap,vue,laravel,element ui]
categories: [工具]
description:
---
后台管理系统框架收集
<!--more-->

## 框架整理
1. [灰黑色后台框架](http://pan.baidu.com/s/1kVGodJD)
2. [论如何把后台管理系统写出花(关于后台开发的相关技术）](https://segmentfault.com/a/1190000005822465)
3. [最新的技术-Vue后台管理](https://www.awesomes.cn/)
4. [AdminLTE](https://almsaeedstudio.com/)
5. 可以使用YII建立RABC权限管理[yii2+AdminLTE搭建完美后台并实现rbac权限控制实例教程](https://segmentfault.com/a/1190000004963496)
6. Laravel建立RABC权限管理 [在Laravel5.* 中使用AdminLTE](https://segmentfault.com/a/1190000003917210)
7. [flatlab](http://download.csdn.net/tag/flatlab)
8. [laravel-vue-cms](https://github.com/liukaijv/laravel-vue-cms)
9. [支付宝蚂蚁前段框架](https://ant.design)
10. [饿了么开源的后台管理框架](http://element.eleme.io/#/zh-CN)
11. [Larval 5.3 Rbac 后台实例](https://laravel-china.org/topics/3609)
12. [2016 Top20后端管理模板](https://colorlib.com/wp/free-html5-admin-dashboard-templates/)
13. [模板之家国内后台管理框架收集](http://www.cssmoban.com/cssthemes/houtaimoban/)

## 选择框架
后台管理系统框架非常多，经过考虑，选择[饿了么的开源框架](http://element.eleme.io/#/zh-CN)，原因是：该框架大气简洁，最重要的一点是使用最新的前端技术[Vue.js](http://cn.vuejs.org/)，Vue简单易上手，有许多现成的脚手架可以直接拿来用，在使用该框架时，也可以顺手学学目前最流行的前端技术。

## 打包工具Webpack了解
[Webpack](http://webpackdoc.com/)是当下最热门的前端资源模块化管理和打包工具。它可以将许多松散的模块按照依赖和规则打包成符合生产环境部署的前端资源。还可以将按需加载的模块进行代码分隔，等到实际需要的时候再异步加载。通过 loader 的转换，任何形式的资源都可以视作模块，比如 CommonJs 模块、 AMD 模块、 ES6 模块、CSS、图片、 JSON、Coffeescript、 LESS 等。

## 包管里工具npm
在正式开始饿了么管理框架学习之前，我们先认识一下npm。

[npm](http://www.runoob.com/nodejs/nodejs-npm.html)是什么东东？npm其实是Node.js的包管理工具（package manager）。

为啥我们需要一个包管理工具呢？因为我们在Node.js上开发时，会用到很多别人写的JavaScript代码。如果我们要使用别人写的某个包，每次都根据名称搜索一下官方网站，下载代码，解压，再使用，非常繁琐。于是一个集中管理的工具应运而生：大家都把自己开发的模块打包后放到npm官网上，如果要使用，直接通过npm安装就可以直接用，不用管代码存在哪，应该从哪下载。

更重要的是，如果我们要使用模块A，而模块A又依赖于模块B，模块B又依赖于模块X和模块Y，npm可以根据依赖关系，把所有依赖的包都下载下来并管理起来。否则，靠我们自己手动管理，肯定既麻烦又容易出错。

简而言之，NPM是随同NodeJS一起安装的包管理工具([安装NodeJS](http://www.runoob.com/nodejs/nodejs-install-setup.html))，能解决NodeJS代码部署上的很多问题，常见的使用场景有以下几种：

允许用户从NPM服务器下载别人编写的第三方包到本地使用。

允许用户从NPM服务器下载并安装别人编写的命令行程序到本地使用。

允许用户将自己编写的包或命令行程序上传到NPM服务器供别人使用。

讲了这么多，npm究竟在哪？

其实npm已经在Node.js安装的时候顺带装好了。我们在命令提示符或者终端输入npm -v，应该看到类似的输出：

1、查看npm的版本
```shell
$ npm -v
```

2、使用 npm 命令安装模块
npm 安装 Node.js 模块语法格式如下：

```shell
$ npm install <Module Name>
```

以下实例，我们使用 npm 命令安装常用的 Node.js web框架模块 express:

```shell
$ npm install express
```
安装好之后，express 包就放在了工程目录下的 node_modules 目录中，因此在代码中只需要通过 require('express') 的方式就好，无需指定第三方包路径。

```shell
var express = require('express');
```

## 示例
[使用element开发的后台框架示例](http://www.cnblogs.com/taylorchen/p/6083099.html)
演示地址：https://taylorchen709.github....
源码地址：https://github.com/taylorchen...

使用：
```shell
# install dependencies
npm install

# serve with hot reload at localhost:8081
npm run dev

# build for production with minification
npm run build
```

## Laravel-Administrator
[【扩展推荐】管理员后台快速生成工具 Administrator "增强版" 分享](https://laravel-china.org/topics/2301/extension-administrator-background-fast-generation-tool-administrator-enhanced-version-sharing)
该后台非常好用，十分钟就可以搭建起一个简单的管理后台
## laravel-admin
该管理后台非常强大，使用Laravel AdminLTE开发，可以快速实现增删改查功能及角色管理。

项目：[GitHub地址: z-song/laravel-admin](https://almsaeedstudio.com/)
