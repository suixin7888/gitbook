# 元组 {#元组}

Python的元组与列表类似，不同之处在于**元组的元素不能修改**。元组使用小括号，列表使用方括号。

```py
>>> aTuple = ('et',77,99.9)
>>> aTuple
('et',77,99.9)
```

#### &lt;1&gt;访问元组 {#访问元组}

![](/assets/29.png)

#### &lt;2&gt;修改元组 {#修改元组}

![](/assets/30.png)

说明：**python中不允许修改元组的数据，包括不能删除其中的元素。**

#### &lt;3&gt;元组的内置函数count, index {#元组的内置函数count-index}

index和count与字符串和列表中的用法相同

```py
>>> a = ('a', 'b', 'c', 'a', 'b')
>>> a.index('a', 1, 3) # 注意是左闭右开区间
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: tuple.index(x): x not in tuple
>>> a.index('a', 1, 4)
3
>>> a.count('b')
2
>>> a.count('d')
0
```



