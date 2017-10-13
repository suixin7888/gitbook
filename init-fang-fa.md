### \_\_init\_\_()方法

想一想:

在上一小节的demo中，我们已经给BMW这个对象添加了2个属性，wheelNum（车的轮胎数量）以及color（车的颜色），试想如果再次创建一个对象的话，肯定也需要进行添加属性，显然这样做很费事，那么有没有办法能够在创建对象的时候，就顺便把车这个对象的属性给设置呢？

答:

\_\_init\_\_()方法

<1>使用方式
def 类名:
    #初始化函数，用来完成一些默认的设定
    def \_\_init\_\_():
        pass
<2>\_\_init\_\_()方法的调用

```py
#定义汽车类
class Car:

    def __init__(self):
        self.wheelNum = 4
        self.color = '蓝色'

    def move(self):
        print('车在跑，目标:夏威夷')

#创建对象

BMW = Car()

print('车的颜色为:%s'%BMW.color)
print('车轮胎数量为:%d'%BMW.wheelNum)
```

总结1

当创建Car对象后，在没有调用\_\_init\_\_()方法的前提下，BMW就默认拥有了2个属性wheelNum和color，原因是\_\_init\_\_()方法是在创建对象后，就立刻被默认调用了

想一想

既然在创建完对象后\_\_init\_\_()方法已经被默认的执行了，那么能否让对象在调用\_\_init\_\_()方法的时候传递一些参数呢？如果可以，那怎样传递呢？

```py
#定义汽车类
class Car:

    def __init__(self, newWheelNum, newColor):
        self.wheelNum = newWheelNum
        self.color = newColor

    def move(self):
        print('车在跑，目标:夏威夷')

#创建对象
BMW = Car(4, 'green')

print('车的颜色为:%s'%BMW.color)
print('车轮子数量为:%d'%BMW.wheelNum)
```

总结2

\_\_init\_\_()方法，在创建一个对象时默认被调用，不需要手动调用
\_\_init\_\_(self)中，默认有1个参数名字为self，如果在创建对象时传递了2个实参，那么\_\_init\_\_(self)中出了self作为第一个形参外还需要2个形参，例如\_\_init\_\_(self,x,y)
\_\_init\_\_(self)中的self参数，不需要开发者传递，python解释器会自动把当前的对象引用传递进去

### 定义\_\_str\_\_()方法
```py
class Car:

    def __init__(self, newWheelNum, newColor):
        self.wheelNum = newWheelNum
        self.color = newColor

    def __str__(self):
        msg = "嘿。。。我的颜色是" + self.color + "我有" + int(self.wheelNum) + "个轮胎..."
        return msg

    def move(self):
        print('车在跑，目标:夏威夷')


BMW = Car(4, "白色")
print(BMW)
```

总结:

在python中方法名如果是__xxxx__()的，那么就有特殊的功能，因此叫做“魔法”方法
当使用print输出对象的时候，只要自己定义了__str__(self)方法，那么就会打印从在这个方法中return的数据

### \_\_del\_\_()方法

创建对象后，python解释器默认调用\_\_init\_\_()方法；

当删除一个对象时，python解释器也会默认调用一个方法，这个方法为\_\_del\_\_()方法

```py
import time
class Animal(object):

    # 初始化方法
    # 创建完对象后会自动被调用
    def __init__(self, name):
        print('__init__方法被调用')
        self.__name = name


    # 析构方法
    # 当对象被删除时，会自动被调用
    def __del__(self):
        print("__del__方法被调用")
        print("%s对象马上被干掉了..."%self.__name)

# 创建对象
dog = Animal("哈皮狗")

# 删除对象
del dog


cat = Animal("波斯猫")
cat2 = cat
cat3 = cat

print("---马上 删除cat对象")
del cat
print("---马上 删除cat2对象")
del cat2
print("---马上 删除cat3对象")
del cat3

print("程序2秒钟后结束")
time.sleep(2)
```

总结

* 当有1个变量保存了对象的引用时，此对象的引用计数就会加1
* 当使用del删除变量指向的对象时，如果对象的引用计数不会1，比如3，那么此时只会让这个引用计数减1，即变为2，当再次调用del时，变为1，如果再调用1次del，此时会真的把对象进行删除



