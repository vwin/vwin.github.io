---
title: Python 时间日期处理总结
toc: true
date: 2018-09-12 18:22:57
tags: [Python,datetime,time,date,时间,日期]
categories: [Python]
description:
---
python中有date,time,datetime等关于时间，日期的模块，经常需要在各种形式中进行转换，进行以下总结

<!--more-->

## 常用模块
### datetime
```python
>>> import datetime
>>> now = datetime.datetime.now()
>>> now
datetime.datetime(2018, 9, 12, 18, 25, 53, 374738)
>>> type(now)
<class 'datetime.datetime'>
```
### timestamp
```python
>>> import time
>>> time.time()
1536747994.9385228
>>> int(time.time())
1536748012
```
### time tuple
```python
>>> import time
>>> time.localtime()
time.struct_time(tm_year=2018, tm_mon=9, tm_mday=12, tm_hour=18, tm_min=27, tm_sec=22, tm_wday=2, tm_yday=255, tm_isdst=0)
```
### string
获取格式化的日期和时间
```python
>>> import datetime
>>> datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
'2018-09-12 18:27:53'
>>> datetime.datetime.now().strftime("%Y-%m-%d")
'2018-09-12'
```
### date
```python
>>> import datetime
>>> datetime.datetime.now().date()
datetime.date(2018, 9, 12)
```

## datetime基本操作
### 获取当前datetime
```python
>>> import datetime
>>> datetime.datetime.now()
datetime.datetime(2018, 9, 12, 18, 29, 42, 735565)
```

### 获取当天date
```python
>>> datetime.date.today()
datetime.date(2018, 9, 12)
```

### 获取明天/前N天
```python
明天：
>>> datetime.date.today() + datetime.timedelta(days=1)
datetime.date(2018, 9, 13)

三天前：
>>> datetime.datetime.now() - datetime.timedelta(days=3)
datetime.datetime(2018, 9, 9, 18, 32, 7, 652425)
```

### 获取当天开始和结束时间(00:00:00 23:59:59)
```python
>>> datetime.datetime.combine(datetime.date.today(), datetime.time.min)
datetime.datetime(2018, 9, 12, 0, 0)
>>> datetime.datetime.combine(datetime.date.today(), datetime.time.max)
datetime.datetime(2018, 9, 12, 23, 59, 59, 999999)
```

### 获取本周/本月/上月最后一天
```python
本周：
>>> today = datetime.date.today()
>>> today
datetime.date(2018, 9, 12)
>>> sunday = today + datetime.timedelta(6 - today.weekday())
>>> sunday
datetime.date(2018, 9, 16)

本月：
>>> import calendar
>>> today = datetime.date.today()
>>> _, last_day_num = calendar.monthrange(today.year, today.month)
>>> last_day = datetime.date(today.year, today.month, last_day_num)
>>> last_day
datetime.date(2018, 9, 30)

上个月最后一天：
>>> today = datetime.date.today()
>>> first = datetime.date(day=1, month=today.month, year=today.year)
>>> lastMonth = first - datetime.timedelta(days=1)
>>> lastMonth
datetime.date(2018, 8, 31)
```


## 关系转换
Datetime Object / String / timestamp / time tuple

### datetime & string

```python
datetime -> string
>>> datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
'2018-09-12 18:36:37'

string -> datetime
>> datetime.datetime.strptime("2019-12-31 18:20:10", "%Y-%m-%d %H:%M:%S")
datetime.datetime(2019, 12, 31, 18, 20, 10)
```

### datetime & timetuple

```python
datetime -> timetuple
>>> datetime.datetime.now().timetuple()
time.struct_time(tm_year=2018, tm_mon=9, tm_mday=12, tm_hour=18, tm_min=37, tm_sec=35, tm_wday=2, tm_yday=255, tm_isdst=-1)

timetuple -> datetime
timetuple => timestamp => datetime [看后面datetime<=>timestamp]
```

### datetime & date
```python
datetime -> date
>>> datetime.datetime.now().date()
datetime.date(2018, 9, 12)

date -> datetime
>>> today = datetime.date.today()
>>> today
datetime.date(2018, 9, 12)
>>> datetime.datetime.combine(today, datetime.time())
datetime.datetime(2018, 9, 12, 0, 0)
>>> datetime.datetime.combine(today, datetime.time.min)
datetime.datetime(2018, 9, 12, 0, 0)
>>> datetime.datetime.combine(today, datetime.time.max)
datetime.datetime(2018, 9, 12, 23, 59, 59, 999999)
```

### datetime & timestamp
```python
datetime -> timestamp
>>> now = datetime.datetime.now()
>>> timestamp = time.mktime(now.timetuple())
>>> timestamp
1536748809.0

timestamp -> datetime
>>> datetime.datetime.fromtimestamp(1536748809.0)
datetime.datetime(2018, 9, 12, 18, 40, 9)
```
