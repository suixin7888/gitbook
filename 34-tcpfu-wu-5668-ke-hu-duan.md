## TCP

#### 1.tcp相关介绍

![suixin](http://ox376n2jk.bkt.clouddn.com/udp广播.png)

#### 2.tcp服务器

完成一个tcp服务器的功能，需要的流程如下：

1. socket创建一个套接字
2. bind绑定ip和port
3. listen使套接字变为可以被动链接
4. accept等待客户端的链接
5. recv/send接收发送数据

一个很简单的tcp服务器如下：

```py
#coding=utf-8
from socket import *

# 创建socket
tcpSerSocket = socket(AF_INET, SOCK_STREAM)

# 绑定本地信息
address = ('', 7788)
tcpSerSocket.bind(address)

# 使用socket创建的套接字默认的属性是主动的，使用listen将其变为被动的，这样就可以接收别人的链接了
tcpSerSocket.listen(5)

# 如果有新的客户端来链接服务器，那么就产生一个新的套接字专门为这个客户端服务器
# newSocket用来为这个客户端服务
# tcpSerSocket就可以省下来专门等待其他新客户端的链接
newSocket, clientAddr = tcpSerSocket.accept()

# 接收对方发送过来的数据，最大接收1024个字节
recvData = newSocket.recv(1024)
print('接收到的数据为:',recvData)

# 发送一些数据到客户端
newSocket.send("thank you !")

# 关闭为这个客户端服务的套接字，只要关闭了，就意味着为不能再为这个客户端服务了，如果还需要服务，只能再次重新连接
newSocket.close()

# 关闭监听套接字，只要这个套接字关闭了，就意味着整个程序不能再接收任何新的客户端的连接
tcpSerSocket.close()
```

#### 3.tcp客户端

tcp客户端构建流程

tcp的客户端要比服务器端简单很多，如果说服务器端是需要自己买手机、查手机卡、设置铃声、等待别人打电话流程的话，那么客户端就只需要找一个电话亭，拿起电话拨打即可，流程要少很多

示例代码：

```py
#coding=utf-8
from socket import *

# 创建socket
tcpClientSocket = socket(AF_INET, SOCK_STREAM)

# 链接服务器，假设服务器ip为192.168.1.102
serAddr = ('192.168.1.102', 7788)
tcpClientSocket.connect(serAddr)

# 提示用户输入数据
sendData = raw_input("请输入要发送的数据：")
#python3：
    sendData = input("请输入要发送的数据：")

tcpClientSocket.send(sendData.encode())

# 接收对方发送过来的数据，最大接收1024个字节
recvData = tcpClientSocket.recv(1024)
print '接收到的数据为:',recvData

# 关闭套接字
tcpClientSocket.close()
```

**服务端和客户端可以搭配测试**

```
#客户端：

[root@server opt]# python kehu.py 
请输入发送的数据:suixin
接受到的数据: b'thank you!'

#服务端：

[root@test shell]# python test.py 
('\xe6\x8e\xa5\xe6\x94\xb6\xe5\x88\xb0\xe6\x95\xb0\xe6\x8d\xae\xe4\xb8\xba:', 'suixin')
```

#### 4.应用-模拟QQ聊天

**客户端参考代码**

```py
#coding=utf-8
from socket import *

# 创建socket
tcpClientSocket = socket(AF_INET, SOCK_STREAM)

# 链接服务器
serAddr = ('192.168.1.102', 7788)
tcpClientSocket.connect(serAddr)

while True:

    # 提示用户输入数据
    sendData = input("send：")
#python2:
    #sendData = raw_input("send：")
    if len(sendData)>0:
        tcpClientSocket.send(sendData.encode())
    else:
        break

    # 接收对方发送过来的数据，最大接收1024个字节
    recvData = tcpClientSocket.recv(1024)
    print('recv:',recvData)

# 关闭套接字
tcpClientSocket.close()
```

**服务器端参考代码**

```py
#coding=utf-8
from socket import *

# 创建socket
tcpSerSocket = socket(AF_INET, SOCK_STREAM)

# 绑定本地信息
address = ('', 7788)
tcpSerSocket.bind(address)

# 使用socket创建的套接字默认的属性是主动的，使用listen将其变为被动的，这样就可以接收别人的链接了
tcpSerSocket.listen(5)

while True:

    # 如果有新的客户端来链接服务器，那么就产生一个信心的套接字专门为这个客户端服务器
    # newSocket用来为这个客户端服务
    # tcpSerSocket就可以省下来专门等待其他新客户端的链接
    newSocket, clientAddr = tcpSerSocket.accept()

    while True:

        # 接收对方发送过来的数据，最大接收1024个字节
        recvData = newSocket.recv(1024)

        # 如果接收的数据的长度为0，则意味着客户端关闭了链接
        if len(recvData)>0:
            print 'recv:',recvData
        else:
            break

        # 发送一些数据到客户端
        sendData = input("send:")
        newSocket.send(sendData.encode())

    # 关闭为这个客户端服务的套接字，只要关闭了，就意味着为不能再为这个客户端服务了，如果还需要服务，只能再次重新连接
    newSocket.close()

# 关闭监听套接字，只要这个套接字关闭了，就意味着整个程序不能再接收任何新的客户端的连接
tcpSerSocket.close()
```



