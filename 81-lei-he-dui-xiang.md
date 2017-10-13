### 类和对象
面向对象编程的2个非常重要的概念：类和对象

对象是面向对象编程的核心，在使用对象的过程中，为了将具有共同特征和行为的一组对象抽象定义，提出了另外一个新的概念——类


#### 定义类
```py
class Car:
    # 移动
    def move(self):
        print('车在奔跑...')

    # 鸣笛
    def toot(self):
        print("车在鸣笛...嘟嘟..")
```
说明：

* 定义类时有2种：新式类和经典类，上面的Car为经典类，如果是Car(object)则为新式类类名 的命名规则按照"大驼峰"


#### 创建对象

python中，可以根据已经定义的类去创建出一个个对象

创建对象的格式为:

对象名 = 类名()
创建对象demo:
```py
# 创建一个对象，并用变量BMW来保存它的引用
BMW = Car()
BMW.color = '黑色'
BMW.wheelNum = 4 #轮子数量
BMW.move()
BMW.toot()
print(BMW.color)
print(BMW.wheelNum)
```



