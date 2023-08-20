---
categories:
  - 计算机基础
  - 数据结构和算法
  - 错题集
---
设计函数 char *locatesubstr(char *str1,char *str2)，查找str2指向的字符串在str1指向的字符串中首次出现的位置，返回指向该位置的指针。若str2指向的字符串不包含在str1指向的字符串中，则返回空指针NULL。
注意这里必须使用指针而不是数组下标来访问字符串。
输入与输出要求：输入两个长度不超过500的非空字符串str1和str2，字符串中可能出现空格，以换行符结束。输出str1中返回指针后的所有字符；否则输出“NULL!”。
程序运行效果：

Sample 1:
didjfsd dabcxxxxxx↙
abc↙
abcxxxxxx
Sample 2:
aaaaabcaaa↙
xxx↙
NULL!
————————————————
```c
#include <stdio.h>

char *locatesubstr(char *str1,char *str2);
int main()
{
    char str1[505],str2[505];
    char *p;
    gets(str1);
    gets(str2);
    p=locatesubstr(str1,str2);

    if(p==NULL)    printf("NULL!\n");
    else    puts(p);

    return 0;
}

/* 请在这里填写答案 */
char *locatesubstr(char *str1, char *str2)
{
	int i;
		while (*str1 != '\0')
		{
			for (i = 0; *(str1 + i) == *(str2 + i); i++)
			{
				if (*(str2 + i + 1) == '\0')
					return (char*)str1;
			}
			str1++;
		}
		return NULL;
	
}
```
