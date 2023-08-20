---
categories:
  - 计算机基础
  - 数据结构和算法
  - 错题集
---
给你一个n*n矩阵，按照顺序填入1到n*n的数，例如n=5，该矩阵如下

1 2 3 4 5
6 7 8 9 10
11 12 13 14 15
16 17 18 19 20
21 22 23 24 25
现在让你连接相邻两条边的中点，然后只保留他们围成封闭图形区域的数字，那么这个矩阵变为
3
7 8 9
11 12 13 14 15
17 18 19
23
现在你们涵哥让你求变化后的矩阵的所有元素的和为多少
此题若是构建数组，通过规律取数组元素求和的话，会报错——内存超限。
所以此题的解法是寻找规律，总结数学公式求解。

```c
#include<iostream>
using namespace std;
int main()
{
    long long sum;
    long long T,n,i,j,k;
    cin >> T;
    while (T--)
    {
        sum = 0;
        cin >> n;
        if (n % 2==1)
        {
            sum = (n + 1) / 2;
            for (i = 1; i <= n / 2; i++)
                sum += (n + 1) / 2 + n * i + ((2 * i + 1)*n + 1)*i;
            for (i = n / 2 + 1; i <n; i++)
                sum += (n + 1) / 2 + n * i + ((2 * i + 1)*n + 1)*(n - 1 - i);
            //sum += (n + 1) / 2 + n * (n - 1);
            cout << sum << endl;
        }
    }

    return 0;
}
```

```c
//内存超限
#include<iostream>
using namespace std;
const int MAX = 10001;
int m[MAX][MAX];
int main()
{
    int T, n, i, j, k;
    int sum1,sum2;
    int v;
    cin >> T;
    while (T--)
    {
        sum1 = 0;
        sum2 = 0;
        cin >> n;//n-1,n/2
        v = 1;
        for (i = 0; i < n; i++)
        {
            for (j = 0; j < n; j++)
            {
                m[i][j] = v;
                v++;
            }
        }
        for (i = 0; i <=n/2; i++)
            for (j = n / 2 - i, k = 1; k <= i * 2 + 1; j++,k++)
                sum1 += m[i][j];
        for (i = n - 1; i > n / 2; i--)
            for (j = n / 2 - (n-1-i), k = 1; k <= (n-1-i) * 2 + 1; j++, k++)
                sum2 += m[i][j];
        cout << sum1+sum2<<endl;
    }
    //cout << sum << endl;
    system("pause");
    return 0;
}
```
