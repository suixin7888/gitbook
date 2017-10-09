## 1. break {#1-break}

### &lt;1&gt; for循环 {#for循环}

* 普通的循环示例如下：

  ```py
  name = 'dongGe'

  for x in name:
      print('----')
      print(x)
  ```

* 带有break的循环示例如下:

  ```

    name = 
  'dongGe'
  for
   x 
  in
   name:
        print(
  '----'
  )
      
  if
   x == 
  'g'
  : 
          
  break

        print(x)

  ```

### &lt;2&gt; while循环 {#while循环}

* 普通的循环示例如下：

  ```

    i = 
  0
  while
   i
  <
  10
  :
        i = i+
  1

        print(
  '----'
  )
        print(i)

  ```

* 带有break的循环示例如下:

  ```

    i = 
  0
  while
   i
  <
  10
  :
        i = i+
  1

        print(
  '----'
  )
      
  if
   i==
  5
  :
          
  break

        print(i)

  ```

### **小总结:** {#小总结}

* break的作用：用来结束整个循环

## 2. continue {#2-continue}

### &lt;1&gt; for循环 {#for循环}

* 带有continue的循环示例如下:

  ```

    name = 
  'dongGe'
  for
   x 
  in
   name:
        print(
  '----'
  )
      
  if
   x == 
  'g'
  : 
          
  continue

        print(x)

  ```

### &lt;2&gt; while循环 {#while循环}

* 带有continue的循环示例如下:

  ```

    i = 
  0
  while
   i
  <
  10
  :
        i = i+
  1

        print(
  '----'
  )
      
  if
   i==
  5
  :
          
  continue

        print(i)

  ```

* **小总结:**
  * continue的作用：用来结束本次循环，紧接着执行下一次的循环

## 3. 注意点 {#3-注意点}

* break/continue只能用在循环中，除此以外不能单独使用

* break/continue在嵌套循环中，只对最近的一层循环起作用



