---
title: "'str' object does not support item assignment"
date: 2023-09-18
tags:
  - python
---

字符串是不可变的对象
不能用下标赋值的方法去改变字符串
**改变字符串的正确方法：**

1.  将字符串按字符生成一个list；
2.  转化为str。

```python
str1=('12345')
list=[]
for i in str1:
	list.append(i)
str1="".join(list1)
print(str1)
```