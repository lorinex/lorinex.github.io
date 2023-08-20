---
categories:
  - 计算机基础
  - 数据结构和算法
  - 错题集
---
# 阿牛的EOF牛肉串

问题

一串由EOF组成的字符串，总个数为n，OO不能相连，问有多少种组合？

分析

最后一个可以是E或者F那么组数就是2*f(n-1)  如果最后一个是O，那么就要倒数第二个是E或者F才符合，所以是2f(n-2)最后得到递推式f(n)=2f(n-1)+2f(n-2)

```C++
#include<bits/stdc++.h>
using namespace std;
long long a[50];
int main()
{
    int n;
    a[1]=3;
    a[2]=8;
    for(int i=3;i<=40;i++)
    {
        a[i]=a[i-1]*2+2*a[i-2];
    }
    while(~scanf("%d",&n))
    {
        printf("%lld\n",a[n]);
    }
    return 0;
}
```


