## 一、import导入模块

### 1.import 搜索路径

```py
>>> import sys
>>> sys.path
['', 
'/usr/local/python3/lib/python36.zip', 
'/usr/local/python3/lib/python3.6', 
'/usr/local/python3/lib/python3.6/lib-dynload', 
'/usr/local/python3/lib/python3.6/site-packages']
```

**路径搜索**

* 从上面列出的目录里依次查找要导入的模块文件

* ' ' 表示当前路径

**程序执行时导入模块路径**
```py
sys.path.append('/home/itcast/xxx')
sys.path.insert(0, '/home/itcast/xxx')    #可以确保先搜索这个路径
```
```py
In [37]: sys.path.insert(0,"/home/python/xxxx")
In [38]: sys.path
Out[38]: 
['/home/python/xxxx',
 '',
 '/usr/bin',
 '/usr/lib/python35.zip',
 '/usr/lib/python3.5',
 '/usr/lib/python3.5/plat-x86_64-linux-gnu',
 '/usr/lib/python3.5/lib-dynload',
 '/usr/local/lib/python3.5/dist-packages',
 '/usr/lib/python3/dist-packages',
 '/usr/lib/python3/dist-packages/IPython/extensions',
 '/home/python/.ipython']
```
### 2.重新导入模块

模块被导入后，import module不能重新导入模块，重新导入需用

* 测试模块内容 
```py
[root@test shell]# cat reload_test.py 
def test():
	print("----1----")
```
* 调用模块中的方法 
```py
>>> from imp import *    #从imp库导入重载模块reload
>>> from shell import reload_test
>>> reload_test.test()
----1----

```
* 修改测试模块 
```py
[root@test shell]# cat reload_test.py 
def test():
	print("----2----")
```
* 重新加载模块 
```py
>>> from shell import reload_test
>>> reload_test.test()
----1----
#修改后再次调用--发现没有改变
>>> reload_test.test()
----1----
#使用从imp库导入的reload来 重载模块
>>> reload(reload_test)
<module 'shell.reload_test' from '/shell/reload_test.py'>
>>> reload_test.test()
----2----
```

## 二、循环导入

### 1.循环导入问题

a.py
```py
from b import b 

print '---------this is module a.py----------'
def a():
    print("hello, a")
    b() 

a()
```
b.py
```py
from a import a

print '----------this is module b.py----------'
def b():
    print("hello, b")

def c():
    a() 
c()
```
运行python a.py
```py
[root@test shell]# python a.py 
Traceback (most recent call last):
  File "a.py", line 1, in <module>
    from b import b 
  File "/shell/b.py", line 1, in <module>
    from a import a
  File "/shell/a.py", line 1, in <module>
    from b import b 
ImportError: cannot import name 'b'
```

### 2.怎样避免循环导入

1. 程序设计上分层，降低耦合
2. 导入语句放在后面需要导入时再导入，例如放在函数体内导入