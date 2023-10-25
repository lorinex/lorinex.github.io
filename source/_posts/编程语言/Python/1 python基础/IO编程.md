---
title: IO编程
date: 2023-10-23 13:09:31
tags:
---
# IO编程
## 文件读写 - with语句
无异常捕获
```python
f = open('/Users/michael/test.txt', 'r')
text = f.read()
f.close()
```
使用try捕获异常
```python
try:
    f = open('/path/to/file', 'r')
    print(f.read())
    f.close()
except:
    pass
```
使用with语句
```python
![](https://picture2023-1309715649.cos.ap-beijing.myqcloud.com/img/20231023131055.png)
```
## 文本编码
文本文件默认是UTF-8编码，要读取非UTF-8编码的文本文件，需要给open()函数传入encoding参数
遇到有些编码不规范的文件，可能会遇到UnicodeDecodeError，因为在文本文件中可能夹杂了一些非法编码的字符。遇到这种情况，open()函数还接收一个errors参数，表示如果遇到编码错误后如何处理。最简单的方式是直接忽略：
## 写文件
写文件是调用open()函数传入标识符'w'或者'wb'表示写文本文件或写二进制文件
```python
>>> f = open('/Users/michael/test.txt', 'w')
>>> f.write('Hello, world!')
>>> f.close()
```
写文件时操作系统往往不会立刻把数据写入磁盘，而是放到内存缓存起来，空闲的时候再慢慢写入。只有调用close()方法时，操作系统才保证把没有写入的数据全部写入磁盘。忘记调用close()的后果是数据可能只写了一部分到磁盘。
```python
with open('/Users/michael/test.txt', 'w') as f:
    f.write('Hello, world!')
```
用with语句可以无需close就能保证数据写入磁盘。
以'w'模式写入文件时，如果文件已存在，会直接覆盖（相当于删掉后新写入一个文件）。如果我们希望追加到文件末尾怎么办？可以传入'a'以追加（append）模式写入
## 二进制文件
要读取二进制文件，比如图片、视频等等，用'rb'模式打开文件即可
```python
f = open('/Users/michael/test.jpg', 'rb')
```
## 序列化
### pickle
把变量从内存中变成可存储或传输的过程称之为序列化，在Python中叫pickling
```python
>>> import pickle
>>> d = dict(name='Bob', age=20, score=88)
>>> pickle.dumps(d)
b'\x80\x03}q\x00(X\x03\x00\x00\x00ageq\x01K\x14X\x05\x00\x00\x00scoreq\x02KXX\x04\x00\x00\x00nameq\x03X\x03\x00\x00\x00Bobq\x04u.'
```
pickle.dumps()方法把任意对象序列化成一个bytes，然后，就可以把这个bytes写入文件。或者用另一个方法pickle.dump()直接把对象序列化后写入一个file-like Object：
```python
>>> f = open('dump.txt', 'wb')
>>> pickle.dump(d, f)
>>> f.close()
```
反过来，把变量内容从序列化的对象重新读到内存里称之为反序列化，即unpickling
```python
>>> f = open('dump.txt', 'rb')
>>> d = pickle.load(f)
>>> f.close()
>>> d
{'age': 20, 'score': 88, 'name': 'Bob'}
```
### JSON
Python内置的json模块提供了非常完善的Python对象到JSON格式的转换
```python
>>> import json
>>> d = dict(name='Bob', age=20, score=88)
>>> json.dumps(d)
'{"age": 20, "score": 88, "name": "Bob"}'
```
要把JSON反序列化为Python对象，用loads()或者对应的load()方法，前者把JSON的字符串反序列化，后者从file-like Object中读取字符串并反序列化
```python
>>> json_str = '{"age": 20, "score": 88, "name": "Bob"}'
>>> json.loads(json_str)
{'age': 20, 'score': 88, 'name': 'Bob'}
```
