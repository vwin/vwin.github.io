---
title: Python-lambda函数
toc: true
date: 2018-12-26 04:08:54
tags: [python,lambda]
categories: [Python]
description:
---
Python 中定义函数有两种方法，一种是用常规方式 def 定义，函数要指定名字，第二种是用 lambda 定义，不需要指定名字，称为 Lambda 函数。

Lambda 函数又称匿名函数，匿名函数就是没有名字的函数，函数没有名字也行？当然可以啦。有些函数如果只是临时一用，而且它的业务逻辑也很简单时，就没必要非给它取个名字不可。

好比电影里面的群众演员，往往他们的戏份很少，最多是衬托主演，跑跑龙套，他们需要名字吗？不需要，因为他们仅仅只是临时出镜，下次可能就用不着了，所以犯不着费心思给他们每个人编个号取个名字，毕竟取个优雅的名字是很费劲的事情。

<!--more-->
先来看个简单 lambda 函数
```python
>>> lambda x, y : x+y
<function <lambda> at 0x102bc1c80>
```
x 和 y 是函数的两个参数，冒号后面的表达式是函数的返回值，你能一眼看出这个函数就是是在求两个变量的和，但作为一个函数，没有名字如何使用呢？这里我们暂且给这个匿名函数绑定一个名字，这样使得我们调用匿名函数成为可能
```python
>>> add = lambda x, y : x+y
>>> add
<function <lambda> at 0x102bc2140>
>>> add(1,2)
3
```
它等同于常规函数
```python
>>> def add2(x, y):
...     return x+y
...
>>> add2
<function add2 at 0x102bc1c80>
>>> add2(1,2)
3
```
如果定义匿名函数，还要给它绑定一个名字的话，有点画蛇添足，通常是直接使用 lambda 函数。那么 lamdba 函数的正确使用场景在哪呢？

# 函数式编程

尽管 Python 算不上是一门纯函数式编程语言，但它本身提供了很多函数式编程的特性，像 map、reduce、filter、sorted 这些函数都支持函数作为参数，lambda 函数就可以应用在函数式编程中。

请看题：一个整数列表，要求按照列表中元素的绝对值大小升序排列，你会怎么做？思考一分钟往下看
```python
>>> list1 = [3,5,-4,-1,0,-2,-6]
>>> sorted(list1, key=lambda x: abs(x))
[0, -1, -2, 3, -4, 5, -6]
```
排序函数 sorted 支持接收一个函数作为参数，该参数作为 sorted 的排序依据，这里按照列表元素的绝对值进行排序，当然，我也可以用普通函数来实现：
```python
>>> def foo(x):
...     return abs(x)
...
>>> sorted(list1, key=foo)
[0, -1, -2, 3, -4, 5, -6]
```
只不过是这种方式代码看起来不够 Pythonic 而已。

# 闭包

闭包本身是一个晦涩难懂的概念，它可以专门单独用一篇文章来介绍，不过在这里我们可以简单粗暴地理解为闭包就是一个定义在函数内部的函数，闭包使得变量即使脱离了该函数的作用域范围也依然能被访问到。

来看一个用 lambda 函数作为闭包的例子。
```python
>>> def my_add(n):
...     return lambda x:x+n
...
>>> add_3 = my_add(3)
>>> add_3(7)
10
```
这里的 lambda 函数就是一个闭包，在全局作用域范围中，add_3(7) 可以正常执行且返回值为10，之所以返回10是因为在 my_add 局部作用域中，变量 n 的值在闭包的作用使得它在全局作用域也可以被访问到。

换成常规函数也可以实现闭包，只不过是这种方式稍显啰嗦。
```python
>>> def my_add(n):
...     def wrapper(x):
...         return x+n
...     return wrapper
...
>>> add_5 = my_add(5)
>>> add_5(2)
7
```
那么是不是任何情况 lambda 函数都要比常规函数更清晰明了呢？看这个例子：
```python
f = lambda x: [[y for j, y in enumerate(set(x)) if (i >> j) & 1] for i in range(2**len(set(x)))]
```
这是一个返回某个集合的所有子集的 lambda 函数，你看明白了吗？我是很难一眼看出来

zen of python 中有这样一句话是 Explicit is better than implicit(明了胜于晦涩)。记住，如果用 lambda 函数不能使你的代码变得更清晰时，这时你就要考虑使用常规的方式来定义函数。
