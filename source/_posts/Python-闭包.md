---
title: Python-闭包
toc: true
date: 2018-12-26 04:11:10
tags: [闭包,python]
categories: [Python]
description:
---
什么是闭包？闭包有什么用？为什么要用闭包？今天我们就带着这3个问题来一步一步认识闭包。闭包和函数紧密联系在一起，介绍闭包前有必要先介绍一些背景知识，诸如嵌套函数、变量的作用域等概念
<!--more-->

# 作用域
作用域是程序运行时变量可被访问的范围，定义在函数内的变量是局部变量，局部变量的作用范围只能是函数内部范围内，它不能在函数外引用。
```python
def foo():
    num = 10 # 局部变量
print(num)  # NameError: name 'num' is not defined
```
定义在模块最外层的变量是全局变量，它是全局范围内可见的，当然在函数里面也可以读取到全局变量的。例如：
```python
num = 10 # 全局变量
def foo():
    print(num)  # 10
```
# 嵌套函数
函数不仅可以定义在模块的最外层，还可以定义在另外一个函数的内部，像这种定义在函数里面的函数称之为嵌套函数（nested function）例如：
```python
def print_msg():
    # print_msg 是外围函数
    msg = "zen of python"

    def printer():
        # printer是嵌套函数
        print(msg)
    printer()
# 输出 zen of python
print_msg()
```
对于嵌套函数，它可以访问到其外层作用域中声明的非局部（non-local）变量，比如代码示例中的变量 msg 可以被嵌套函数 printer 正常访问。

那么有没有一种可能即使脱离了函数本身的作用范围，局部变量还可以被访问得到呢？答案是闭包

# 什么是闭包
函数身为第一类对象，它可以作为函数的返回值返回，现在我们来考虑如下的例子：
```python
def print_msg():
    # print_msg 是外围函数
    msg = "zen of python"
    def printer():
        # printer 是嵌套函数
        print(msg)
    return printer

another = print_msg()
# 输出 zen of python
another()
```
这段代码和前面例子的效果完全一样，同样输出 "zen of python"。不同的地方在于内部函数 printer 直接作为返回值返回了。

一般情况下，函数中的局部变量仅在函数的执行期间可用，一旦 print_msg() 执行过后，我们会认为 msg变量将不再可用。然而，在这里我们发现 print_msg 执行完之后，在调用 another 的时候 msg 变量的值正常输出了，这就是闭包的作用，闭包使得局部变量在函数外被访问成为可能。

看完这个例子，我们再来定义闭包，维基百科上的解释是:

>在计算机科学中，闭包（Closure）是词法闭包（Lexical Closure）的简称，是引用了自由变量的函数。这个被引用的自由变量将和这个函数一同存在，即使已经离开了创造它的环境也不例外。所以，有另一种说法认为闭包是由函数和与其相关的引用环境组合而成的实体。

这里的 another 就是一个闭包，闭包本质上是一个函数，它有两部分组成，printer 函数和变量 msg。闭包使得这些变量的值始终保存在内存中。

<b>闭包，顾名思义，就是一个封闭的包裹，里面包裹着自由变量，就像在类里面定义的属性值一样，自由变量的可见范围随同包裹，哪里可以访问到这个包裹，哪里就可以访问到这个自由变量。</b>

# 为什么要使用闭包
闭包避免了使用全局变量，此外，闭包允许将函数与其所操作的某些数据（环境）关连起来。这一点与面向对象编程是非常类似的，在面对象编程中，对象允许我们将某些数据（对象的属性）与一个或者多个方法相关联。

一般来说，当对象中只有一个方法时，这时使用闭包是更好的选择。来看一个例子：
```python
def adder(x):
    def wrapper(y):
        return x + y
    return wrapper

adder5 = adder(5)
# 输出 15
adder5(10)
# 输出 11
adder5(6)
```
这比用类来实现更优雅，此外装饰器也是基于闭包的一种应用场景。

所有函数都有一个 __closure__属性，如果这个函数是一个闭包的话，那么它返回的是一个由 cell 对象 组成的元组对象。cell 对象的cell_contents 属性就是闭包中的自由变量。
```python
>>> adder.__closure__
>>> adder5.__closure__
(<cell at 0x103075910: int object at 0x7fd251604518>,)
>>> adder5.__closure__[0].cell_contents
5
```
这解释了为什么局部变量脱离函数之后，还可以在函数之外被访问的原因的，因为它存储在了闭包的 cell_contents中了。
