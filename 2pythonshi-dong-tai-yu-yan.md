## python是动态语言

### 1.动态语言的定义

动态编程语言 是 高级程序设计语言 的一个类别，在计算机科学领域已被广泛应用。它是一类 在运行时可以改变其结构的语言 ：例如新的函数、对象、甚至代码可以被引进，已有的函数可以被删除或是其他结构上的变化。动态语言目前非常具有活力。例如JavaScript便是一个动态语言，除此之外如 PHP 、 Ruby 、 Python 等也都属于动态语言，而 C 、 C++ 等语言则不属于动态语言。----来自 维基百科

### 2.运行的过程中给类绑定(添加)方法

```py
import types

#定义了一个类
class Person(object):
    num = 0
    def __init__(self, name = None, age = None):
        self.name = name
        self.age = age
    def eat(self):
        print("eat food")

#定义一个实例方法
def run(self, speed):
    print("%s在移动, 速度是 %d km/h"%(self.name, speed))

#定义一个类方法
@classmethod
def testClass(cls):
    cls.num = 100

#定义一个静态方法
@staticmethod
def testStatic():
    print("---static method----")

#创建一个实例对象
P = Person("老王", 24)
#调用在class中的方法
P.eat()

#给这个对象添加实例方法
P.run = types.MethodType(run, P)
#调用实例方法
P.run(180)

#给Person类绑定类方法
Person.testClass = testClass
#调用类方法
print(Person.num)
Person.testClass()
print(Person.num)

#给Person类绑定静态方法
Person.testStatic = testStatic
#调用静态方法
Person.testStatic()
```


### 3.运行的过程中删除属性、方法

删除的方法:

1. del 对象.属性名
2. delattr(对象, "属性名")

通过以上例子可以得出一个结论：相对于动态语言，静态语言具有严谨性！所以，玩动态语言的时候，小心动态的坑！

那么怎么避免这种情况呢？ 请使用__slots__，

### 4.\_\_slots\_\_

现在我们终于明白了，动态语言与静态语言的不同

* 动态语言：可以在运行的过程中，修改代码

* 静态语言：编译时已经确定好代码，运行过程中不能修改

如果我们想要限制实例的属性怎么办？比如，只允许对Person实例添加name和age属性。

为了达到限制的目的，Python允许在定义class的时候，定义一个特殊的\_\_slots\_\_变量，来限制该class实例能添加的属性：

```py
>>> class Person(object):
    __slots__ = ("name", "age")

>>> P = Person()
>>> P.name = "老王"
>>> P.age = 20
>>> P.score = 100
Traceback (most recent call last):
  File "<pyshell#3>", line 1, in <module>
AttributeError: Person instance has no attribute 'score'
```

**注意:**

使用\_\_slots\_\_要注意，\_\_slots\_\_定义的属性仅对当前类实例起作用，对继承的子类是不起作用的

```py
In [67]: class Test(Person):
    ...:     pass
    ...:

In [68]: t = Test()

In [69]: t.score = 100
```