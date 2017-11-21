## 协程

协程，又称微线程，纤程。英文名Coroutine。

### 协程是啥

首先我们得知道协程是啥？协程其实可以认为是比线程更小的执行单元。 为啥说他是一个执行单元，因为他自带CPU上下文。这样只要在合适的时机， 我们可以把一个协程 切换到另一个协程。 只要这个过程中保存或恢复 CPU上下文那么程序还是可以运行的。

通俗的理解：在一个线程中的某个函数，可以在任何地方保存当前函数的一些临时变量等信息，然后切换到另外一个函数中执行，注意不是通过调用函数的方式做到的，并且切换的次数以及什么时候再切换到原来的函数都由开发者自己确定

### 协程和线程差异

那么这个过程看起来比线程差不多。其实不然, 线程切换从系统层面远不止保存和恢复 CPU上下文这么简单。 操作系统为了程序运行的高效性每个线程都有自己缓存Cache等等数据，操作系统还会帮你做这些数据的恢复操作。 所以线程的切换非常耗性能。但是协程的切换只是单纯的操作CPU的上下文，所以一秒钟切换个上百万次系统都抗的住。

### 协程的问题

但是协程有一个问题，就是系统并不感知，所以操作系统不会帮你做切换。 那么谁来帮你做切换？让需要执行的协程更多的获得CPU时间才是问题的关键。

### 例子

目前的协程框架一般都是设计成 1:N 模式。所谓 1:N 就是一个线程作为一个容器里面放置多个协程。 那么谁来适时的切换这些协程？答案是有协程自己主动让出CPU，也就是每个协程池里面有一个调度器， 这个调度器是被动调度的。意思就是他不会主动调度。而且当一个协程发现自己执行不下去了(比如异步等待网络的数据回来，但是当前还没有数据到)， 这个时候就可以由这个协程通知调度器，这个时候执行到调度器的代码，调度器根据事先设计好的调度算法找到当前最需要CPU的协程。 切换这个协程的CPU上下文把CPU的运行权交个这个协程，直到这个协程出现执行不下去需要等等的情况，或者它调用主动让出CPU的API之类，触发下一次调度。

### 那么这个实现有没有问题？

其实是有问题的，假设这个线程中有一个协程是CPU密集型的他没有IO操作， 也就是自己不会主动触发调度器调度的过程，那么就会出现其他协程得不到执行的情况， 所以这种情况下需要程序员自己避免。这是一个问题，假设业务开发的人员并不懂这个原理的话就可能会出现问题。

### 协程的好处

在IO密集型的程序中由于IO操作远远慢于CPU的操作，所以往往需要CPU去等IO操作。 同步IO下系统需要切换线程，让操作系统可以在IO过程中执行其他的东西。 这样虽然代码是符合人类的思维习惯但是由于大量的线程切换带来了大量的性能的浪费，尤其是IO密集型的程序。

所以人们发明了异步IO。就是当数据到达的时候触发我的回调。来减少线程切换带来性能损失。 但是这样的坏处也是很大的，主要的坏处就是操作被 “分片” 了，代码写的不是 “一气呵成” 这种。 而是每次来段数据就要判断 数据够不够处理哇，够处理就处理吧，不够处理就在等等吧。这样代码的可读性很低，其实也不符合人类的习惯。

但是协程可以很好解决这个问题。比如 把一个IO操作 写成一个协程。当触发IO操作的时候就自动让出CPU给其他协程。要知道协程的切换很轻的。 协程通过这种对异步IO的封装 既保留了性能也保证了代码的容易编写和可读性。在高IO密集型的程序下很好。但是高CPU密集型的程序下没啥好处。

### 协程一个简单实现
```py
import time

def A():
    while True:
        print("----A---")
        yield
        time.sleep(0.5)

def B(c):
    while True:
        print("----B---")
        c.next()
        time.sleep(0.5)

if __name__=='__main__':
    a = A()
    B(a)
```
运行结果：

```
--B--
--A--
--B--
--A--
--B--
--A--
--B--
--A--
--B--
--A--
--B--
--A--
...省略...
```


## 协程-greenlet版

为了更好使用协程来完成多任务，python中的greenlet模块对其封装，从而使得切换任务变的更加简单

#### 安装方式

使用如下命令安装greenlet模块:
```
sudo pip install greenlet
```

```py
#coding=utf-8

from greenlet import greenlet
import time

def test1():
    while True:
        print "---A--"
        gr2.switch()
        time.sleep(0.5)

def test2():
    while True:
        print "---B--"
        gr1.switch()
        time.sleep(0.5)

gr1 = greenlet(test1)
gr2 = greenlet(test2)

#切换到gr1中运行
gr1.switch()
```
运行效果
```
---A--
---B--
---A--
---B--
---A--
---B--
---A--
---B--
...省略...
```


## 协程-gevent版

greenlet已经实现了协程，但是这个还的人工切换，是不是觉得太麻烦了，不要捉急，python还有一个比greenlet更强大的并且能够自动切换任务的模块gevent

其原理是当一个greenlet遇到IO(指的是input output 输入输出，比如网络、文件操作等)操作时，比如访问网络，就自动切换到其他的greenlet，等到IO操作完成，再在适当的时候切换回来继续执行。

由于IO操作非常耗时，经常使程序处于等待状态，有了gevent为我们自动切换协程，就保证总有greenlet在运行，而不是等待IO

#### 1.gevent的使用

```py
#coding=utf-8

#请使用python 2 来执行此程序

import gevent

def f(n):
    for i in range(n):
        print gevent.getcurrent(), i

g1 = gevent.spawn(f, 5)
g2 = gevent.spawn(f, 5)
g3 = gevent.spawn(f, 5)
g1.join()
g2.join()
g3.join()
```
运行结果
```
<Greenlet at 0x10e49f550: f(5)> 0
<Greenlet at 0x10e49f550: f(5)> 1
<Greenlet at 0x10e49f550: f(5)> 2
<Greenlet at 0x10e49f550: f(5)> 3
<Greenlet at 0x10e49f550: f(5)> 4
<Greenlet at 0x10e49f910: f(5)> 0
<Greenlet at 0x10e49f910: f(5)> 1
<Greenlet at 0x10e49f910: f(5)> 2
<Greenlet at 0x10e49f910: f(5)> 3
<Greenlet at 0x10e49f910: f(5)> 4
<Greenlet at 0x10e49f4b0: f(5)> 0
<Greenlet at 0x10e49f4b0: f(5)> 1
<Greenlet at 0x10e49f4b0: f(5)> 2
<Greenlet at 0x10e49f4b0: f(5)> 3
<Greenlet at 0x10e49f4b0: f(5)> 4
```

可以看到，3个greenlet是依次运行而不是交替运行

#### 2.gevent切换执行

```py
import gevent

def f(n):
    for i in range(n):
        print gevent.getcurrent(), i
        #用来模拟一个耗时操作，注意不是time模块中的sleep
        gevent.sleep(1)

g1 = gevent.spawn(f, 5)
g2 = gevent.spawn(f, 5)
g3 = gevent.spawn(f, 5)
g1.join()
g2.join()
g3.join()
```

运行结果
```
<Greenlet at 0x7fa70ffa1c30: f(5)> 0
<Greenlet at 0x7fa70ffa1870: f(5)> 0
<Greenlet at 0x7fa70ffa1eb0: f(5)> 0
<Greenlet at 0x7fa70ffa1c30: f(5)> 1
<Greenlet at 0x7fa70ffa1870: f(5)> 1
<Greenlet at 0x7fa70ffa1eb0: f(5)> 1
<Greenlet at 0x7fa70ffa1c30: f(5)> 2
<Greenlet at 0x7fa70ffa1870: f(5)> 2
<Greenlet at 0x7fa70ffa1eb0: f(5)> 2
<Greenlet at 0x7fa70ffa1c30: f(5)> 3
<Greenlet at 0x7fa70ffa1870: f(5)> 3
<Greenlet at 0x7fa70ffa1eb0: f(5)> 3
<Greenlet at 0x7fa70ffa1c30: f(5)> 4
<Greenlet at 0x7fa70ffa1870: f(5)> 4
<Greenlet at 0x7fa70ffa1eb0: f(5)> 4
```
3个greenlet交替运行

#### 3.gevent并发下载器

当然，实际代码里，我们不会用gevent.sleep()去切换协程，而是在执行到IO操作时，gevent自动切换，代码如下
```py
#coding=utf-8

from gevent import monkey; 
import gevent
import urllib2

#有IO才做时需要这一句
monkey.patch_all()

def myDownLoad(url):
    print('GET: %s' % url)
    resp = urllib2.urlopen(url)
    data = resp.read()
    print('%d bytes received from %s.' % (len(data), url))

gevent.joinall([
        gevent.spawn(myDownLoad, 'http://www.baidu.com/'),
        gevent.spawn(myDownLoad, 'http://www.itcast.cn/'),
        gevent.spawn(myDownLoad, 'http://www.itheima.com/'),
])
```

运行结果
```
GET: http://www.baidu.com/
GET: http://www.itcast.cn/
GET: http://www.itheima.com/
102247 bytes received from http://www.baidu.com/.
166903 bytes received from http://www.itheima.com/.
162294 bytes received from http://www.itcast.cn/.
```

从上能够看到是先发送的获取baidu的相关信息，然后依次是itcast、itheima，但是收到数据的先后顺序不一定与发送顺序相同，这也就体现出了异步，即不确定什么时候会收到数据，顺序不一定


## 单进程服务器-gevent版
```py
#gevent版-TCP服务器

import sys
import time
import gevent

from gevent import socket,monkey
monkey.patch_all()

def handle_request(conn):
    while True:
        data = conn.recv(1024)
        if not data:
            conn.close()
            break
        print("recv:", data)
        conn.send(data)


def server(port):
    s = socket.socket()
    s.bind(('', port))
    s.listen(5)
    while True:
        cli, addr = s.accept()
        gevent.spawn(handle_request, cli)

if __name__ == '__main__':
    server(7788)

```