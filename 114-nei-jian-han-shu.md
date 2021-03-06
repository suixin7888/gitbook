## 内建函数

Build-in Function,启动python解释器，输入dir(__builtins__), 可以看到很多python解释器启动后默认加载的属性和函数，这些函数称之为内建函数， 这些函数因为在编程时使用较多，cpython解释器用c语言实现了这些函数，启动解释器 时默认加载。

这些函数数量众多，不宜记忆，开发时不是都用到的，待用到时再help(function), 查看如何使用，或结合百度查询即可，在这里介绍些常用的内建函数。

#### range
```py
range(stop) -> list of integers
range(start, stop[, step]) -> list of integers
```
* start:计数从start开始。默认是从0开始。例如range（5）等价于range（0， 5）;
* stop:到stop结束，但不包括stop.例如：range（0， 5） 是[0, 1, 2, 3, 4]没有5
* step:每次跳跃的间距，默认为1。例如：range（0， 5） 等价于 range(0, 5, 1)

python2中range返回列表，python3中range返回一个迭代值。如果想得到列表,可通过list函数
```py
a = range(5)
list(a)
```
创建列表的另外一种方法

```py
In [21]: testList = [x+2 for x in range(5)]

In [22]: testList
Out[22]: [2, 3, 4, 5, 6]
```
#### map函数

map函数会根据提供的函数对指定序列做映射
```py
map(...)
    map(function, sequence[, sequence, ...]) -> list
```  
* function:是一个函数
* sequence:是一个或多个序列,取决于function需要几个参数
* 返回值是一个list

参数序列中的每一个元素分别调用function函数，返回包含每次function函数返回值的list。
```py
#函数需要一个参数
map(lambda x: x*x, [1, 2, 3])
#结果为:[1, 4, 9]

#函数需要两个参数
map(lambda x, y: x+y, [1, 2, 3], [4, 5, 6])
#结果为:[5, 7, 9]


def f1( x, y ):  
    return (x,y)

l1 = [ 0, 1, 2, 3, 4, 5, 6 ]  
l2 = [ 'Sun', 'M', 'T', 'W', 'T', 'F', 'S' ]
l3 = map( f1, l1, l2 ) 
print(list(l3))
#结果为:[(0, 'Sun'), (1, 'M'), (2, 'T'), (3, 'W'), (4, 'T'), (5, 'F'), (6, 'S')]
```
#### filter函数

filter函数会对指定序列执行过滤操作
```py
filter(...)
    filter(function or None, sequence) -> list, tuple, or string

    Return those items of sequence for which function(item) is true.  If
    function is None, return the items that are true.  If sequence is a tuple
    or string, return the same type, else return a list.
```
* function:接受一个参数，返回布尔值True或False
* sequence:序列可以是str，tuple，list

filter函数会对序列参数sequence中的每个元素调用function函数，最后返回的结果包含调用结果为True的元素。

返回值的类型和参数sequence的类型相同
```py
filter(lambda x: x%2, [1, 2, 3, 4])
[1, 3]

filter(None, "she")
'she'
```
#### reduce函数

reduce函数，reduce函数会对参数序列中元素进行累积
```
reduce(...)
    reduce(function, sequence[, initial]) -> value

    Apply a function of two arguments cumulatively to the items of a sequence,
    from left to right, so as to reduce the sequence to a single value.
    For example, reduce(lambda x, y: x+y, [1, 2, 3, 4, 5]) calculates
    ((((1+2)+3)+4)+5).  If initial is present, it is placed before the items
    of the sequence in the calculation, and serves as a default when the
    sequence is empty.
 ```   
* function:该函数有两个参数
* sequence:序列可以是str，tuple，list
* initial:固定初始值

reduce依次从sequence中取一个元素，和上一次调用function的结果做参数再次调用function。 第一次调用function时，如果提供initial参数，会以sequence中的第一个元素和initial 作为参数调用function，否则会以序列sequence中的前两个元素做参数调用function。 注意function函数不能为None。
```py
reduce(lambda x, y: x+y, [1,2,3,4])
10

reduce(lambda x, y: x+y, [1,2,3,4], 5)
15

reduce(lambda x, y: x+y, ['aa', 'bb', 'cc'], 'dd')
'ddaabbcc'
```
>在Python3里,reduce函数已经被从全局名字空间里移除了, 它现在被放置在fucntools模块里用的话要先引入： from functools import reduce

#### sorted函数
```
sorted(...)
    sorted(iterable, cmp=None, key=None, reverse=False) --> new sorted list
```
```py
>>> sorted([1,4,2,6,3,5])
[1, 2, 3, 4, 5, 6]
>>> sorted([1,4,2,6,3,5],reverse=1)
[6, 5, 4, 3, 2, 1]
>>> sorted(['dd','aa','cc','bb'])
['aa', 'bb', 'cc', 'dd']
>>> sorted(['dd','aa','cc','bb'],reverse=1)
['dd', 'cc', 'bb', 'aa']
>>> 
```
