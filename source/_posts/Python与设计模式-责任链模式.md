---
title: Python与设计模式 - 责任链模式
toc: true
date: 2018-12-24 04:57:14
tags: [设计模式,责任链模式]
categories: [设计模式]
description:
---
责任链模式是一种对象的行为模式。在责任链模式里，很多对象由每一个对象对其下家的引用而连接起来形成一条链。请求在这个链上传递，直到链上的某一个对象决定处理此请求。发出这个请求的客户端并不知道链上的哪一个对象最终处理这个请求，这使得系统可以在不影响客户端的情况下动态地重新组织和分配责任。

<!--more-->
举个例子说明责任链模式在Python中的使用
# 场景一
比如我们还在读书的时候，考试的分数都是几个档次，比如90-100分，80-90分，好吧我想做一个根据分数打印你的学习成绩的反馈， 比如90-100就是A+，80-90就是A，70-80就是B+… 当然你可以用很多种方法实现，我这里就来实现一个Chain模式:用一系列的类来响应， 但只有遇到适合处理它的类才会处理，类似与case和switch的作用
## Python实现
```python
class BaseHandler:
    # 它起到了链的作用
    def successor(self, successor):
        self.successor = successor

class ScoreHandler1(BaseHandler):
    def handle(self, request):
        if request > 90 and request <= 100:
            return "A+"
        else:
            # 否则传给下一个链，下同，但是我是要return回结果的
            return self.successor.handle(request)

class ScoreHandler2(BaseHandler):
    def handle(self, request):
        if request > 80 and request <= 90:
            return "A"
        else:
            return self.successor.handle(request)

class ScoreHandler3(BaseHandler):
    def handle(self, request):
        if request > 70 and request <= 80:
            return "B+"
        else:
            return "unsatisfactory result"

class Client:
    def __init__(self):
        h1 = ScoreHandler1()
        h2 = ScoreHandler2()
        h3 = ScoreHandler3()
        # 注意这个顺序，h3包含一个类似于default结果的东西，是要放在最后的，其他的顺序是无所谓的，比如h1和h2
        h1.successor(h2)
        h2.successor(h3)

        requests =  {'zhangsan': 78,
                    'lisi': 98,
                    'wangwu': 82,
                    'zhaoliu': 60}
        for name, score in requests.iteritems():
            print '{} is {}'.format(name, h1.handle(score))

if __name__== "__main__":
    client = Client()
```

# 场景二
假设有这么一个请假系统：员工若想要请3天以内（包括3天的假），只需要直属经理批准就可以了；如果想请3-7天，不仅需要直属经理批准，部门经理需要最终批准；如果请假大于7天，不光要前两个经理批准，也需要总经理最终批准。类似的系统相信大家都遇到过，那么该如何实现呢？首先想到的当然是if…else…，但一旦遇到需求变动，其臃肿的代码和复杂的耦合缺点都显现出来。简单分析下需求，“假条”在三个经理间是单向传递关系，像一条链条一样，因而，我们可以用一条“链”把他们进行有序连接。

## Python实现
```python
class manager():
    successor = None
    name = ''
    def __init__(self, name):
        self.name = name
    def setSuccessor(self, successor):
        self.successor = successor
    def handleRequest(self, request):
        pass
class lineManager(manager):
    def handleRequest(self, request):
        if request.requestType == 'DaysOff' and request.number <= 3:
            print '%s:%s Num:%d Accepted OVER' % (self.name, request.requestContent, request.number)
        else:
            print '%s:%s Num:%d Accepted CONTINUE' % (self.name, request.requestContent, request.number)
            if self.successor != None:
                self.successor.handleRequest(request)
class departmentManager(manager):
    def handleRequest(self, request):
        if request.requestType == 'DaysOff' and request.number <= 7:
            print '%s:%s Num:%d Accepted OVER' % (self.name, request.requestContent, request.number)
        else:
            print '%s:%s Num:%d Accepted CONTINUE' % (self.name, request.requestContent, request.number)
            if self.successor != None:
                self.successor.handleRequest(request)
class generalManager(manager):
    def handleRequest(self, request):
        if request.requestType == 'DaysOff':
            print '%s:%s Num:%d Accepted OVER' % (self.name, request.requestContent, request.number)
class request():
    requestType = ''
    requestContent = ''
    number = 0

#request类封装了假期请求。在具体的经理类中，可以通过setSuccessor接口来构建“责任链”，并在handleRequest接口中实现逻辑。场景类中实现如下：

if  __name__=="__main__":
    line_manager = lineManager('LINE MANAGER')
    department_manager = departmentManager('DEPARTMENT MANAGER')
    general_manager = generalManager('GENERAL MANAGER')

    line_manager.setSuccessor(department_manager)
    department_manager.setSuccessor(general_manager)

    req = request()
    req.requestType = 'DaysOff'
    req.requestContent = 'Ask 1 day off'
    req.number = 1
    line_manager.handleRequest(req)

    req.requestType = 'DaysOff'
    req.requestContent = 'Ask 5 days off'
    req.number = 5
    line_manager.handleRequest(req)

    req.requestType = 'DaysOff'
    req.requestContent = 'Ask 10 days off'
    req.number = 10
    line_manager.handleRequest(req)
```

# 责任链模式
责任链模式的定义如下：使多个对象都有机会处理请求，从而避免了请求的发送者和接收者之间的耦合关系。将这些对象连成一条链，并沿着这条链传递该请求，直到有对象处理它为止。

![](https://ws4.sinaimg.cn/large/006tNbRwly1fyi0dpyqabj30bs06bgm3.jpg)

需要说明的是，责任链模式中的应该只有一个处理者，也就是说，本例中的“最终批准”为该对象所谓的“请求处理”。

# 责任链模式的优缺点和应用场景

## 优点
- 将请求者与处理者分离，请求者并不知道请求是被哪个处理者所处理，易于扩展。

## 应用场景
- 若一个请求可能由一个对请求有链式优先级的处理群所处理时，可以考虑责任链模式。除本例外，银行的客户请求处理系统也可以用责任链模式实现（VIP客户和普通用户处理方式当然会有不同）。

## 缺点
- 如果责任链比较长，会有比较大的性能问题；
- 如果责任链比较长，若业务出现问题，比较难定位是哪个处理者的问题。

参考：https://www.cnblogs.com/java-my-life/archive/2012/05/28/2516865.html