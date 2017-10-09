## &lt;2&gt;字典 {#软件开发中的字典}

变量info为字典类型：

```py
    info = {'name':'班长', 'id':100, 'sex':'f', 'address':'地球亚洲中国北京'}
```

说明：

* 字典和列表一样，也能够存储多个数据
* 列表中找某个元素时，是根据下标进行的
* 字典中找某个元素时，是根据'名字'（就是冒号:前面的那个值，例如上面代码中的'name'、'id'、'sex'）
* 字典的每个元素由2部分组成，键:值。例如 'name':'班长' ,'name'为键，'班长'为值

## &lt;2&gt;根据键访问值 {#根据键访问值}

```py
info = {'name':'班长', 'id':100, 'sex':'f', 'address':'地球亚洲中国北京'}

print(info['name'])
print(info['address'])
```

结果:

```
    班长
    地球亚洲中国北京

```

若访问不存在的键，则会报错：

```py
>>> info['age']
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'age'
```

在我们不确定字典中是否存在某个键而又想获取其值时，可以使用get方法，还可以设置默认值：

```py
>>> age = info.get('age')
>>> age #'age'键不存在，所以age为None
>>> type(age)
<type 'NoneType'>
>>> age = info.get('age', 18) # 若info中不存在'age'这个键，就返回默认值18
>>> age
18
```



