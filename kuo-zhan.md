#### 1.给程序传参数

```py
import sys
print(sys.argv)
```
执行结果
```
[root@test shell]# python test.py 
['test.py']
[root@test shell]# python test.py haha
['test.py', 'haha']
```

#### 2.列表推导式
1..基本方式
```py
>>> a = [x for x in range(4)]
>>> a
[0, 1, 2, 3]
>>> a = [x for x in range(3,4)]
>>> a
[3]
>>> a = [x for x in range(3,19,2)]
>>> a
[3, 5, 7, 9, 11, 13, 15, 17]
```
2..在循环的过程中使用if
```py
>>> a = [x for x in range(3,10) if x%2==0]
>>> a
[4, 6, 8]
>>> a = [x for x in range(3,10) if x%2!=0]
>>> a
[3, 5, 7, 9]
```
3.2个for循环
```py
>>> a = [(x,y) for x in range(1,3) for y in range(3)]
>>> a
[(1, 0), (1, 1), (1, 2), (2, 0), (2, 1), (2, 2)]
```
4.3个for循环
```py
>>> a = [(x,y,z) for x in range(1,3) for y in range(3) for z in range(4,6)]
>>> a
[(1, 0, 4), (1, 0, 5), (1, 1, 4), (1, 1, 5), (1, 2, 4), (1, 2, 5), (2, 0, 4), (2, 0, 5), (2, 1, 4), (2, 1, 5), (2, 2, 4), (2, 2, 5)]
```
#### 3.set、list、tuple
1.set是集合类型
```py
>>> a = set()
>>> type(a)
<class 'set'>
>>> 
>>> b = [1,2,3,4,5,6,7]
>>> b
[1, 2, 3, 4, 5, 6, 7]
>>> 
>>> c = set(b)
>>> 
>>> type(c)
<class 'set'>
>>> 
>>> c
{1, 2, 3, 4, 5, 6, 7}
>>> 
>>> d = list(c)
>>> 
>>> d
[1, 2, 3, 4, 5, 6, 7]
>>> 
```
2.set、list、tuple之间相互转换
```py
>>> d = list(c)
>>> 
>>> d
[1, 2, 3, 4, 5, 6, 7]
>>> 
>>> e = tuple(d)
>>> 
>>> e
(1, 2, 3, 4, 5, 6, 7)
>>> 
>>> f = list(e)
>>> f
[1, 2, 3, 4, 5, 6, 7]
>>> 
>>> g = set(e)
>>> g
{1, 2, 3, 4, 5, 6, 7}
```