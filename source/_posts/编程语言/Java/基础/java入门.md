---
categories:
  - 编程语言
  - Java
  - 基础
---
## 注释

单行 `//`

多行 `/*`

文档注释：注释内容可以被DK提供的工具javadoc所解析，生成一套以网页文件形式体现的该程序的说明文档，一般写在类

`-d documentcomment -author -version comment.java`

`javadoc -d 文件夹名 -xx -yy Demo3.java`

## 加法

当左右两边都是数值时，做加法运算

当左右两边有一方是字符串，做拼接运算

```java
System.out.println("200"+3);//2003
```