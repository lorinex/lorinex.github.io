---
categories:
  - 编程语言
  - Python
  - 3 控制流
---
for i in range(起始值，终止值，步长)

eg.for i in range(1,5)
默认步长为1
相当于C语言中for(i=1;i<5;i++)

range 函数用于创建一个整数列表

```python
str_1 = 'H U A W E I'

for i in str_1:
    print (i) # 输出后自动换行
    
for j in str_1:
    print (j, end='') # 输出后不换行继续输出下一个字符
```
![[Pasted image 20211110084943.png]]

