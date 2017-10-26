## 进程的创建-fork

#### 1.进程 VS 程序

编写完毕的代码，在没有运行的时候，称之为程序

正在运行着的代码，就成为进程

进程，除了包含代码以外，还有需要运行的环境等，所以和程序是有区别的

#### 2.fork\( \)

Python的os模块封装了常见的系统调用，其中就包括fork，可以在Python程序中轻松创建子进程：

```py
import os

# 注意，fork函数，只在Unix/Linux/Mac上运行，windows不可以
pid = os.fork()

if pid == 0:
    print('111111')
else:
    print('2222222222')
```

运行结果：

```py
[root@test shell]# python test.py 
2222222222
111111
```

说明：

* 程序执行到os.fork\(\)时，操作系统会创建一个新的进程（子进程），然后复制父进程的所有信息到子进程中

* 然后父进程和子进程都会从fork\(\)函数中得到一个返回值，在子进程中这个值一定是0，而父进程中是子进程的 id号

在Unix/Linux操作系统中，提供了一个fork\(\)系统函数，它非常特殊。

普通的函数调用，调用一次，返回一次，但是fork\(\)调用一次，返回两次，因为操作系统自动把当前进程（称为父进程）复制了一份（称为子进程），然后，分别在父进程和子进程内返回。

子进程永远返回0，而父进程返回子进程的ID。

这样做的理由是，一个父进程可以fork出很多子进程，所以，父进程要记下每个子进程的ID，而子进程只需要调用getppid\(\)就可以拿到父进程的ID。

多进程中，每个进程中所有数据（包括全局变量）都各有拥有一份，互不影响

#### 3.getpid\(\)、getppid\(\)

```py
import os

rpid = os.fork()
if rpid<0:
    print("fork调用失败。")
elif rpid == 0:
    print("我是子进程（%s），我的父进程是（%s）"%(os.getpid(),os.getppid()))
    x+=1
else:
    print("我是父进程（%s），我的子进程是（%s）"%(os.getpid(),rpid))

print("父子进程都可以执行这里的代码")
```

运行结果：

```
我是父进程（19360），我的子进程是（19361）
父子进程都可以执行这里的代码
我是子进程（19361），我的父进程是（19360）
父子进程都可以执行这里的代码
```

#### 4.多次fork问题

如果在一个程序，有2次的fork函数调用，是否就会有3个进程呢？

```py
#coding=utf-8
import os
import time

# 注意，fork函数，只在Unix/Linux/Mac上运行，windows不可以
pid = os.fork()
if pid == 0:
    print('哈哈1')
else:
    print('哈哈2')

pid = os.fork()
if pid == 0:
    print('哈哈3')
else:
    print('哈哈4')

time.sleep(1)
```

运行结果：

```
[root@test shell]# python test.py 
哈哈2
哈哈4
哈哈3
哈哈1
哈哈4
哈哈3
```

**说明**  
![suixin](http://ox376n2jk.bkt.clouddn.com/fork.png)

总结：

* 父进程、子进程执行顺序没有规律，完全取决于操作系统的调度算法



