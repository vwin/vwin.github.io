---
title: Python使用lamba应对各种排序
toc: true
date: 2018-08-30 14:04:15
tags: [python,lambda,排序,list,dict]
categories: [Python]
description:
---

使用Python实现各种排序，比如dict的按key和value排序，list嵌套dict排序
考虑到tuple在排序时和list的区别并不大，所以不考虑tuple；
而dict的key采用hash实现，仅仅对dict进行排序并无实际意义，所以仅考虑需要输出等情况下的dict排序实现。
<!--more-->

## 函数
### sorted
sorted函数原型
```python
sorted(iterable[,key][,reverse])
```

sorted()可以接受3个参数，需要排序的变量必不可少，然后是key指定排序的元素，reverse指定是否逆序

### lambda

lambda是匿名函数

一般形式
```python 
lambda arguments:expression
```

函数形式
```python
def <lambda>(arguments):
    return expression
```

## 排序

### 简单list排序
```python
lis = ['a', 'b', 'c']
print(sorted(lis))
# ['a', 'b', 'c']
print(sorted(lis, reverse=True))
# ['c', 'b', 'a']
```

### dict按照key排序
```python
dic = {'c': 1, 'b': 2, 'a': 3}
print(sorted(dic))
# ['a', 'b', 'c']
print(sorted(dic, reverse=True))
# ['c', 'b', 'a']
```

### dict按照value排序
```python
dic = {'c': 1, 'b': 2, 'a': 3}
print(sorted(dic, key=lambda k: dic[k]))
# ['c', 'b', 'a']
print(sorted(dic, key=lambda k: dic[k], reverse=True))
# ['a', 'b', 'c']
```

### list嵌套list排序

```python
lis = [[4, 2, 9], [1, 5, 6], [7, 8, 3]]
print(sorted(lis, key=lambda k: k[0]))
# [[1, 5, 6], [4, 2, 9], [7, 8, 3]]

print(sorted(lis, key=lambda k: k[1]))
# [[4, 2, 9], [1, 5, 6], [7, 8, 3]]

print(sorted(lis, key=lambda k: k[2]))
# [[7, 8, 3], [1, 5, 6], [4, 2, 9]]

print(sorted(lis, key=lambda k: k[0], reverse=True))
# [[7, 8, 3], [4, 2, 9], [1, 5, 6]]
```

### dict嵌套dict排序
```python
lis = [
    {'x': 3, 'y': 2, 'z': 1},
    {'x': 2, 'y': 1, 'z': 3},
    {'x': 1, 'y': 3, 'z': 2},
]
print(sorted(lis, key=lambda k: k['x']))
# [{'z': 2, 'x': 1, 'y': 3}, {'z': 3, 'x': 2, 'y': 1}, {'z': 1, 'x': 3, 'y': 2}]

print(sorted(lis, key=lambda k: k['y']))
# [{'z': 3, 'x': 2, 'y': 1}, {'z': 1, 'x': 3, 'y': 2}, {'z': 2, 'x': 1, 'y': 3}]

print(sorted(lis, key=lambda k: k['z']))
# [{'z': 1, 'x': 3, 'y': 2}, {'z': 2, 'x': 1, 'y': 3}, {'z': 3, 'x': 2, 'y': 1}]

print(sorted(lis, key=lambda k: k['x'], reverse=True))
# [{'z': 1, 'x': 3, 'y': 2}, {'z': 3, 'x': 2, 'y': 1}, {'z': 2, 'x': 1, 'y': 3}]

```

### dict嵌套list排序
```python
dic = {
    'a': [1, 2, 3],
    'b': [2, 1, 3],
    'c': [3, 1, 2],
}
print(sorted(dic, key=lambda k: dic[k][0]))
# ['a', 'b', 'c']

print(sorted(dic, key=lambda k: dic[k][1]))
# ['b', 'c', 'a']

print(sorted(dic, key=lambda k: dic[k][2]))
# ['c', 'b', 'a']

print(sorted(dic, key=lambda k: dic[k][0], reverse=True))
# ['c', 'b', 'a']
```

## 其他嵌套排序
更深层嵌套排序方法和上面介绍的大同小异，实际就是lambda的操作；
需要注意的就是dict的排序只会取其key，所以需要lambda首先将其转换为value才能操作value排序。

