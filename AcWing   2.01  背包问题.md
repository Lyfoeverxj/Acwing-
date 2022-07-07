# AcWing   2.01  背包问题

分析：有限集合求极值问题

## 1.闫氏DP分析法

### 状态表示f(v,j)【选择问题中第一维为其i个物品 第二个为限制条件:类似重量、体积的限制。】

集合：所有只考虑前v个物品，且体积不超过j的选法的集合。

属性：MAX

f(n,v):只考虑前n个物品,且体积不超过v的选法的集合

### 状态计算 f(i,j)

f(i,j)划分为两个子集：所有不选第i个物品的方案、所有选择第i个物品的方案。

1. 所有不选第i个物品的方案

   从 1 ~（i-1）中选体积小于j的方案   **即f(i-1,j)**

2. 所有选择第i个物品的方案

   从1~i中选且必然包含i总体积小于v. 

   **由于第i个物品必然包括在内,只需要f( i - 1,j - v[i])最大      此时w最大=f(i-1,j-v[i])+w[i]。(v为最大体积)**

   即 ***1 ~ i，v <= j       ==》  1 ~ i - 1, v <= j - v[i]*** 

   ![image-20220706161842194](C:\Users\Lyanjie\AppData\Roaming\Typora\typora-user-images\image-20220706161842194.png)当j<v[i]是右侧部分（所有选择第i个物品的方案)是不存在的，在编写程序时要注意添加判断条件。
   
2.   程序优化

   f(i,j)=max( f(i - 1, j ),f( i-1 , j - v[i] ) + w[i])

   一维中始终未用到i，只用到了 i - 1

   二维用到 j 或者 j-v[i]   第二维始终用到的是小于等于自己的数

   ==》 优化为   for(j = v ; j >= v[i] ; j--)

```
#include<iostream>
#include<cstring>
using namespace std;
const int N = 1010;
int n, m;
int dp[N];
int v[N], w[N];

int main(){
    cin >> n >> m;
    for (int i = 1; i <= n; i++) 
    cin >> v[i] >> w[i];
    for (int i = 1; i <= n; i++)
        for (int j = m; j >= v[i]; j --)
            dp[j] = max(dp[j], dp[j - v[i]] + w[i]);
    cout << dp[m];
    return 0;
}
将数组定义在全局是不需要初始化，定义在main函数内时需要初始化。
```

## 2.动态规划

f(i,j)表示只看前i个物品，总体积是j的情况下，总体积最大是多少。

result=max(f(0,0~v))

f(i,j):

1.不选第i个物品   f(i,j)=f(i-1,j);

2.选第i个物品    f(i,j)=f( i-1 ,j-v[i])

result=max( f(i,j)=f(i-1,j)  ,  f( i-1 ,j-v[i]) )

```
#include<iostream>
#include<cstring>
using namespace std;
const int N = 1010;
int n, m;
int dp[N][N];
int v[N], w[N];

int main(){
    cin >> n >> m;
    for (int i = 1; i <= n; i++) cin >> v[i] >> w[i];

    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= m; j++)
        {
            dp[i][j] = dp[i - 1][j];
            if(j >= v[i]) dp[i][j] = max(dp[i][j], dp[i - 1][j - v[i]] + w[i]);
        }

    cout << dp[n][m];
    return 0;
}
```

