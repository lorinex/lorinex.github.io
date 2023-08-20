---
categories:
  - 计算机基础
  - 数据结构和算法
  - 错题集
---
设计函数 void delcharfun(char *str,char ch)实现从字符串str中删除指定的字符ch。同一字母的大、小写按不同字符处理。

### 输入样例：

```in
abcABCabc#
b
```
```in
aaaabbb
a
```
### 输出样例：

```out
acABCac#
```
```out
bbb
```


```c
#include<stdio.h>

void delcharfun(char *str,char ch);

int main()
{
    char    ch,str[110];

    scanf("%s",str);    //读入字符串 
    getchar();            //读取回车符号 
    scanf("%c",&ch);    //读入字符 
    delcharfun(str,ch);    //删除 
    printf("%s\n",str);    //输出删除后结果 
    return 0;    
}

/* 请在这里填写答案 */
void delcharfun(char *str,char ch)
{
    char *m=str,*n;
    while(*m!='\0')
    {
        if(*m==ch)
        {
            n=m;
            while(*n!='\0')
            {
                *n=*(n+1);
                n++;
            }
        }
        if(*m!=ch)
        {
            m++;
        }
    }
}
```