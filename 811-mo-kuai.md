## 模块的导入
#### 1.import
在Python中用关键字import来引入某个模块，比如要引用模块math，就可以在文件最开始的地方用import math来引入。

形如:
```py
import module1,mudule2...
```
当解释器遇到import语句，如果模块在当前的搜索路径就会被导入。
```py
import math

#这样会报错
print sqrt(2)

#这样才能正确输出结果
print math.sqrt(2)
```

有时候我们只需要用到模块中的某个函数，只需要引入该函数即可，此时可以用下面方法实现：
```py
from 模块名 import 函数名1,函数名2....
```
不仅可以引入函数，还可以引入一些全局变量、类等

* 注意:

>   通过这种方式引入的时候，调用函数时只能给出函数名，不能给出模块名，但是当两个模块中含有相同名称函数的时候，后面一次引入会覆盖前一次引入。也就是说假如模块A中有函数function( )，在模块B中也有函数function( )，如果引入A中的function在先、B中的function在后，那么当调用function函数的时候，是去执行模块B中的function函数。

>   如果想一次性引入math中所有的东西，还可以通过from math import *来实现

#### 2.from…import
Python的from语句让你从模块中导入一个指定的部分到当前命名空间中

语法如下：
```py
from modname import name1[, name2[, ... nameN]]
```
例如，要导入模块fib的fibonacci函数，使用如下语句：
```py
from fib import fibonacci
```

注意

* 不会把整个fib模块导入到当前的命名空间中，它只会将fib里的fibonacci单个引入


#### 3.from … import *

把一个模块的所有内容全都导入到当前的命名空间也是可行的，只需使用如下声明：

```py
from modname import *
```
注意

* 这提供了一个简单的方法来导入一个模块中的所有项目。然而这种声明不该被过多地使用。

#### 4. as

```py
In [1]: import time as tt

In [2]: time.sleep(1)
---------------------------------------------------------------------------
NameError                                 Traceback (most recent call last)
<ipython-input-2-07a34f5b1e42> in <module>()
----> 1 time.sleep(1)

NameError: name 'time' is not defined

In [3]: 

In [3]: tt.sleep(1)

In [4]: 

In [4]: from time import sleep as sp

In [5]: sleep(1)
---------------------------------------------------------------------------
NameError                                 Traceback (most recent call last)
<ipython-input-5-82e5c2913b44> in <module>()
----> 1 sleep(1)

NameError: name 'sleep' is not defined

In [6]: sp(1)

```
#### 5.定位模块

当你导入一个模块，Python解析器对模块位置的搜索顺序是：

1.当前目录

2.如果不在当前目录，Python则搜索在shell变量PYTHONPATH下的每个目录。

3.如果都找不到，Python会察看默认路径。UNIX下，默认路径一般为/usr/local/lib/python/

4.模块搜索路径存储在system模块的sys.path变量中。变量里包含当前目录，PYTHONPATH和由安装过程决定的默认目录。

