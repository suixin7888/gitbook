## 1.死锁

在线程间共享多个资源的时候，如果两个线程分别占有一部分资源并且同时等待对方的资源，就会造成死锁。

尽管死锁很少发生，但一旦发生就会造成应用的停止响应。下面看一个死锁的例子

```py
#coding=utf-8
import threading
import time

class MyThread1(threading.Thread):
    def run(self):
        if mutexA.acquire():
            print(self.name+'----do1---up----')
            time.sleep(1)

            if mutexB.acquire():
                print(self.name+'----do1---down----')
                mutexB.release()
            mutexA.release()

class MyThread2(threading.Thread):
    def run(self):
        if mutexB.acquire():
            print(self.name+'----do2---up----')
            time.sleep(1)
            if mutexA.acquire():
                print(self.name+'----do2---down----')
                mutexA.release()
            mutexB.release()

mutexA = threading.Lock()
mutexB = threading.Lock()

if __name__ == '__main__':
    t1 = MyThread1()
    t2 = MyThread2()
    t1.start()
    t2.start()
```

运行结果： 
```
[root@test shell]# python test.py 
Thread-1----do1---up----
Thread-2----do2---up----

```
此时已经进入到了死锁状态，可以使用ctrl-z退出

2. 说明

![suixin](http://ox376n2jk.bkt.clouddn.com/%E6%AD%BB%E9%94%81.jpg)

## 2.同步应用

多个线程有序执行

```py
from threading import Thread,Lock
from time import sleep

class Task1(Thread):
    def run(self):
        while True:
            if lock1.acquire():
                print("------Task 1 -----")
                sleep(0.5)
                lock2.release()

class Task2(Thread):
    def run(self):
        while True:
            if lock2.acquire():
                print("------Task 2 -----")
                sleep(0.5)
                lock3.release()

class Task3(Thread):
    def run(self):
        while True:
            if lock3.acquire():
                print("------Task 3 -----")
                sleep(0.5)
                lock1.release()

#使用Lock创建出的锁默认没有“锁上”
lock1 = Lock()
#创建另外一把锁，并且“锁上”
lock2 = Lock()
lock2.acquire()
#创建另外一把锁，并且“锁上”
lock3 = Lock()
lock3.acquire()

t1 = Task1()
t2 = Task2()
t3 = Task3()

t1.start()
t2.start()
t3.start()
```
运行结果:
```
------Task 1 -----
------Task 2 -----
------Task 3 -----
------Task 1 -----
------Task 2 -----
------Task 3 -----
------Task 1 -----
------Task 2 -----
------Task 3 -----
------Task 1 -----
------Task 2 -----
------Task 3 -----
------Task 1 -----
------Task 2 -----
------Task 3 -----
...省略...
```
总结

* 可以使用互斥锁完成多个任务，有序的进程工作，这就是线程的同步