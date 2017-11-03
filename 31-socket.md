## 1.socket简介

#### 1）本地的进程间通信（IPC）有很多种方式，例如

* 队列
* 同步（互斥锁、条件变量等）

以上通信方式都是在一台机器上不同进程之间的通信方式，那么问题来了

网络中进程之间如何通信？

#### 2）什么是socket

socket\(简称 套接字\) 是进程间通信的一种方式，它与其他进程间通信的一个主要不同是：

它能实现不同主机间的进程间通信，我们网络上各种各样的服务大多都是基于 Socket 来完成通信的

例如我们每天浏览网页、QQ 聊天、收发 email 等等

#### 3）创建socket

在 Python 中 使用socket 模块的函数 socket 就可以完成：

```py
socket.socket(AddressFamily, Type)
```

**说明：**

函数 socket.socket 创建一个 socket，返回该 socket 的描述符，该函数带有两个参数：

* Address Family：可以选择 AF\_INET（用于 Internet 进程间通信） 或者 AF\_UNIX（用于同一台机器进程间通信）,实际工作中常用AF\_INET

* Type：套接字类型，可以是 SOCK\_STREAM（流式套接字，主要用于 TCP 协议）或者 SOCK\_DGRAM（数据报套接字，主要用于 UDP 协议）

创建一个tcp socket（tcp套接字）

```
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

print 'Socket Created'
```

创建一个udp socket（udp套接字）

```
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

print 'Socket Created'
```

## 2.udp网络程序-发送数据

创建一个udp客户端程序的流程是简单，具体步骤如下：

1. 创建客户端套接字
2. 发送/接收数据
3. 关闭套接字

![suixin](http://ox376n2jk.bkt.clouddn.com/socket1.png)

代码如下：

```py
#coding=utf-8

from socket import *

#1. 创建套接字
udpSocket = socket(AF_INET, SOCK_DGRAM)

#2. 准备接收方的地址
sendAddr = ('192.168.1.103', 8080)

#3. 从键盘获取数据
sendData = raw_input("请输入要发送的数据:")

#4. 发送数据到指定的电脑上
udpSocket.sendto(sendData, sendAddr)

#5. 关闭套接字
udpSocket.close()
```

运行现象：

```
[root@test shell]# python test.py 
请输入要发送的数据:haha
```

在windows中运行“网络调试助手”：

![suixin](http://ox376n2jk.bkt.clouddn.com/socket2.png)

## 3.udp网络程序-发送、接收数据

```py
#coding=utf-8

from socket import *

#1. 创建套接字
udpSocket = socket(AF_INET, SOCK_DGRAM)

#2. 准备接收方的地址
sendAddr = ('192.168.1.103', 8080)

#3. 从键盘获取数据
sendData = raw_input("请输入要发送的数据:")

#4. 发送数据到指定的电脑上
udpSocket.sendto(sendData, sendAddr)

#5. 等待接收对方发送的数据
recvData = udpSocket.recvfrom(1024) # 1024表示本次接收的最大字节数

#6. 显示对方发送的数据
print(recvData)

#7. 关闭套接字
udpSocket.close()
```

运行结果：

```
[root@test shell]# python test.py 
请输入要发送的数据:hsh
('miss', ('192.168.25.168', 8080))
```

在windows中运行“网络调试助手”：  
![suixin](http://ox376n2jk.bkt.clouddn.com/socket3.png)

## 4.udp网络程序-端口问题

#### 会变的端口号

重新运行多次脚本，然后在“网络调试助手”中，看到的现象如下：

![suixin](http://ox376n2jk.bkt.clouddn.com/socket4.png)

**说明：**

* 每重新运行一次网络程序，上图中红圈中的数字，不一样的原因在于，这个数字标识这个网络程序，当重新运行时，如果没有确定到底用哪个，系统默认会随机分配

* 记住一点：这个网络程序在运行的过程中，这个就唯一标识这个程序，所以如果其他电脑上的网络程序如果想要向此程序发送数据，那么就需要向这个数字（即端口）标识的程序发送即可

为了防止这种情况，引进来了UDP绑定信息

#### 绑定端口号

```py
#coding=utf-8

from socket import *

#1. 创建套接字
udpSocket = socket(AF_INET, SOCK_DGRAM)

#2. 绑定本地的相关信息，如果一个网络程序不绑定，则系统会随机分配
bindAddr = ('', 7888) # ip地址和端口号，ip一般不用写，表示本机的任何一个ip
udpSocket.bind(bindAddr)

#3. 等待接收对方发送的数据
recvData = udpSocket.recvfrom(1024) # 1024表示本次接收的最大字节数

#4. 显示接收到的数据
print recvData

#5. 关闭套接字
udpSocket.close()
```

windows运行调试助手发送消息：

![suixin](http://ox376n2jk.bkt.clouddn.com/socket5.png)

脚本运行结果：

```
[root@test shell]# python test.py 
('I Am robin', ('192.168.25.168', 8080))
```

总结：

* 一个udp网络程序，可以不绑定，此时操作系统会随机进行分配一个端口，如果重新运行次程序端口可能会发生变化

* 一个udp网络程序，也可以绑定信息（ip地址，端口号），如果绑定成功，那么操作系统用这个端口号来进行区别收到的网络数据是否是此进程的

## 5.UDP应用-echo服务器

```py
#coding=utf-8

from socket import *

#1. 创建套接字
udpSocket = socket(AF_INET, SOCK_DGRAM)

#2. 绑定本地的相关信息
bindAddr = ('', 7788) # ip地址和端口号，ip一般不用写，表示本机的任何一个ip
udpSocket.bind(bindAddr)

num = 1
while True:

    #3. 等待接收对方发送的数据
    recvData = udpSocket.recvfrom(1024) # 1024表示本次接收的最大字节数

    #4. 将接收到的数据再发送给对方
    udpSocket.sendto(recvData[0], recvData[1])

    #5. 统计信息
    print('已经将接收到的第%d个数据返回给对方,内容为:%s'%(num,recvData[0]))
    num+=1


#5. 关闭套接字
udpSocket.close()
```

windows调试助手发送消息：

![suixin](http://ox376n2jk.bkt.clouddn.com/socket6.png)

运行结果：

```
[root@test shell]# python test.py 
已经将接收到的第1个数据返回给对方，内容为：i am 111111
已经将接收到的第2个数据返回给对方，内容为：i am 222222
已经将接收到的第3个数据返回给对方，内容为：i am 333333
已经将接收到的第4个数据返回给对方，内容为：i am 444444
```



