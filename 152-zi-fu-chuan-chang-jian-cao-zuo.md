#### &lt;1&gt;find {#find}

检测 str 是否包含在 mystr中，如果是返回开始的索引值，否则返回-1

```py
mystr.find(str, start=0, end=len(mystr))
```

demo：

![](/assets/2.png)

#### &lt;2&gt;index {#index}

跟find\(\)方法一样，只不过如果str不在 mystr中会报一个异常.

```
mystr.index(str, start=0, end=len(mystr))
```

#### ![](/assets/3.png) {#count}

#### &lt;3&gt;count {#count}

返回 str在start和end之间 在 mystr里面出现的次数

```
mystr.count(str, start=0, end=len(mystr))
```

#### ![](/assets/4.png) {#replace}

#### &lt;4&gt;replace {#replace}

把 mystr 中的 str1 替换成 str2,如果 count 指定，则替换不超过 count 次.

```
mystr.replace(str1, str2,  mystr.count(str1))
```

#### ![](/assets/5.png) {#split}

#### &lt;5&gt;split {#split}

以 str 为分隔符切片 mystr，如果 maxsplit有指定值，则仅分隔 maxsplit 个子字符串

```
mystr.split(str=" ", 2)
```

#### ![](/assets/6.png) {#capitalize}

#### &lt;6&gt;capitalize {#capitalize}

把字符串的第一个字符大写

```
mystr.capitalize()
```

#### ![](/assets/7.png) {#title}

#### &lt;7&gt;title {#title}

把字符串的每个单词首字母大写

```py
>>> a = "hello itcast"
>>> a.title()
'Hello Itcast'
```

#### &lt;8&gt;startswith {#startswith}

检查字符串是否是以 obj 开头, 是则返回 True，否则返回 False

```
mystr.startswith(obj)
```

#### ![](/assets/8.png) {#endswith}

#### &lt;9&gt;endswith {#endswith}

检查字符串是否以obj结束，如果是返回True,否则返回 False.

```
mystr.endswith(obj)
```

#### ![](/assets/9.png) {#lower}

#### &lt;10&gt;lower {#lower}

转换 mystr 中所有大写字符为小写

```
mystr.lower()
```

#### ![](/assets/10.png) {#upper}

#### &lt;11&gt;upper {#upper}

转换 mystr 中的小写字母为大写

```
mystr.upper()
```

#### ![](/assets/11.png) {#ljust}

#### &lt;12&gt;ljust {#ljust}

返回一个原字符串左对齐,并使用空格填充至长度 width 的新字符串

```
mystr.ljust(width)
```

#### ![](/assets/12.png) {#rjust}

#### &lt;13&gt;rjust {#rjust}

返回一个原字符串右对齐,并使用空格填充至长度 width 的新字符串

```
mystr.rjust(width)
```

#### ![](/assets/13.png) {#center}

#### &lt;14&gt;center {#center}

返回一个原字符串居中,并使用空格填充至长度 width 的新字符串

```
mystr.center(width)
```

![](/assets/14.png)

#### &lt;15&gt;lstrip {#center}

删除 mystr 左边的空白字符

```
mystr.lstrip()
```

#### &lt;16&gt;rstrip {#rstrip}

删除 mystr 字符串末尾的空白字符

```
mystr.rstrip()
```

#### &lt;17&gt;strip {#strip}

删除mystr字符串两端的空白字符

```py
>>> a = "\n\t itcast \t\n"
>>> a.strip()
'itcast'
```

### &lt;18&gt;rfind {#rfind}

类似于 find\(\)函数，不过是从右边开始查找.

```
mystr.rfind(str, start=0,end=len(mystr) )
```

### &lt;19&gt;rindex {#rindex}

类似于 index\(\)，不过是从右边开始.

```
mystr.rindex( str, start=0,end=len(mystr))
```

### &lt;20&gt;partition {#partition}

把mystr以str分割成三部分,str前，str和str后

```
mystr.partition(str)
```

### &lt;21&gt;rpartition {#rpartition}

类似于 partition\(\)函数,不过是从右边开始.

```
mystr.rpartition(str)
```

### &lt;22&gt;splitlines {#splitlines}

按照行分隔，返回一个包含各行作为元素的列表

```
mystr.splitlines()
```

### &lt;23&gt;isalpha {#isalpha}

如果 mystr 所有字符都是字母 则返回 True,否则返回 False

```
mystr.isalpha()
```

### &lt;24&gt;isdigit {#isdigit}

如果 mystr 只包含数字则返回 True 否则返回 False.

```
mystr.isdigit()
```

### &lt;25&gt;isalnum {#isalnum}

如果 mystr 所有字符都是字母或数字则返回 True,否则返回 False

```
mystr.isalnum()
```

### &lt;26&gt;isspace {#isspace}

如果 mystr 中只包含空格，则返回 True，否则返回 False.

```
mystr.isspace()
```

### &lt;27&gt;join {#join}

mystr 中每个字符后面插入str,构造出一个新的字符串

```
mystr.join(str)
```



