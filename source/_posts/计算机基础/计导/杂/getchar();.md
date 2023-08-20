---
categories:
  - 计算机基础
  - 计导
  - 杂
---
将存于缓冲区的回车符读入
（吃掉这个字符

   
```c
#include<stdio.h>
main()
{
 int i;
 float f;
 char c;
 printf("input i,f\n");
 scanf("%d,%f", &i,&f );
 printf("input c\n");
 scanf("%c", &c );
 printf("the result is:\n");
 printf("i=%d,f=%f,c=%c",i,f,c);
 system("pause");
 return 0;
}
```
问题：输出时回车符被读入c
结果如下：  
input i,f
10,3.14
input c
the result is:
i=10,f=3.140000,c=
请按任意键继续 . . .

解决办法：
用getchar（）；吃掉回车符
```c
main()
{
 int i;
 float f;
 char c;
 printf("input i,f\n");
 scanf("%d,%f", &i,&f );
 getchar(); /*将存于缓冲区的回车符读入*/
 printf("input c\n");
 scanf("%c", &c );
 printf("the result is:结果是\n");
 printf("i=%d,f=%f,c=%c",i,f,c);
 system("pause");
}

