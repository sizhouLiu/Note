# Locust学习

-------

 最近修复一个内存泄漏的bug ，低并发情况下很难复现 因此，在运维老哥的建议下，我学习了Locust 进行压力测试，以达到复现内存泄漏的问题

## 一、前言

当我们做Web系统性能测试方案时，压力模拟工具的选择通常是一个绕不开的环节。对于大部分互联网公司的业务规模和测试资源投入，JMeter这个老牌开源性能测试工具能够满足大部分测试需求，它也可能是世面上书籍、博客教程丰富程度仅次于LoadRunner的性能测试工具。然而当我们的场景需要模拟的并发用户数以千为单位时，使用JMeter的成本越来越大，甚至超出我们掌握的资源。此时，我们开始寻找更低成本的方案，而Locust，为这样的方案带来了一种可能。

Locust是开源、使用Python开发、基于事件、支持分布式并且提供Web Ul进行测试执行和结果展示的性能测试工具。而它之所以能够在资源占用方面明显优于JMeter，一个关键点在于两者模拟虚拟用户的方式不同，JMeter通过线程来作为虚拟用户，而Locust借助gevent库对协程的支持，以greenlet来实现对用户的模拟，相同配置下Locust能支持的并发用户数相比JMeter可以达到一个数量级的提升。Locust使用Python代码定义测试场景，它自带-个Web Ui,用于定义用户模型，发起测试，实时测试数据，错误统计等，在最新未正式发布的v0.8a2(当前最新发布版本v0.8a1)，还提供QPS、评价响应时间等几个简单的图表。



## 二、Locust安装

1. [安装 Python](https://docs.python-guide.org/starting/installation/)（如果还没有）
2. 安装 Locust

```
$ pip3 install locust
```

1. 验证安装

```
$ locust -V
locust 2.31.8 from /usr/local/lib/python3.12/site-packages/locust (Python 3.12.5)
```

## 三、Locust测试

Locust 测试本质上只是一个 Python 程序

```python
import time
from locust import HttpUser, task, between

class QuickstartUser(HttpUser):
    wait_time = between(1, 5)

    @task
    def hello_world(self):
        self.client.get("/hello")
        self.client.get("/world")

    @task(3)
    def view_items(self):
        for item_id in range(10):
            self.client.get(f"/item?id={item_id}", name="/item")
            time.sleep(1)

    def on_start(self):
        self.client.post("/login", json={"username":"foo", "password":"bar"})
```

