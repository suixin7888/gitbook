## 字典常见 操作1 {#修改元素}

### &lt;1&gt;修改元素

字典的每个元素中的数据是可以修改的，只要通过key找到，即可修改

demo:

```py
info = {'name':'班长', 'id':100, 'sex':'f', 'address':'地球亚洲中国北京'}

newId = input('请输入新的学号')

info['id'] = int(newId)

print('修改之后的id为%d:'%info['id'])
```

### &lt;2&gt;添加元素

demo:访问不存在的元素

```
    info = {'name':'班长', 'sex':'f', 'address':'地球亚洲中国北京'}

    print('id为:%d'%info['id'])
```

结果:报错

如果在使用**变量名\['键'\] = 数据**时，这个“键”在字典中，不存在，那么就会新增这个元素

demo:添加新的元素

```py
info = {'name':'班长', 'sex':'f', 'address':'地球亚洲中国北京'}

# print('id为:%d'%info['id'])#程序会终端运行，因为访问了不存在的键

newId = input('请输入新的学号')

info['id'] = newId

print('添加之后的id为:%d'%info['id'])
```

结果:

```
    请输入新的学号188
    添加之后的id为: 188
```

### &lt;3&gt;删除元素

对字典进行删除操作，有一下几种：

* del
* clear\(\)

demo:del删除指定的元素

```py
info = {'name':'班长', 'sex':'f', 'address':'地球亚洲中国北京'}

print('删除前,%s'%info['name'])

del info['name']

print('删除后,%s'%info['name'])

注：删除后不能访问
```

demo:del删除整个字典

```py
info = {'name':'monitor', 'sex':'f', 'address':'China'}

print('删除前,%s'%info)

del info

print('删除后,%s'%info)
```

demo:clear清空整个字典

```py
info = {'name':'monitor', 'sex':'f', 'address':'China'}

print('清空前,%s'%info)

info.clear()

print('清空后,%s'%info)
```

## **字典 常见操作2**

### &lt;1&gt;len\(\) {#len}

测量字典中，键值对的个数

```py
>>> dict = {"name":'zhangsan','sex':'m'}
>>> len(dict)
2
>>>
```

### &lt;2&gt;keys {#keys}

返回一个包含字典所有KEY的列表

```py
>>> dict = {"name":'zhangsan','sex':'m'}
>>> dict.keys
['name','sex']
>>>
```

### &lt;3&gt;values {#values}

返回一个包含字典所有value的列表

```py
>>> dict = {"name":'zhangsan','sex':'m'}
>>> dict.values()
['zhangsan','m']
>>>
```

### &lt;4&gt;items {#items}

返回一个包含所有（键，值）元祖的列表

```py
>>> dict = {"name":'zhangsan','sex':'m'}
>>> dict.items()
([('name', 'zhangsan'), ('sex', 'm')])
```

### &lt;5&gt;has\_key {#haskey}

dict.has\_key\(key\)如果key在字典中，返回True，否则返回False

```py
>>> dict = {"name":'zhangsan','sex':'m'}
>>> dict.has_key('name')
True
>>> dict.has_key('phone')
False
```

## --遍历--

通过for ... in ...:的语法结构，我们可以遍历字符串、列表、元组、字典等数据结构。

**注意python语法的缩进**

### 字符串遍历

```py
>>> a_str = "hello itcast"
>>> for char in a_str:
...     print(char,end=' ')
...
h e l l o   i t c a s t
```

### 列表遍历

```py
>>> a_list = [1, 2, 3, 4, 5]
>>> for num in a_list:
...     print(num,end=' ')
...
1 2 3 4 5
```

### 元组遍历

```py
>>> a_turple = (1, 2, 3, 4, 5)
>>> for num in a_turple:
...     print(num,end=" ")
1 2 3 4 5
```

## --字典遍历-- {#字典遍历}

#### &lt;1&gt; 遍历字典的key（键） {#遍历字典的key（键）}

```py
>>> dict = {"name":'zhangsan','sex':'m'}
>>> for key in dict.keys():
...     print(key)
... 
name
sex
```

#### &lt;2&gt; 遍历字典的value（值） {#遍历字典的value（值）}

```py
>>> dict = {"name":'zhangsan','sex':'m'}
>>> for value in dict.values():
...     print(value)
... 
namezhangsan
m
```

#### &lt;3&gt; 遍历字典的项（元素） {#遍历字典的项（元素）}

```py
>>> dict = {"name":'zhangsan','sex':'m'}
>>> for item in dict.items():
...     print(item)
... 
('name','zhangsan')
('sex','m')
```

#### &lt;4&gt; 遍历字典的key-value（键值对） {#遍历字典的keyvalue（键值对）}

```py
>>> dict = {"name":'zhangsan','sex':'m'}
>>> for key,value in dict.items():
...     print("key=%s,value=%s"%(key,value))
... 
key=name,value=zhangsan
key=sex,value=m
```



