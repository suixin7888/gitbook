## &lt;1&gt;列表的格式 {#列表的格式}

变量A的类型为列表

```py
namesList = ['xiaoWang','xiaoZhang','xiaoHua']
```

比C语言的数组强大的地方在于列表中的元素可以是不同类型的

```py
testList = [1, 'a']
```

## &lt;2&gt;打印列表 {#打印列表}

demo:

```py
namesList = ['xiaoWang','xiaoZhang','xiaoHua']
print(namesList[0])
print(namesList[1])
print(namesList[2])
```

结果：

```
xiaoWang
xiaoZhang
xiaoHua
```

## &lt;3&gt;列表的循环遍历

## 1. 使用for循环 {#1-使用for循环}

为了更有效率的输出列表的每个数据，可以使用循环来完成

demo:

```py
namesList = ['xiaoWang','xiaoZhang','xiaoHua']
for name in namesList:
    print(name)
```

结果:

```
    xiaoWang
    xiaoZhang
    xiaoHua
```

## 2. 使用while循环 {#2-使用while循环}

为了更有效率的输出列表的每个数据，可以使用循环来完成

demo:

```py
namesList = ['xiaoWang','xiaoZhang','xiaoHua']

length = len(namesList)

i = 0

while i<length:
    print(namesList[i])
    i+=1
```

结果:

```
    xiaoWang
    xiaoZhang
    xiaoHua
```



