## 元类

### 1. 类也是对象

在大多数编程语言中，类就是一组用来描述如何生成一个对象的代码段。在Python中这一点仍然成立：

```py
>>> class ObjectCreator(object):
… pass
…
>>> my_object = ObjectCreator()
>>> print my_object
<__main__.ObjectCreator object at 0x8974f2c>
```

### 2.动态的创建类

因为类也是对象，你可以在运行时动态的创建它们，就像其他任何对象一样。首先，你可以在函数中创建类，使用class关键字即可。

```py
>>> def choose_class(name):
… if name == 'foo':
… class Foo(object):
… pass
… return Foo # 返回的是类，不是类的实例
… else:
… class Bar(object):
… pass
… return Bar
…
>>> MyClass = choose_class('foo')
>>> print MyClass # 函数返回的是类，不是类的实例
<class '__main__'.Foo>
>>> print MyClass() # 你可以通过这个类创建类实例，也就是对象
<__main__.Foo object at 0x89c6d4c>
```

### 3.使用type创建类

type还有一种完全不同的功能，动态的创建类。

type\(类名, 由父类名称组成的元组（针对继承的情况，可以为空），包含属性的字典（名称和值）\)

比如下面的代码：

```py
In [2]: class Test: #定义了一个Test类
...: pass
...:
In [3]: Test() #创建了一个Test类的实例对象
Out[3]: <__main__.Test at 0x10d3f8438>
```

可以手动像这样创建：

```py
Test2 = type("Test2",(),{}) #定了一个Test2类
In [5]: Test2() #创建了一个Test2类的实例对象
Out[5]: <__main__.Test2 at 0x10d406b38>
```

我们使用"Test2"作为类名，并且也可以把它当做一个变量来作为类的引用。类和变量是不同的，这里没有任何理由把事情弄的复杂。即type函数中第1个实参，也可以叫做其他的名字，这个名字表示类的名字

```py
In [23]: MyDogClass = type('MyDog', (), {})

In [24]: print MyDogClass
<class '__main__.MyDog'>
```

使用help来测试这2个类

```py
In [10]: help(Test) #用help查看Test类

Help on class Test in module __main__:

class Test(builtins.object)
Data descriptors defined here:
|
| __dict__
| dictionary for instance variables (if defined)
|
| __weakref__
| list of weak references to the object (if defined)
In [8]: help(Test2) #用help查看Test2类

Help on class Test2 in module __main__:

class Test2(builtins.object)
Data descriptors defined here:
|
| __dict__
| dictionary for instance variables (if defined)
|
| __weakref__
| list of weak references to the object (if defined)
```

### 4.使用type创建带有属性的类

type 接受一个字典来为类定义属性，因此

```py
>>> Foo = type('Foo', (), {'bar':True})
```

可以翻译为：

```py
>>> class Foo(object):
… bar = True
```

当然，你可以向这个类继承，所以，如下的代码：

```py
>>> class FooChild(Foo):
… pass
```

就可以写成：

```py
>>> FooChild = type('FooChild', (Foo,),{})
>>> print FooChild
<class '__main__.FooChild'>
>>> print FooChild.bar # bar属性是由Foo继承而来
True
```

注意：

* type的第2个参数，元组中是父类的名字，而不是字符串
* 添加的属性是类属性，并不是实例属性

### 5.使用type创建带有方法的类

**添加实例方法**

```py
In [46]: def echo_bar(self): #定义了一个普通的函数
    ...:     print(self.bar)
    ...:

In [47]: FooChild = type('FooChild', (Foo,), {'echo_bar': echo_bar}) #让FooChild类中的echo_bar属性，指向了上面定义的函数

In [48]: hasattr(Foo, 'echo_bar') #判断Foo类中，是否有echo_bar这个属性
Out[48]: False

In [49]:

In [49]: hasattr(FooChild, 'echo_bar') #判断FooChild类中，是否有echo_bar这个属性
Out[49]: True

In [50]: my_foo = FooChild()

In [51]: my_foo.echo_bar()
True
```

**添加静态方法**

```py
In [36]: @staticmethod
    ...: def testStatic():
    ...:     print("static method ....")
    ...:

In [37]: Foochild = type('Foochild', (Foo,), {"echo_bar":echo_bar, "testStatic":
    ...: testStatic})

In [38]: fooclid = Foochild()

In [39]: fooclid.testStatic
Out[39]: <function __main__.testStatic>

In [40]: fooclid.testStatic()
static method ....

In [41]: fooclid.echo_bar()
True
```
**添加类方法**

```py
In [42]: @classmethod
    ...: def testClass(cls):
    ...:     print(cls.bar)
    ...:

In [43]:

In [43]: Foochild = type('Foochild', (Foo,), {"echo_bar":echo_bar, "testStatic":
    ...: testStatic, "testClass":testClass})

In [44]:

In [44]: fooclid = Foochild()

In [45]: fooclid.testClass()
True
```

