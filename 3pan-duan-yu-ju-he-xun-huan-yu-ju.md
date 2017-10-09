# &lt;1&gt;if的使用格式 {#ifelse}

```py
   if 性别为男性:
       输出男性的特征
       ...
   elif 性别为女性:
       输出女性的特征
       ...
   else:
       第三种性别的特征
       ...
```

## &lt;2&gt;while循环的格式 {#while循环的格式}

```py
while条件:
        条件满足时，做的事情
1

        条件满足时，做的事情
2

        条件满足时，做的事情
3

        ...(省略)...
```

demo： 计算1~100的累积和（包含1和100）

参考代码如下:

```py
#encoding=utf-8

i = 1
sum = 0
while i<=100:
    sum = sum + i
    i += 1

print("1~100的累积和为:%d"%sum)
```

demo：while嵌套应用二：九九乘法表

参考代码：

```py
i = 1
while i<=9:
    j=1
    while j<=i:
        print("%d*%d=%-2d "%(j,i,i*j),end='')
        j+=1
    print('\n')
    i+=1
```

## &lt;3&gt;for循环的格式 {#for循环的格式}

```
for
 临时变量 
in
 列表或者字符串等:
        循环满足条件时执行的代码
    
else
:
        循环不满足条件时执行的代码
```



