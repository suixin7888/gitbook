## 多线程-threading

python的thread模块是比较底层的模块，python的threading模块是对thread做了一些包装的，可以更加方便的被使用

#### 1.使用threading模块

单线程执行
```py
#coding=utf-8
import time

def saySorry():
    print("亲爱的，我错了，我能吃饭了吗？")
    time.sleep(1)

if __name__ == "__main__":
    for i in range(5):
        saySorry()
```
运行结果：
```
[root@test shell]# python test.py 
亲爱的，我错了，我能吃饭了吗？
亲爱的，我错了，我能吃饭了吗？
亲爱的，我错了，我能吃饭了吗？
亲爱的，我错了，我能吃饭了吗？
亲爱的，我错了，我能吃饭了吗？
```

多线程执行

```py
#coding=utf-8
import threading
import time

def saySorry():
    print("亲爱的，我错了，我能吃饭了吗？")
    time.sleep(1)

if __name__ == "__main__":
    for i in range(5):
        t = threading.Thread(target=saySorry)
        t.start() #启动线程，即让线程开始执行
```
运行结果：
```
[root@test shell]# python test.py 
亲爱的，我错了，我能吃饭了吗？
亲爱的，我错了，我能吃饭了吗？
亲爱的，我错了，我能吃饭了吗？
亲爱的，我错了，我能吃饭了吗？
亲爱的，我错了，我能吃饭了吗？
```

说明

* 可以明显看出使用了多线程并发的操作，花费时间要短很多
* 创建好的线程，需要调用start()方法来启动

#### 2.主线程会等待所有的子线程结束后才结束
```py
#coding=utf-8
import threading
from time import sleep,ctime

def sing():
    for i in range(3):
        print("正在唱歌...%d"%i)
        sleep(1)

def dance():
    for i in range(3):
        print("正在跳舞...%d"%i)
        sleep(1)

if __name__ == '__main__':
    print('---开始---:%s'%ctime())

    t1 = threading.Thread(target=sing)
    t2 = threading.Thread(target=dance)

    t1.start()
    t2.start()

    #sleep(5) # 屏蔽此行代码，试试看，程序是否会立马结束？
    print('---结束---:%s'%ctime())
```

#### 3.查看线程数量
```py
#coding=utf-8
import threading
from time import sleep,ctime

def sing():
    for i in range(3):
        print("正在唱歌...%d"%i)
        sleep(1)

def dance():
    for i in range(3):
        print("正在跳舞...%d"%i)
        sleep(1)

if __name__ == '__main__':
    print('---开始---:%s'%ctime())

    t1 = threading.Thread(target=sing)
    t2 = threading.Thread(target=dance)

    t1.start()
    t2.start()

    while True:
        length = len(threading.enumerate())
        print('当前运行的线程数为：%d'%length)
        if length<=1:
            break

        sleep(0.5)
```

## threading注意点

#### 1.线程执行代码的封装

通过上一小节，能够看出，通过使用threading模块能完成多任务的程序开发，为了让每个线程的封装性更完美，所以使用threading模块时，往往会定义一个新的子类class，只要继承threading.Thread就可以了，然后重写run方法

示例如下：

```py
#coding=utf-8
import threading
import time

class MyThread(threading.Thread):
    def run(self):
        for i in range(3):
            time.sleep(1)
            msg = "I'm "+self.name+' @ '+str(i) #name属性中保存的是当前线程的名字
            print(msg)


if __name__ == '__main__':
    t = MyThread()
    t.start()

```
说明

python的threading.Thread类有一个run方法，用于定义线程的功能函数，可以在自己的线程类中覆盖该方法。而创建自己的线程实例后，通过Thread类的start方法，可以启动该线程，交给python虚拟机进行调度，当该线程获得执行的机会时，就会调用run方法执行线程。

#### 2.线程的执行顺序
```py
#coding=utf-8
import threading
import time

class MyThread(threading.Thread):
    def run(self):
        for i in range(3):
            time.sleep(1)
            msg = "I'm "+self.name+' @ '+str(i)
            print(msg)
def test():
    for i in range(5):
        t = MyThread()
        t.start()
if __name__ == '__main__':
    test()
```

执行结果：(运行的结果可能不一样，但是大体是一致的)
```
    I'm Thread-1 @ 0
    I'm Thread-2 @ 0
    I'm Thread-5 @ 0
    I'm Thread-3 @ 0
    I'm Thread-4 @ 0
    I'm Thread-3 @ 1
    I'm Thread-4 @ 1
    I'm Thread-5 @ 1
    I'm Thread-1 @ 1
    I'm Thread-2 @ 1
    I'm Thread-4 @ 2
    I'm Thread-5 @ 2
    I'm Thread-2 @ 2
    I'm Thread-1 @ 2
    I'm Thread-3 @ 2
```
说明

从代码和执行结果我们可以看出，多线程程序的执行顺序是不确定的。当执行到sleep语句时，线程将被阻塞（Blocked），到sleep结束后，线程进入就绪（Runnable）状态，等待调度。而线程调度将自行选择一个线程执行。上面的代码中只能保证每个线程都运行完整个run函数，但是线程的启动顺序、run函数中每次循环的执行顺序都不能确定。

#### 3.总结

1. 每个线程一定会有一个名字，尽管上面的例子中没有指定线程对象的name，但是python会自动为线程指定一个名字。

2. 当线程的run()方法结束时该线程完成。

3. 无法控制线程调度程序，但可以通过别的方式来影响线程调度的方式。

4. 线程的几种状态
