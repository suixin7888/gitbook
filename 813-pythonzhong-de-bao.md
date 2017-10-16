## python中的包

#### 1.将两个有关系的模块放到同一文件夹下
![suixin](http://ox376n2jk.bkt.clouddn.com/%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A15.png)

#### 2.使用import 文件.模块的方式导入
![suixin](http://ox376n2jk.bkt.clouddn.com/%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A16.png)

#### 3.使用from 文件夹 import 模块 的方式导入
![suixin](http://ox376n2jk.bkt.clouddn.com/%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A17.png)

#### 4.在msg文件夹下创建__init__.py文件
![suixin](http://ox376n2jk.bkt.clouddn.com/%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A18.png)

#### 5.在__init__.py文件中写入
![suixin](http://ox376n2jk.bkt.clouddn.com/%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A19.png)

#### 6.重新使用from 文件夹 import 模块 的方式导入
![suixin](http://ox376n2jk.bkt.clouddn.com/%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A110.png)

总结：

* 包将有联系的模块组织在一起，即放到同一个文件夹下，并且在这个文件夹创建一个名字为__init__.py 文件，那么这个文件夹就称之为包
* 文件夹下的__init__.py文件控制着包的导入行为
* 文件夹下的__init__.py为空，仅仅是把这个包导入，不会导入包中的模块
* 在__init__.py文件中，定义一个__all__变量，它控制着 from 包名 import *时导入的模块

