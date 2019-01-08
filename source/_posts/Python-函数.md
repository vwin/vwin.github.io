---
title: Python-函数
toc: true
date: 2018-12-25 22:28:02
tags: [函数,python]
categories: [Python]
description:
---
正确理解 Python函数，能够帮助我们更好地理解 Python 装饰器、匿名函数（lambda）、函数式编程等高阶技术。

函数（Function）作为程序语言中不可或缺的一部分，太稀松平常了。但函数作为第一类对象（First-Class Object）却是 Python 函数的一大特性。那到底什么是第一类对象（First-Class Object）呢？

<!--more-->

# 函数是对象
在 Python 中万物皆为对象，函数也不例外，函数作为对象可以赋值给一个变量、可以作为元素添加到集合对象中、可作为参数值传递给其它函数，还可以当做函数的返回值，这些特性就是第一类对象所特有的。
先来看一个简单的例子
```python
>>> def foo(text):
...     return len(text)
...
>>> foo("python")
6
```

这是一个再简单不过的函数，用于计算参数 text 的长度，调用函数就是函数名后面跟一个括号，再附带一个参数，返回值是一个整数。

函数身为一个对象，拥有对象模型的三个通用属性：id、类型、和值。

```python
>>> id(foo)
4361313816
>>> type(foo)
<class 'function'>
>>> foo
<function foo at 0x103f45e18>
```

作为对象，函数可以赋值给一个变量

```python
>>> bar = foo
```

![](https://ws2.sinaimg.cn/large/006tNbRwly1fyjznyxpgaj308w052t8v.jpg)

赋值给另外一个变量时，函数并不会被调用，仅仅是在函数对象上绑定一个新的名字而已。

```python
>>> bar("python")
6
```

同理，你还可以把该函数赋值给更多的变量，唯一变化的是该函数对象的引用计数不断地增加，本质上这些变量最终指向的都是同一个函数对象。

```python
>>> a = foo
>>> b = foo
>>> c = bar
>>> a is b is c
True
```

# 函数可以存储在容器
容器对象（list、dict、set等）中可以存放任何对象，包括整数、字符串，函数也可以作存放到容器对象中，例如
```python
>>> funcs = [foo, str, len]
>>> funcs
[<function foo at 0x103f45e18>, <class 'str'>, <built-in function len>]
>>> for f in funcs:
...     print(f("hello"))
...
5
hello
5
```

foo 是我们自定义的函数，str 和 len 是两个内置函数。for 循环逐个地迭代出列表中的每个元素时，函数对象赋值给了 f 变量，调用 f(“hello”) 与 调用 foo(“hello”) 本质是一样的效果，每次 f 都重新指向一个新的函数对象。当然，你也可以使用列表的索引定位到元素来调用函数。
```python
>>> funcs[0]("Python")
# 等效于 foo("Python")
6
```
# 函数可以作为参数
函数还可以作为参数值传递给另外一个函数，例如：
```python
>>> def show(func):
...     size = func("python") # 等效于 foo("Python") 
...     print ("length of string is : %s" % size)
...
>>> show(foo)
length of string is : 6
```

# 函数可以作为返回值
函数作为另外一个函数的返回值，例如：
```python
>>> def nick():
...     return foo
>>> nick
<function nick at 0x106b549d8>
>>> a = nick()
>>> a
<function foo at 0x10692ae18>
>>> a("python")
6
```
还可以简写为
```python
>>> nick()("python")
6
```
函数接受一个或多个函数作为输入或者函数输出（返回）的值是函数时，我们称这样的函数为高阶函数，比如上面的 show 和 nick 都属于高阶函数。

Python内置函数中，典型的高阶函数是 map 函数，map 接受一个函数和一个迭代对象作为参数，调用 map 时，依次迭代把迭代对象的元素作为参数调用该函数。
```python
>>> map(foo, ["the","map","of","python"])
>>> lens = map(foo, ["the","map","of","python"])
>>> list(lens)
[3, 3, 2, 6]
```
map 函数的作用相当于：
```python
>>> [foo(i) for i in ["the","map","of","python"]]
[3, 3, 2, 6]
```
只不过 map 的运行效率更快一点。

# 函数可以嵌套
Python还允许函数中定义函数，这种函数叫嵌套函数。
```python
>>> def get_length(text):
...     def clean(t):           # 2
...         return t[1:]
...     new_text = clean(text)  # 1
...     return len(new_text)
...
>>> get_length("python")
5
>>>
```
这个函数的目的是去除字符串的第一个字符后再计算它的长度，尽管函数本身的意义不大，但能足够说明嵌套函数。get_length 调用时，先执行1处代码，发现有调用 clean 函数，于是接着执行2中的代码，把返回值赋值给了 new_text ，再继续执行后续代码。
```python
>>> clean("python")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'clean' is not defined
```
函数中里面嵌套的函数不能在函数外面访问，只能是在函数内部使用，超出了外部函数的做用域就无效了。

# 实现了 __call__ 的类也可以作为函数
对于一个自定义的类，如果实现了 __call__ 方法，那么该类的实例对象的行为就是一个函数，是一个可以被调用（callable)的对象。例如：
```python
class Add:
    def __init__(self, n):
         self.n = n
    def __call__(self, x):
        return self.n + x

>>> add = Add(1)
>>> add(4)
>>> 5
```
执行 add(4) 相当于调用 Add._call__(add, 4)，self 就是实例对象 add，self.n 等于 1，所以返回值为 1+4
```python
add(4)
  ||
Add(1)(4)
  ||
Add.__call__(add, 4)
```
确定对象是否为可调用对象可以用内置函数callable来判断。
```python
>>> callable(foo)
True
>>> callable(1)
False
>>> callable(int)
True
```
# 总结
Python中包含函数在内的一切皆为对象，函数作为第一类对象，支持赋值给变量，作为参数传递给其它函数，作为其它函数的返回值，支持函数的嵌套，实现了__call__方法的类实例对象也可以当做函数被调用。

