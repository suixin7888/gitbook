## 调试

### pdb

pdb是基于命令行的调试工具，非常类似gnu的gdb（调试c/c++）。

|命令	|简写命令	|作用|
|-----|----------|-----|
|break|	b	|设置断点|
|continue	|c	|继续执行程序|
|list	|l	|查看当前行的代码段|
|step	|s	|进入函数|
|return	|r	|执行代码直到从当前函数返回|
|quit	|q	|中止并退出|
|next	|n|	执行下一行|
|print|	p	|打印变量的值|
|help	|h	|帮助|
|args	|a	|查看传入参数|
|       |回车	|重复上一条命令|
|break|	b	|显示所有断点|
|break lineno	|b lineno	|在指定行设置断点|
|break file:lineno|	b file:lineno	|在指定文件的行设置断点|
|clear num|		|删除指定断点|
|bt	|	|查看函数调用栈帧|

#### 执行时调试

程序启动，停止在第一行等待单步调试。
```
python -m pdb some.py
```
#### 交互调试

进入python或ipython解释器
```py
import pdb
pdb.run('testfun(args)') #此时会打开pdb调试，注意：先使用s跳转到这个testfun函数中，然后就可以使用l看到代码了
```
#### 程序里埋点

当程序执行到pdb.set_trace() 位置时停下来调试
```py
代码上下文
...

import pdb 
pdb.set_trace() 

...
```
### 日志调试

#### print大法好

使用pdb调试的5个demo

#### demo 1
```py
import pdb 
a = "aaa"
pdb.set_trace()
b = "bbb"
c = "ccc"
final = a + b + c 
print final

#调试方法

# 《1 显示代码》
# l---->能够显示当前调试过程中的代码，其实l表示list列出的意思
  #如下，途中，-> 指向的地方表示要将要执行的位置
  # 2      a = "aaa"
  # 3      pdb.set_trace()
  # 4      b = "bbb"
  # 5      c = "ccc"
  # 6      pdb.set_trace()
  # 7  ->    final = a + b + c
  # 8      print final

# 《2 执行下一行代码》
# n---->能够向下执行一行代码，然后停止运行等待继续调试 n表示next的意思

# 《3 查看变量的值》
# p---->能够查看变量的值，p表示prit打印输出的意思
    #例如：
    # p name 表示查看变量name的值
```

#### demo 2
```py
import pdb 
a = "aaa"
pdb.set_trace()
b = "bbb"
c = "ccc"
pdb.set_trace()
final = a + b + c 
print final

# 《4 将程序继续运行》
# c----->让程序继续向下执行，与n的区别是n只会执行下面的一行代码，而c会像python xxxx.py一样 继续执行不会停止；c表示continue的意思

# 《5 set_trace()》
# 如果程序中有多个set_trace()，那么能够让程序在使用c的时候停留在下一个set_trace()位置处
```
#### demo 3
```py
#coding=utf-8
import pdb 

def combine(s1,s2):
    s3 = s1 + s2 + s1
    s3 = '"' + s3 +'"'
    return s3

a = "aaa"
pdb.set_trace() 
b = "bbb"
c = "ccc"
final = combine(a,b)
print final

# 《6 设置断点》
# b---->设置断点，即当使用c的时候，c可以在遇到set_trace()的时候停止，也可以在遇到标记有断点的地方停止；b表示break的意思
    #例如：
    #b 11 在第11行设置断点，注意这个11可以使用l来得到
    # (Pdb) l
    #   4          s3 = s1 + s2 + s1
    #   5          s3 = '"' + s3 +'"'
    #   6          return s3
    #   7      a = "aaa"
    #   8      pdb.set_trace()
    #   9  ->    b = "bbb"
    #  10      c = "ccc"
    #  11      final = combine(a,b)
    #  12      print final
    # [EOF]
    # (Pdb) b 11
    # Breakpoint 1 at /Users/wangmingdong/Desktop/test3.py:11
    # (Pdb) c
    # > /Users/wangmingdong/Desktop/test3.py(11)<module>()
    # -> final = combine(a,b)
    # (Pdb) l
    #   6          return s3
    #   7      a = "aaa"
    #   8      pdb.set_trace()
    #   9      b = "bbb"
    #  10      c = "ccc"
    #  11 B->    final = combine(a,b)
    #  12      print final

# 《7 进入函数继续调试》
# s---->进入函数里面继续调试，如果使用n表示把一个函数的调用当做一条语句执行过去，而使用s的话，会进入到这个函数 并且停止
    #例如
    # (Pdb) l
    #   6          return s3
    #   7      a = "aaa"
    #   8      pdb.set_trace()
    #   9      b = "bbb"
    #  10      c = "ccc"
    #  11 B->    final = combine(a,b)
    #  12      print final
    # [EOF]
    # (Pdb) s
    # --Call--
    # > /Users/wangmingdong/Desktop/test3.py(3)combine()
    # -> def combine(s1,s2):
    # (Pdb) l
    #   1      import pdb
    #   2
    #   3  ->    def combine(s1,s2):
    #   4          s3 = s1 + s2 + s1
    #   5          s3 = '"' + s3 +'"'
    #   6          return s3
    #   7      a = "aaa"
    #   8      pdb.set_trace()
    #   9      b = "bbb"
    #  10      c = "ccc"
    #  11 B    final = combine(a,b)
    # (Pdb)

# 《8 查看传递到函数中的变量》
# a---->调用一个函数时，可以查看传递到这个函数中的所有的参数；a表示arg的意思
    #例如：
    # (Pdb) l
    #   1      #coding=utf-8
    #   2      import pdb
    #   3
    #   4  ->    def combine(s1,s2):
    #   5          s3 = s1 + s2 + s1
    #   6          s3 = '"' + s3 +'"'
    #   7          return s3
    #   8
    #   9      a = "aaa"
    #  10      pdb.set_trace()
    #  11      b = "bbb"
    # (Pdb) a
    # s1 = aaa
    # s2 = bbb

# 《9 执行到函数的最后一步》
# r----->如果在函数中不想一步步的调试了，只是想到这个函数的最后一条语句那个位置，比如return语句，那么就可以使用r；r表示return的意思
```
#### demo 4
```py
In [1]: def pdb_test(arg):
   ...:     for i in range(arg):
   ...:         print(i)
   ...:     return arg
   ...:

In [2]: #在python交互模式中，如果想要调试这个函数，那么可以

In [3]: #采用，pdb.run的方式，如下：

In [4]: import pdb

In [5]: pdb.run("pdb_test(10)")
> <string>(1)<module>()
(Pdb) s
--Call--
> <ipython-input-1-ef4d08b8cc81>(1)pdb_test()
-> def pdb_test(arg):
(Pdb) l
  1  ->    def pdb_test(arg):
  2          for i in range(arg):
  3              print(i)
  4          return arg
[EOF]
(Pdb) n
> <ipython-input-1-ef4d08b8cc81>(2)pdb_test()
-> for i in range(arg):
(Pdb) l
  1      def pdb_test(arg):
  2  ->        for i in range(arg):
  3              print(i)
  4          return arg
[EOF]
(Pdb) n
> <ipython-input-1-ef4d08b8cc81>(3)pdb_test()
-> print(i)
(Pdb)
0
> <ipython-input-1-ef4d08b8cc81>(2)pdb_test()
-> for i in range(arg):
(Pdb)
> <ipython-input-1-ef4d08b8cc81>(3)pdb_test()
-> print(i)
(Pdb)
1
> <ipython-input-1-ef4d08b8cc81>(2)pdb_test()
-> for i in range(arg):
(Pdb)
```
#### demo 5 运行过程中使用pdb修改变量的值
```py
In [7]: pdb.run("pdb_test(1)")
> <string>(1)<module>()
(Pdb) s
--Call--
> <ipython-input-1-ef4d08b8cc81>(1)pdb_test()
-> def pdb_test(arg):
(Pdb) a
arg = 1
(Pdb) l
  1  ->    def pdb_test(arg):
  2          for i in range(arg):
  3              print(i)
  4          return arg
[EOF]
(Pdb) !arg = 100  #!!!这里是修改变量的方法
(Pdb) n
> <ipython-input-1-ef4d08b8cc81>(2)pdb_test()
-> for i in range(arg):
(Pdb) l
  1      def pdb_test(arg):
  2  ->        for i in range(arg):
  3              print(i)
  4          return arg
[EOF]
(Pdb) p arg
100
(Pdb)
```
#### 练一练:请使用所学的pdb调试技巧对其进行调试出bug
```py
#coding=utf-8
import pdb 

def add3Nums(a1,a2,a3):
    result = a1+a2+a3
    return result


def get3NumsAvarage(s1,s2):
    s3 = s1 + s2 + s1
    result = 0
    result = add3Nums(s1,s2,s3)/3

if __name__ == '__main__':

    a = 11
    # pdb.set_trace() 
    b = 12
    final = get3NumsAvarage(a,b)
    print final
```

pdb 调试有个明显的缺陷就是对于多线程，远程调试等支持得不够好，同时没有较为直观的界面显示，不太适合大型的 python 项目。而在较大的 python 项目中，这些调试需求比较常见，因此需要使用更为高级的调试工具。