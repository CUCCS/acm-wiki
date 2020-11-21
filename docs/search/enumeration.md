!!! note "Copyright"
    本页面贡献者：[lengwind](https://github.com/lengwind)。
    本页面内容遵循 MIT 协议，转载请附上原文出处链接和本声明。

## 一.枚举排列

**什么是排列 ?**

长度为$n$的排列指$1\sim n$不重不漏的恰好都出现一次

如： [1 2 5 3 4] ， [5 4 3 2 1] 都是排列

而     [1 1 2 3] （出现重复的元素），[1 3 4] (缺少元素2)不是排列

**什么是排列的顺序 ?**

排列是有顺序的，比如长度为3的排列从小到大依次为

[1 2 3]  [1 3 2]  [2 1 3]  [2 3 1]  [3 1 2]  [3 2 1] 

**定理： 长度为n的排列一共有$n!$种**



顾名思义就是从小到大去枚举排列，一般要求排列的长度不能过大（$n\leq11$）
因为$11!< 1e8, 12! > 1e8$

枚举排列的方法：

**1.next_permutation 与 prev_permutation**

**包含头文件 `#include<algorithm>`**

用法：
`next_permutation(迭代器1，迭代器2)`（类似`sort`，对[迭代器1，迭代器2）进行操作 ），
如`next_permutation(a,a+n)` （从$[0,n)$）或者 `next_permutation(a+1,a+1+n)`（ $[1,n+1)$ ）

**2.DFS**

```CPP
#include<iostream>
#include<algorithm>
using namespace std;
const int N = 20;
bool used[N];
int a[N];
int n;
void DFS(int u)
{
    if(u == n)
    {
        for(int i = 0; i < n; ++ i) cout << a[i] + 1 << ' ';
        cout <<endl;
        return;
    }
    for(int i = 0; i < n; ++ i)
    {
        if(!used[i])
        {
            used[i] = 1, a[u] = i;
            DFS(u+1);
            used[i] = 0, a[u] = 0;
        }
    }
}
int main()
{
   cin >> n;
   DFS(0);
}
```



### next_permutation


```CPP
#include<iostream>
#include<algorithm>
using namespace std;
int main()
{
    int a[] = {1,2,3};
    int n = 3;
    do
    {
        for(int i = 0; i < 3; ++ i) cout << a[i] << ' ';
        cout <<endl;
    }while(next_permutation(a,a+n));
}
```



### prev_permutation


```cpp
#include<iostream>
#include<algorithm>
using namespace std;
int main()
{
    int a[] = {3,2,1};
    int n = 3;
    do
    {
        for(int i = 0; i < 3; ++ i) cout << a[i] << ' ';
        cout <<endl;
    }while(prev_permutation(a,a+n));
}
```



## 二.枚举子集

### **什么是子集 ?**

子集是一个数学概念：如果集合A的任意一个元素都是集合B的元素，那么集合A称为集合B的子集。

符号语言：$若\forall a \in A, 均有 a \in B, 则A\subseteq B$。



### **大小为n的集合有多少个子集?**

大小为$n$的集合有$2^n$个子集（其中包括一个空集，一个自身）

### 枚举子集方法:

#### 1.位运算

对于集合中第$i$个元素（$i$从0开始编号）0表示选这个元素，1表示不选这个元素

那么对于一个大小为4的集合来说 

3    2     1     0    （下标）

0    1     0     1    （枚举值的二进制0101选第0位和第2位，也就是十进制数5）



得到启发，枚举大小为4的子集只需要从$0000_{(2)}\sim 1111_{(2)}$都枚举一遍即可，对应十进制即$0_{(10)}\sim 15_{(10)}$。

**规律：枚举大小为$n$的集合只需要枚举$0\sim2^n-1$（一共$2^n$种情况）**

```CPP
#include<iostream>
#include<algorithm>
using namespace std;
int main()
{
    int n = 4; ///集合大小5
    for(int i = 0; i < (1<<n); ++ i) ///从0 ~ 2^n - 1枚举
    {
        ///每一个i的二进制对应一种情况
        for(int j = 0; j < n; ++ j)
        {
            ///如果i的二进制的第j位为1 说明选这个数
            if(i >> j & 1)
            {
                cout << j << ' ';
            }
        }
        cout <<endl;
    }
}
```

#### 2.DFS

```CPP
#include<bits/stdc++.h>
using namespace std;
const int N = 20;
bool used[N];
int a[N];
int n;
vector<int>v;
void DFS(int u)
{
    if(u == n)
    {
        for(int i : v) cout << i + 1 << ' ';
        cout << endl;
        return;
    }
    ///不选第u个元素
    DFS(u + 1);

    ///选第u个元素
    v.push_back(u);
    DFS(u + 1);
    v.pop_back();
}
int main()
{
   cin >> n;
   DFS(0);
}
```



