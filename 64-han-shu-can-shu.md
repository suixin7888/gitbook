## 函数参数

### 1. 定义带有参数的函数

示例如下：

```py
def add2num(a, b):
c = a+b
print c
```

### 2. 调用带有参数的函数

以调用上面的add2num\(a, b\)函数为例:

```py
def add2num(a, b):
c = a+b
print c
#调用带有参数的函数时，需要在小括号中，传递数据
```


### 3. 缺省参数

调用函数时，缺省参数的值如果没有传入，则被认为是默认值。下例会打印默认的age，如果age没有被传入：
```py
def printinfo( name, age = 35 ):
   # 打印任何传入的字符串
   print "Name: ", name
   print "Age ", age

# 调用printinfo函数
printinfo(name="miki" )
printinfo( age=9,name="miki" )
```
以上实例输出结果：
```
Name:  miki
Age  35
Name:  miki
Age  9
```

注意：带有默认值的参数一定要位于参数列表的最后面。
```py
>>> def printinfo(name, age=35, sex):
...     print name
...
  File "<stdin>", line 1
SyntaxError: non-default argument follows default argument
```

### 4.不定长参数

有时可能需要一个函数能处理比当初声明时更多的参数。这些参数叫做不定长参数，声明时不会命名。

基本语法如下：
```py
def functionname([formal_args,] *args, **kwargs):
    "函数_文档字符串"
    function_suite
    return [expression]
```
加了星号（*）的变量args会存放所有未命名的变量参数，args为元组；而加**的变量kwargs会存放命名参数，即形如key=value的参数， kwargs为字典。
```py
>>> def fun(a, b, *args, **kwargs):
...     """可变参数演示示例"""
...     print "a =", a
...     print "b =", b
...     print "args =", args
...     print "kwargs: "
...     for key, value in kwargs.items():
...         print key, "=", value
...
>>> fun(1, 2, 3, 4, 5, m=6, n=7, p=8)  # 注意传递的参数对应
a = 1
b = 2
args = (3, 4, 5)
kwargs: 
p = 8
m = 6
n = 7
>>>
>>>
>>>
>>> c = (3, 4, 5)
>>> d = {"m":6, "n":7, "p":8}
>>> fun(1, 2, *c, **d)    # 注意元组与字典的传参方式
a = 1
b = 2
args = (3, 4, 5)
kwargs: 
p = 8
m = 6
n = 7
>>>
>>>
>>>
>>> fun(1, 2, c, d) # 注意不加星号与上面的区别
a = 1
b = 2
args = ((3, 4, 5), {'p': 8, 'm': 6, 'n': 7})
kwargs:
>>>
```
### 5. 引用传参

可变类型与不可变类型的变量分别作为函数参数时，会有什么不同吗？
Python有没有类似C语言中的指针传参呢？
```py
>>> def selfAdd(a):
...     """自增"""
...     a += a
...
>>> a_int = 1
>>> a_int
1
>>> selfAdd(a_int)
>>> a_int
1
>>> a_list = [1, 2]
>>> a_list
[1, 2]
>>> selfAdd(a_list)
>>> a_list
[1, 2, 1, 2]
```
Python中函数参数是引用传递（注意不是值传递）。对于不可变类型，因变量不能修改，所以运算不会影响到变量自身；
而对于可变类型来说，函数体中的运算有可能会更改传入的参数变量。
