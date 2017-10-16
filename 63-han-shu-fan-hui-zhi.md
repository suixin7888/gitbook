## 函数返回值

### 1.带有返回值的函数
想要在函数中把结果返回给调用者，需要在函数中使用return

如下示例:
```py
def add2num(a, b):
	c = a+b
	return c
```
或者
```py
def add2num(a, b):
	return a+b
```
### 2.保存函数的返回值
师父说阿
保存函数的返回值示例如下:
```py
#定义函数
def add2num(a, b):
	return a+b

#调用函数，顺便保存函数的返回值
result = add2num(100,98)

#因为result已经保存了add2num的返回值，所以接下来就可以使用了
print result
```
结果:
```py
    198
```

### 3.函数返回多个值？
```py
>>> def divid(a, b):
...     shang = a//b
...     yushu = a%b 
...     return shang, yushu
...
>>> sh, yu = divid(5, 2)
>>> sh
5
>>> yu
1
```