---
title: Locust基础-1
toc: true
date: 2018-08-08 18:39:30
tags: [locust,性能测试]
categories: [性能测试]
description:
---

# 介绍
Locust 官方网站：https://www.locust.io/
An open source load testing tool.
一个开源性能测试工具。
define user behaviour with python code, and swarm your system with millions of simultaneous users.
使用 Python 代码来定义用户行为。用它可以模拟百万计的并发用户访问你的系统。
![](https://ws4.sinaimg.cn/large/006tNc79ly1fyt89j9tb3j31bd0u0tf7.jpg)
<!--more-->
1. LoadRunner 是非常有名的商业性能测试工具，功能非常强大。使用也比较复杂，目前大多介绍性能测试的书籍都以该工具为基础，甚至有些书整本都在介绍 LoadRunner 的使用。

2. Jmeter 同样是非常有名的开源性能测试工具，功能也很完善，在本书中介绍了它作为接口测试工具的使用。但实际上，它是一个标准的性能测试工具。关于Jmeter相关的资料也非常丰富，它的官方文档也很完善。

3. Locust 同样是性能测试工具，虽然官方这样来描述它 “An open source load testing tool.” 。但其它和前面两个工具有着较大的不同。相比前面两个工具，功能上要差上不少，但它也并非优点全无。
   - Locust 完全基本 Python 编程语言，采用 Pure Python 描述测试脚本，并且 HTTP 请求完全基于 Requests 库。除了 HTTP/HTTPS 协议，Locust 也可以测试其它协议的系统，只需要采用Python调用对应的库进行请求描述即可。
   - LoadRunner 和 Jmeter 这类采用进程和线程的测试工具，都很难在单机上模拟出较高的并发压力。Locust 的并发机制摒弃了进程和线程，采用协程（gevent）的机制。协程避免了系统级资源调度，由此可以大幅提高单机的并发能力。

正是基于这样的特点，使我选择使用Locust工具来做性能测试，另外一个原因是它可以让我们换一种方式认识性能测试，可能更容易看清性能测试的本质。

# 安装

## pip安装
```shell
pip install locust
```
## Git安装
GitHub项目地址：https://github.com/locustio/locust/

将项目克隆下来，通过Python 执行 setup.py 文件

```python
python setup.py install
```

最后，检查是否安装成功:
```shell
locust --help
```

## 依赖分析
- gevent 是在 Python 中实现协程的一个第三方库。协程，又称微线程（Coroutine）。使用gevent可以获得极高的并发性能。
- flask 是 Python 的一个 Web 开发框架。
- Requests 用来做 HTTP 接口测试。
- msgpack-python 是一种快速、紧凑的二进制序列化格式，适用于类似JSON的数据。
- six 提供了一些简单的工具用来封装 Python2 和 Python3 之间的差异性。
- pyzmq 如果你打算运行 Locust 分布在多个进程/机器，建议你安装pyzmq。

当我们在安装 Locust 时，它会检测我们当前的 Python 环境是否已经安装了这些库，如果没有安装，它会先把这些库一一装上。并且对这些库版本有要求，有些是必须等于某版本，有些是大于某版本。我们也可以事先把这些库全部按要求装好，再安装Locust时就会快上许多。

# 创建性能测试
## 写脚本
创建 load_test.py 文件，通过 Python 编写性能测试脚本。
```python
from locust import HttpLocust, TaskSet, task

# 定义用户行为
class UserBehavior(TaskSet):

    @task
    def baidu_index(self):
        self.client.get("/")


class WebsiteUser(HttpLocust):
    task_set = UserBehavior
    min_wait = 3000
    max_wait = 6000
```
- UserBehavior类继承TaskSet类，用于描述用户行为。
- baidu_index() 方法表示一个用户为行，访问百度首页。使用@task装饰该方法为一个事务。client.get()用于指请求的路径“/”，因为是百度首页，所以指定为根路径。
- WebsiteUser类用于设置性能测试。
  - task_set ：指向一个定义的用户行为类。
  - min_wait ：执行事务之间用户等待时间的下界（单位：毫秒）。
  - max_wait ：执行事务之间用户等待时间的上界（单位：毫秒）。

## 启动性能测试
```shell
> locust -f .\load_test.py --host=https://www.baidu.com

[2017-10-16 16:44:40,839] DESKTOP-SMGQBBM/INFO/locust.main: Starting web monitor at *:8089
[2017-10-16 16:44:40,842] DESKTOP-SMGQBBM/INFO/locust.main: Starting Locust 0.8
```
- -f 指定性能测试脚本文件。
- --host 指定被测试应用的URL的地址，注意访问百度使用的HTTPS协议。
- 通过浏览器访问：http://localhost:8089（Locust启动网络监控器，默认为端口号为: 8089）
![](https://ws1.sinaimg.cn/large/006tNc79ly1fyt953kd2lj30zb0mjwgt.jpg)
- Number of users to simulate 设置模拟用户数。
- Hatch rate（users spawned/second） 每秒产生（启动）的虚拟用户数。

点击 “Start swarming” 按钮，开始运行性能测试。
![](https://ws4.sinaimg.cn/large/006tNc79ly1fyt95m2zbaj30z90jlmza.jpg)
性能测试参数
1. Type： 请求的类型，例如GET/POST。
2. Name：请求的路径。这里为百度首页，即：https://www.baidu.com/
3. request：当前请求的数量。
4. fails：当前请求失败的数量。
5. Median：中间值，单位毫秒，一半的服务器响应时间低于该值，而另一半高于该值。
6. Average：平均值，单位毫秒，所有请求的平均响应时间。
7. Min：请求的最小服务器响应时间，单位毫秒。
8. Max：请求的最大服务器响应时间，单位毫秒。
9. Content Size：单个请求的大小，单位字节。
10. reqs/sec：是每秒钟请求的个数。

# no-web模式
以上一节的 baidu 首页测试（load_test.py）为例 通过 no-web 模式运行测试。

```shell
> locust -f load_test.py --host=https://www.baidu.com --no-web -c 10 -r 2 -t 1m

[2017-10-30 22:17:30,292] DESKTOP-SMGQBBM/INFO/locust.main: Run time limit set to 60 seconds
[2017-10-30 22:17:30,302] DESKTOP-SMGQBBM/INFO/locust.main: Starting Locust 0.8
[2017-10-30 22:17:30,302] DESKTOP-SMGQBBM/INFO/locust.runners: Hatching and swarming 10 clients at the rate 2 clients/s...
 Name                                                          # reqs      # fails     Avg     Min     Max  |  Median   req/s

....


 [2017-10-30 22:18:30,301] DESKTOP-SMGQBBM/INFO/locust.main: Time limit reached. Stopping Locust.
 [2017-10-30 22:18:30,302] DESKTOP-SMGQBBM/INFO/locust.main: Shutting down (exit code 0), bye.
  Name                                                          # reqs      # fails     Avg     Min     Max  |  Median   req/s
 --------------------------------------------------------------------------------------------------------------------------------------------
  GET /                                                            117     0(0.00%)      31      17      96  |      28    2.10
 --------------------------------------------------------------------------------------------------------------------------------------------
  Total                                                            117     0(0.00%)                                       2.10

 Percentage of the requests completed within given times
  Name                                                           # reqs    50%    66%    75%    80%    90%    95%    98%    99%   100%
 --------------------------------------------------------------------------------------------------------------------------------------------
  GET /                                                             117     28     30     36     37     49     62     69     72     96
 --------------------------------------------------------------------------------------------------------------------------------------------
  Total                                                             117     28     30     36     37     49     62     69     72     96
```
启动参数：
- --no-web 表示不使用Web界面运行测试。
- -c 设置虚拟用户数。
- -r 设置每秒启动虚拟用户数。
- -t 设置设置运行时间。(在有些locust版本中可能不支持该参数，改用-n参数可以指定发送请求数量，具体的可以通过locust --help查看)

# Locust参数
## 查看
打开命令提示符（或Linux终端），输入 locust --help 。
```shell
> locust --help
Usage: locust [options] [LocustClass [LocustClass2 ... ]]

Options:
  -h, --help            show this help message and exit
  -H HOST, --host=HOST  Host to load test in the following format:
                        http://10.21.32.33
  --web-host=WEB_HOST   Host to bind the web interface to. Defaults to '' (all
                        interfaces)
  -P PORT, --port=PORT, --web-port=PORT
                        Port on which to run web host
  -f LOCUSTFILE, --locustfile=LOCUSTFILE
                        Python module file to import, e.g. '../other.py'.
                        Default: locustfile
  --csv=CSVFILEBASE, --csv-base-name=CSVFILEBASE
                        Store current request stats to files in CSV format.
  --master              Set locust to run in distributed mode with this
                        process as master
  --slave               Set locust to run in distributed mode with this
                        process as slave
  --master-host=MASTER_HOST
                        Host or IP address of locust master for distributed
                        load testing. Only used when running with --slave.
                        Defaults to 127.0.0.1.
  --master-port=MASTER_PORT
                        The port to connect to that is used by the locust
                        master for distributed load testing. Only used when
                        running with --slave. Defaults to 5557. Note that
                        slaves will also connect to the master node on this
                        port + 1.
  --master-bind-host=MASTER_BIND_HOST
                        Interfaces (hostname, ip) that locust master should
                        bind to. Only used when running with --master.
                        Defaults to * (all available interfaces).
  --master-bind-port=MASTER_BIND_PORT
                        Port that locust master should bind to. Only used when
                        running with --master. Defaults to 5557. Note that
                        Locust will also use this port + 1, so by default the
                        master node will bind to 5557 and 5558.
  --expect-slaves=EXPECT_SLAVES
                        How many slaves master should expect to connect before
                        starting the test (only when --no-web used).
  --no-web              Disable the web interface, and instead start running
                        the test immediately. Requires -c and -r to be
                        specified.
  -c NUM_CLIENTS, --clients=NUM_CLIENTS
                        Number of concurrent Locust users. Only used together
                        with --no-web
  -r HATCH_RATE, --hatch-rate=HATCH_RATE
                        The rate per second in which clients are spawned. Only
                        used together with --no-web
  -t RUN_TIME, --run-time=RUN_TIME
                        Stop after the specified amount of time, e.g. (300s,
                        20m, 3h, 1h30m, etc.). Only used together with --no-
                        web
  -L LOGLEVEL, --loglevel=LOGLEVEL
                        Choose between DEBUG/INFO/WARNING/ERROR/CRITICAL.
                        Default is INFO.
  --logfile=LOGFILE     Path to log file. If not set, log will go to
                        stdout/stderr
  --print-stats         Print stats in the console
  --only-summary        Only print the summary stats
  --no-reset-stats      Do not reset statistics once hatching has been
                        completed
  -l, --list            Show list of possible locust classes and exit
  --show-task-ratio     print table of the locust classes' task execution
                        ratio
  --show-task-ratio-json
                        print json data of the locust classes' task execution
                        ratio
  -V, --version         show program's version number and exit

```
## 说明
|参数|说明|
|----|----|
|-h, --help|查看帮助
|-H HOST, --host=HOST|指定被测试的主机，采用以格式：http://10.21.32.33
|--web-host=WEB_HOST|指定运行 Locust Web 页面的主机，默认为空 ''。
|-P PORT, --port=PORT, --web-port=PORT|指定 --web-host 的端口，默认是8089
|-f LOCUSTFILE, --locustfile=LOCUSTFILE|指定运行 Locust 性能测试文件，默认为: locustfile.py
|--csv=CSVFILEBASE, --csv-base-name=CSVFILEBASE|以CSV格式存储当前请求测试数据。
|--master|Locust 分布式模式使用，当前节点为 master 节点。
|--slave|Locust 分布式模式使用，当前节点为 slave 节点。
|--master-host=MASTER_HOST|分布式模式运行，设置 master 节点的主机或 IP 地址，只在与 --slave 节点一起运行时使用，默认为：127.0.0.1.
|--master-port=MASTER_PORT|分布式模式运行， 设置 master 节点的端口号，只在与 --slave 节点一起运行时使用，默认为：5557。注意，slave 节点也将连接到这个端口+1 上的 master 节点。
|--master-bind-host=MASTER_BIND_HOST|Interfaces (hostname, ip) that locust master should bind to. Only used when running with --master. Defaults to * (all available interfaces).
|--master-bind-port=MASTER_BIND_PORT|Port that locust master should bind to. Only used when running with --master. Defaults to 5557. Note that Locust will also use this port + 1, so by default the master node will bind to 5557 and 5558.
|--expect-slaves=EXPECT_SLAVES|How many slaves master should expect to connect before starting the test (only when --no-web used).
|--no-web|no-web 模式运行测试，需要 -c 和 -r 配合使用.
|-c NUM_CLIENTS, --clients=NUM_CLIENTS|指定并发用户数，作用于 --no-web 模式。
|-r HATCH_RATE, --hatch-rate=HATCH_RATE|指定每秒启动的用户数，作用于 --no-web 模式。
|-t RUN_TIME, --run-time=RUN_TIME|设置运行时间, 例如： (300s, 20m, 3h, 1h30m). 作用于 --no-web 模式。
|-L LOGLEVEL, --loglevel=LOGLEVEL|选择 log 级别（DEBUG/INFO/WARNING/ERROR/CRITICAL）. 默认是 INFO.
|--logfile=LOGFILE|日志文件路径。如果没有设置，日志将去 stdout/stderr
|--print-stats|在控制台中打印数据
|--only-summary|只打印摘要统计
|--no-reset-stats|Do not reset statistics once hatching has been completed。
|-l, --list|显示测试类, 配置 -f 参数使用
|--show-task-ratio|打印 locust 测试类的任务执行比例，配合 -f 参数使用.
|--show-task-ratio-json|以 json 格式打印 locust 测试类的任务执行比例，配合 -f 参数使用.
|-V, --version|查看当前 Locust 工具的版本.

