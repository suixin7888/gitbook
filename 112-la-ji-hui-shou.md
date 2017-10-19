## 垃圾回收

#### 1.小整数对象池

整数在程序中的使用非常广泛，Python为了优化速度，使用了小整数对象池， 避免为整数频繁申请和销毁内存空间。

Python 对小整数的定义是 [-5, 257) 这些整数对象是提前建立好的，不会被垃圾回收。在一个 Python 的程序中，所有位于这个范围内的整数使用的都是同一个对象.

同理，单个字母也是这样的。

但是当定义2个相同的字符串时，引用计数为0，触发垃圾回收

#### 2. 大整数对象池

每一个大整数，均创建一个新的对象。
```py
>>> b = 123456
>>> id(b)
140076242137744
>>> 
>>> a = 123456
>>> id(a)
140076242138064
>>> b=a
>>> id(b)
140076242138064
```
#### 3.intern机制

python中有这样一个机制——intern机制，让他只占用一个”abcde”所占的内存空间。靠引用计数去维护何时释放。
```py
>>> a = 'abcde'
>>> b = 'abcde'
>>> 
>>> id(a)
140076242331608
>>> id(b)
140076242331608
```
总结

* 小整数[-5,257)共用对象，常驻内存

* 单个字符共用对象，常驻内存

* 单个单词，不可修改，默认开启intern机制，共用对象，引用计数为0，则销毁 

* 字符串（含有空格），不可修改，没开启intern机制，不共用对象，引用计数为0，销毁

* 大整数不共用内存，引用计数为0，销毁 
```py
>>> b = 123456
>>> id(b)
140076242137744
>>> 
>>> a = 123456
>>> id(a)
140076242138064
```
* 数值类型和字符串类型在 Python 中都是不可变的，这意味着你无法修改这个对象的值，每次对变量的修改，实际上是创建一个新的对象 
```py
>>> a = 10
>>> id(a)
9088288
>>> a+=1
>>> id(a)
9088320
>>> 
>>> b='hello'
>>> id(b)
140076242331664
>>> 
>>> b = 'suixin'
>>> id(b)
140076242331608
```

## gc模块

#### 1.垃圾回收机制

Python中的垃圾回收是以引用计数为主，分代收集为辅。

1、导致引用计数+1的情况

对象被创建，例如a=23
对象被引用，例如b=a
对象被作为参数，传入到一个函数中，例如func(a)
对象作为一个元素，存储在容器中，例如list1=[a,a]
2、导致引用计数-1的情况

对象的别名被显式销毁，例如del a
对象的别名被赋予新的对象，例如a=24
一个对象离开它的作用域，例如f函数执行完毕时，func函数中的局部变量（全局变量不会）
对象所在的容器被销毁，或从容器中删除对象
3、查看一个对象的引用计数
```py
import sys
a = "hello world"
sys.getrefcount(a)
```
可以查看a对象的引用计数，但是比正常计数大1，因为调用函数的时候传入a，这会让a的引用计数+1

#### 2.循环引用导致内存泄露

引用计数的缺陷是循环引用的问题

```py
import gc

class ClassA():
    def __init__(self):
        print('object born,id:%s'%str(hex(id(self))))

def f2():
    while True:
        c1 = ClassA()
        c2 = ClassA()
        c1.t = c2
        c2.t = c1
        del c1
        del c2

#把python的gc关闭
gc.disable()

f2()
```

执行f2()，进程占用的内存会不断增大。

* 创建了c1，c2后这两块内存的引用计数都是1，执行c1.t=c2和c2.t=c1后，这两块内存的引用计数变成2.
* 在del c1后，内存1的对象的引用计数变为1，由于不是为0，所以内存1的对象不会被销毁，所以内存2的对象的引用数依然是2，在del c2后，同理，内存1的对象，内存2的对象的引用数都是1。
* 虽然它们两个的对象都是可以被销毁的，但是由于循环引用，导致垃圾回收器都不会回收它们，所以就会导致内存泄露。

#### 3.垃圾回收
```py
#coding=utf-8
import gc

class ClassA():
    def __init__(self):
        print('object born,id:%s'%str(hex(id(self))))
    # def __del__(self):
    #     print('object del,id:%s'%str(hex(id(self))))

def f3():
    print("-----0------")
    # print(gc.collect())
    c1 = ClassA()
    c2 = ClassA()
    c1.t = c2
    c2.t = c1
    print("-----1------")
    del c1
    del c2
    print("-----2------")
    print(gc.garbage)
    print("-----3------")
    print(gc.collect()) #显式执行垃圾回收
    print("-----4------")
    print(gc.garbage)
    print("-----5------")

if __name__ == '__main__':
    gc.set_debug(gc.DEBUG_LEAK) #设置gc模块的日志
    f3()
```

python2运行结果:
```
-----0------
object born,id:0x724b20
object born,id:0x724b48
-----1------
-----2------
[]
-----3------
gc: collectable <ClassA instance at 0x724b20>
gc: collectable <ClassA instance at 0x724b48>
gc: collectable <dict 0x723300>
gc: collectable <dict 0x71bf60>
4
-----4------
[<__main__.ClassA instance at 0x724b20>, <__main__.ClassA instance at 0x724b48>, {'t': <__main__.ClassA instance at 0x724b48>}, {'t': <__main__.ClassA instance at 0x724b20>}]
-----5------
```
说明:

* 垃圾回收后的对象会放在gc.garbage列表里面
* gc.collect()会返回不可达的对象数目，4等于两个对象以及它们对应的dict

**有三种情况会触发垃圾回收：**

1. 调用gc.collect(),
2. 当gc模块的计数器达到阀值的时候。
3. 程序退出的时候

#### 4.gc模块常用功能解析

gc模块提供一个接口给开发者设置垃圾回收的选项。上面说到，采用引用计数的方法管理内存的一个缺陷是循环引用，而gc模块的一个主要功能就是解决循环引用的问题。

常用函数：

1、gc.set_debug(flags) 设置gc的debug日志，一般设置为gc.DEBUG_LEAK

2、gc.collect([generation]) 显式进行垃圾回收，可以输入参数，0代表只检查第一代的对象，1代表检查一，二代的对象，2代表检查一，二，三代的对象，如果不传参数，执行一个full collection，也就是等于传2。 返回不可达（unreachable objects）对象的数目

3、gc.get_threshold() 获取的gc模块中自动执行垃圾回收的频率。

4、gc.set_threshold(threshold0[, threshold1[, threshold2]) 设置自动执行垃圾回收的频率。

5、gc.get_count() 获取当前自动执行垃圾回收的计数器，返回一个长度为3的列表

**gc模块的自动垃圾回收机制**

必须要import gc模块，并且is_enable()=True才会启动自动垃圾回收。

这个机制的主要作用就是发现并处理不可达的垃圾对象。

垃圾回收=垃圾检查+垃圾回收

在Python中，采用分代收集的方法。把对象分为三代，一开始，对象在创建的时候，放在一代中，如果在一次一代的垃圾检查中，改对象存活下来，就会被放到二代中，同理在一次二代的垃圾检查中，该对象存活下来，就会被放到三代中。

gc模块里面会有一个长度为3的列表的计数器，可以通过gc.get_count()获取。

例如(488,3,0)，其中488是指距离上一次一代垃圾检查，Python分配内存的数目减去释放内存的数目，注意是内存分配，而不是引用计数的增加。例如：
```py
print gc.get_count() # (590, 8, 0)
a = ClassA()
print gc.get_count() # (591, 8, 0)
del a
print gc.get_count() # (590, 8, 0)
```
3是指距离上一次二代垃圾检查，一代垃圾检查的次数，同理，0是指距离上一次三代垃圾检查，二代垃圾检查的次数。

gc模快有一个自动垃圾回收的阀值，即通过gc.get_threshold函数获取到的长度为3的元组，例如(700,10,10) 每一次计数器的增加，gc模块就会检查增加后的计数是否达到阀值的数目，如果是，就会执行对应的代数的垃圾检查，然后重置计数器

例如，假设阀值是(700,10,10)：
```
当计数器从(699,3,0)增加到(700,3,0)，gc模块就会执行gc.collect(0),即检查一代对象的垃圾，并重置计数器为(0,4,0)
当计数器从(699,9,0)增加到(700,9,0)，gc模块就会执行gc.collect(1),即检查一、二代对象的垃圾，并重置计数器为(0,0,1)
当计数器从(699,9,9)增加到(700,9,9)，gc模块就会执行gc.collect(2),即检查一、二、三代对象的垃圾，并重置计数器为(0,0,0)
```

#### 注意点

gc模块唯一处理不了的是循环引用的类都有__del__方法，所以项目中要避免定义__del__方法
```py
import gc

class ClassA():
    pass
    # def __del__(self):
    #     print('object born,id:%s'%str(hex(id(self))))

gc.set_debug(gc.DEBUG_LEAK)
a = ClassA()
b = ClassA()

a.next = b
b.prev = a

print "--1--"
print gc.collect()
print "--2--"
del a
print "--3--"
del b
print "--3-1--"
print gc.collect()
print "--4--"
```
运行结果：
```
--1--
0
--2--
--3--
--3-1--
gc: collectable <ClassA instance at 0x21248c8>
gc: collectable <ClassA instance at 0x21248f0>
gc: collectable <dict 0x2123030>
gc: collectable <dict 0x2123150>
4
--4--
```
如果把del打开，运行结果为:
```
--1--
0
--2--
--3--
--3-1--
gc: uncollectable <ClassA instance at 0x6269b8>
gc: uncollectable <ClassA instance at 0x6269e0>
gc: uncollectable <dict 0x61bed0>
gc: uncollectable <dict 0x6230c0>
4
--4--
```

