---
title: Python与设计模式 - 单例模式
toc: true
date: 2018-12-24 04:57:03
tags: [设计模式,单例模式]
categories: [设计模式]
description:
---
# 单例模式
单例模式（Singleton Pattern）是一种常用的软件设计模式，该模式的主要目的是确保某一个类只有一个实例存在。当你希望在整个系统中，某个类只能出现一个实例时，单例对象就能派上用场。

比如，某个服务器程序的配置信息存放在一个文件中，客户端通过一个 AppConfig 的类来读取配置文件的信息。如果在程序运行期间，有很多地方都需要使用配置文件的内容，也就是说，很多地方都需要创建 AppConfig 对象的实例，这就导致系统中存在多个 AppConfig 的实例对象，而这样会严重浪费内存资源，尤其是在配置文件内容很多的情况下。事实上，类似 AppConfig 这样的类，我们希望在程序运行期间只存在一个实例对象。

在 Python 中，我们可以用多种方法来实现单例模式。
<!--more-->
# 实现单例模式的几种方式
## 使用模块
其实，Python 的模块就是天然的单例模式，因为模块在第一次导入时，会生成 .pyc 文件，当第二次导入时，就会直接加载 .pyc 文件，而不会再次执行模块代码。因此，我们只需把相关的函数和数据定义在一个模块中，就可以获得一个单例对象了。如果我们真的想要一个单例类，可以考虑这样做：
```python
mysingleton.py
class Singleton(object):
    def foo(self):
        pass
singleton = Singleton()
```
将上面的代码保存在文件 mysingleton.py 中，要使用时，直接在其他文件中导入此文件中的对象，这个对象即是单例模式的对象
```python
from a import singleton
```
## 使用装饰器
### 方法一：
```python
def Singleton(cls):
    _instance = {}

    def _singleton(*args, **kargs):
        if cls not in _instance:
            _instance[cls] = cls(*args, **kargs)
        return _instance[cls]

    return _singleton


@Singleton
class A:
    a = 1

    def __init__(self, x=0):
        self.x = x

a1 = A(2)
a2 = A(3)
```
### 方法二：
该方法解决了多线程的问题。
```python
# coding:utf-8
import threading
def synchronized(func):

    func.__lock__ = threading.Lock()

    def lock_func(*args, **kwargs):
        with func.__lock__:
            return func(*args, **kwargs)
    return lock_func


class Singleton(object):
    """
    单例模式
    """
    instance = None

    @synchronized
    def __new__(cls, *args, **kwargs):
        if cls.instance is None:
            cls.instance = object.__new__(cls, *args, **kwargs)
        return cls.instance
```

## 使用类
```python
class Singleton(object):

    def __init__(self):
        pass

    @classmethod
    def instance(cls, *args, **kwargs):
        if not hasattr(Singleton, "_instance"):
            Singleton._instance = Singleton(*args, **kwargs)
        return Singleton._instance
```

<font color=red>但是这样当使用多线程时会存在问题!</font>
## 多线程使用问题
```python
class Singleton(object):

    def __init__(self):
        pass

    @classmethod
    def instance(cls, *args, **kwargs):
        if not hasattr(Singleton, "_instance"):
            Singleton._instance = Singleton(*args, **kwargs)
        return Singleton._instance

import threading

def task(arg):
    obj = Singleton.instance()
    print(obj)

for i in range(10):
    t = threading.Thread(target=task,args=[i,])
    t.start()
```
程序执行后，打印结果如下：
```shell
<__main__.Singleton object at 0x02C933D0>
<__main__.Singleton object at 0x02C933D0>
<__main__.Singleton object at 0x02C933D0>
<__main__.Singleton object at 0x02C933D0>
<__main__.Singleton object at 0x02C933D0>
<__main__.Singleton object at 0x02C933D0>
<__main__.Singleton object at 0x02C933D0>
<__main__.Singleton object at 0x02C933D0>
<__main__.Singleton object at 0x02C933D0>
<__main__.Singleton object at 0x02C933D0>
```

看起来也没有问题，那是因为执行速度过快，如果在init方法中有一些IO操作，就会发现问题了，下面我们通过time.sleep模拟

我们在上面__init__方法中加入以下代码：
```python
def __init__(self):
        import time
        time.sleep(1)
```
重新执行程序后，结果如下
```shell
<__main__.Singleton object at 0x034A3410>
<__main__.Singleton object at 0x034BB990>
<__main__.Singleton object at 0x034BB910>
<__main__.Singleton object at 0x034ADED0>
<__main__.Singleton object at 0x034E6BD0>
<__main__.Singleton object at 0x034E6C10>
<__main__.Singleton object at 0x034E6B90>
<__main__.Singleton object at 0x034BBA30>
<__main__.Singleton object at 0x034F6B90>
<__main__.Singleton object at 0x034E6A90>
```
<font color=red>问题出现了！按照以上方式创建的单例，无法支持多线程</font>

解决办法：加锁！未加锁部分并发执行,加锁部分串行执行,速度降低,但是保证了数据安全

## 多线程加锁
```python
import time
import threading
class Singleton(object):
    _instance_lock = threading.Lock()

    def __init__(self):
        time.sleep(1)

    @classmethod
    def instance(cls, *args, **kwargs):
        with Singleton._instance_lock:
            if not hasattr(Singleton, "_instance"):
                Singleton._instance = Singleton(*args, **kwargs)
        return Singleton._instance


def task(arg):
    obj = Singleton.instance()
    print(obj)
for i in range(10):
    t = threading.Thread(target=task,args=[i,])
    t.start()
time.sleep(20)
obj = Singleton.instance()
print(obj)
```
打印结果如下：

```shell
<__main__.Singleton object at 0x02D6B110>
<__main__.Singleton object at 0x02D6B110>
<__main__.Singleton object at 0x02D6B110>
<__main__.Singleton object at 0x02D6B110>
<__main__.Singleton object at 0x02D6B110>
<__main__.Singleton object at 0x02D6B110>
<__main__.Singleton object at 0x02D6B110>
<__main__.Singleton object at 0x02D6B110>
<__main__.Singleton object at 0x02D6B110>
<__main__.Singleton object at 0x02D6B110>
```
这样就差不多了，但是还是有一点小问题，就是当程序执行时，执行了time.sleep(20)后，下面实例化对象时，此时已经是单例模式了，但我们还是加了锁，这样不太好，再进行一些优化，把intance方法，改成下面的这样就行：
```python
@classmethod
def instance(cls, *args, **kwargs):
    if not hasattr(Singleton, "_instance"):
        with Singleton._instance_lock:
            if not hasattr(Singleton, "_instance"):
                Singleton._instance = Singleton(*args, **kwargs)
    return Singleton._instance
```
## 支持多线程的单例模式
```python
import time
import threading
class Singleton(object):
    _instance_lock = threading.Lock()

    def __init__(self):
        time.sleep(1)

    @classmethod
    def instance(cls, *args, **kwargs):
        if not hasattr(Singleton, "_instance"):
            with Singleton._instance_lock:
                if not hasattr(Singleton, "_instance"):
                    Singleton._instance = Singleton(*args, **kwargs)
        return Singleton._instance


def task(arg):
    obj = Singleton.instance()
    print(obj)
for i in range(10):
    t = threading.Thread(target=task,args=[i,])
    t.start()
time.sleep(20)
obj = Singleton.instance()
print(obj)
```
<font color=red>这种方式实现的单例模式，使用时会有限制，以后实例化必须通过 obj = Singleton.instance() ;如果用 obj=Singleton() ,这种方式得到的不是单例</font>

## 基于__new__ 创建
通过上面例子，我们可以知道，当我们实现单例时，为了保证线程安全需要在内部加入锁

我们知道，当我们实例化一个对象时，是先执行了类的__new__方法（我们没写时，默认调用object.__new__），实例化对象；然后再执行类的__init__方法，对这个对象进行初始化，所有我们可以基于这个，实现单例模式
```python
import threading
class Singleton(object):
    _instance_lock = threading.Lock()

    def __init__(self):
        pass


    def __new__(cls, *args, **kwargs):
        if not hasattr(Singleton, "_instance"):
            with Singleton._instance_lock:
                if not hasattr(Singleton, "_instance"):
                    Singleton._instance = object.__new__(cls)  
        return Singleton._instance

obj1 = Singleton()
obj2 = Singleton()
print(obj1,obj2)

def task(arg):
    obj = Singleton()
    print(obj)

for i in range(10):
    t = threading.Thread(target=task,args=[i,])
    t.start()
```
打印结果如下：

```shelll
<__main__.Singleton object at 0x038B33D0> <__main__.Singleton object at 0x038B33D0>
<__main__.Singleton object at 0x038B33D0>
<__main__.Singleton object at 0x038B33D0>
<__main__.Singleton object at 0x038B33D0>
<__main__.Singleton object at 0x038B33D0>
<__main__.Singleton object at 0x038B33D0>
<__main__.Singleton object at 0x038B33D0>
<__main__.Singleton object at 0x038B33D0>
<__main__.Singleton object at 0x038B33D0>
<__main__.Singleton object at 0x038B33D0>
<__main__.Singleton object at 0x038B33D0>
``` 
采用这种方式的单例模式，以后实例化对象时，和平时实例化对象的方法一样 obj = Singleton() 

## 基于metaclass方式实现
> 1.类由type创建，创建类时，type的__init__方法自动执行，类() 执行type的 __call__方法(类的__new__方法,类的__init__方法)
> 2.对象由类创建，创建对象时，类的__init__方法自动执行，对象()执行类的 __call__ 方法
例子：
```python
class Foo:
    def __init__(self):
        pass

    def __call__(self, *args, **kwargs):
        pass

obj = Foo()
# 执行type的 __call__ 方法，调用 Foo类（是type的对象）的 __new__方法，用于创建对象，然后调用 Foo类（是type的对象）的 __init__方法，用于对对象初始化。

obj() # 执行Foo的 __call__ 方法    
```

### 元类的使用
```python
class SingletonType(type):
    def __init__(self,*args,**kwargs):
        super(SingletonType,self).__init__(*args,**kwargs)

    def __call__(cls, *args, **kwargs): # 这里的cls，即Foo类
        print('cls',cls)
        obj = cls.__new__(cls,*args, **kwargs)
        cls.__init__(obj,*args, **kwargs) # Foo.__init__(obj)
        return obj

class Foo(metaclass=SingletonType): # 指定创建Foo的type为SingletonType
    def __init__(self，name):
        self.name = name
    def __new__(cls, *args, **kwargs):
        return object.__new__(cls)

obj = Foo('xx')
```

### 实现单例模式
```python
import threading

class SingletonType(type):
    _instance_lock = threading.Lock()
    def __call__(cls, *args, **kwargs):
        if not hasattr(cls, "_instance"):
            with SingletonType._instance_lock:
                if not hasattr(cls, "_instance"):
                    cls._instance = super(SingletonType,cls).__call__(*args, **kwargs)
        return cls._instance

class Foo(metaclass=SingletonType):
    def __init__(self,name):
        self.name = name


obj1 = Foo('name')
obj2 = Foo('name')
print(obj1,obj2)
```
