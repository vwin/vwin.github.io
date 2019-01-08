---
title: Python 解析一行中的多个json对象
toc: true
date: 2018-11-15 01:05:01
tags: [python,json]
categories: [Python]
description:
---

最近遇到一个需要解析一行中有多个json对象的问题。
比如：{"data1": {"data1_inside": "bla{bl\"a"}}{"data1": {"data1_inside": "blabla["}}{"data1": {"data1_inside": "bla{bla"}}{"data1": {"data1_inside": "bla["}}

<!--more-->
## 方法一：re正则
```python
import re

s = r'{"data1": {"data1_inside": "bla{bl\"a"}}{"data1": {"data1_inside": "blabla["}}{"data1": {"data1_inside": "bla{bla"}}{"data1": {"data1_inside": "bla["}}'

r = re.split('(\{.*?\})(?= *\{)', s)

['', '{"data1": {"data1_inside": "bla{bl\\"a"}}', '', '{"data1": {"data1_inside": "blabla["}}', '', '{"data1": {"data1_inside": "bla{bla"}}', '{"data1": {"data1_inside": "bla["}}']
```

上述方法在遇到如果包含}{的时候会失败
用下面的方法对每个子串进行校验
r就是上面的结果
```python
accumulator = ''
res = []
for subs in r:
    accumulator += subs
    try:
        res.append(json.loads(accumulator))
        accumulator = ''
    except:
        pass
```

## 方法二：json raw_decoder
```python
dec = json.JSONDecoder()
json_str = '{"data": "Foo"}{"data": "BarBaz"}{"data": "Qux"}'
dec.raw_decode(json_str)
({u'data': u'Foo'}, 15)
dec.raw_decode(json_str[15:])
({u'data': u'BarBaz'}, 18)
dec.raw_decode(json_str[33:])
({u'data': u'Qux'}, 15)
```

可由上面输出得到，raw_decode会输出一个子串的position，所以使用循环搞定
```python
dec = json.JSONDecoder()
pos = 0
while not pos == len(str(json_str)):
    j, json_len = dec.raw_decode(str(json_str)[pos:])
    pos += json_len
    # Do something with the json j here
```
