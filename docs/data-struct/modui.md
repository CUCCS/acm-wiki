# 莫队算法

!!! note "Copyright"
    本页面贡献者：[LyuLumos](https://github.com/LyuLumos)。
    本页面内容遵循 MIT 协议，转载请附上原文出处链接和本声明。

首先需要声明，莫队算法严格意义上不属于任何一个部分，但是由于它解决的问题与「主席树」、「树状数组」等数据结构具有极高的相似度，姑且置于数据结构单元。

## 普通莫队算法

### 概述
主要用于**离线**解决通常不带修改的区间查询问题，基于分块思想，复杂度为 $O(nlogn)$。


一般来说，如果可以在 $O(1)$ 内从 $[l, r]$ 的答案转移到 $[l-1, r]$、 $[l+1, r]$ 、 $[l, r-1]$ 、 $[l, r+1]$ 这四个与之紧邻的区间的答案，则可以考虑使用莫队。

### 题目原型

> 给定一个长度为 $n$ 序列，$m$ 次询问，每次询问区间 $[l,r]$ 有多少种不同的颜色。$n,m\leq 100000, 1\leq l \leq r \leq n$

暴力的代码显然是$O(nm)$的。用树状数组或者线段树会超时 ~~用主席树可以解决但是只有jdl会~~。

### 解决

莫队算法的核心在于，**离线**得到了一堆需要处理的区间后，合理安排这些区间计算的次序以得到一个较优的复杂度。其采用的策略是：

将序列分成 $\sqrt{n}$ 个长度为 $\sqrt{n}$ 的块，若左端点在同一个块内，则按右端点排序（以左端点所在块为第一关键字，右端点为第二关键字）。

这样的算法复杂度可以被[证明](https://oi-wiki.org/misc/mo-algo/#_5)是 $O(n\sqrt n)$ 的。

对于 $n<m$ 的情况，分块大小应选取 $\frac{n}{\sqrt m}$，此时算法复杂度为 $O(n\sqrt m)$。

### 卡常

莫队算法是很容易被卡常的（但是必要情况不用莫队AC想都不要想）。

#### 奇偶性排序

将一般的排序

```cpp
int cmp(query a, query b) {
    return belong[a.l] == belong[b.l] ? a.r < b.r : belong[a.l] < belong[b.l];
}
```

更改为

```cpp
int cmp(query a, query b) {
	return (belong[a.l] ^ belong[b.l]) ? belong[a.l] < belong[b.l] : ((belong[a.l] & 1) ? a.r < b.r : a.r > b.r);
}
```

一个合理的解释是

> 右指针跳完奇数块往回跳时在同一个方向能顺路把偶数块跳完，然后跳完这个偶数块又能顺带把下一个奇数块跳完。

#### 移动指针的常数压缩

只有少数顺序是较快的，例如

```cpp
while(l>q[i].l) add(a[--l]);
while(r<q[i].r) add(a[++r]);
while(l<q[i].l) del(a[l++]);
while(r>q[i].r) del(a[r--]);
```

详细分析见 [OI-wiki - 莫队算法 - 关于四个循环位置的讨论](https://oi-wiki.org/misc/mo-algo/)

#### 快速读/写

略。

#### O2优化

莫队在O2优化下速度非常可怕。但是鉴于正式比赛中具体评测环境不一致，非必要情况下不建议使用。

目前支持O2优化的平台有 洛谷、codeforces、CUC-ACM-OJ 等。

### 模板

```cpp

#include <bits/stdc++.h>
using namespace std;

int read()
{
    char x;
    while ((x = getchar()) > '9' || x < '0');
    int u = x - '0';
    while ((x = getchar()) <= '9' && x >= '0') u = (u << 3) + (u << 1) + x - '0';
    return u;
}
int buf[105];
inline void write(int i) {
    int p = 0;
    if (i == 0) p++;
    else while (i) {
        buf[p++] = i % 10;
        i /= 10;
    }
    for (int j = p - 1; j >= 0; j--) putchar('0' + buf[j]);
}

#define il inline
#define re register

int block, ans = 0, cnt[1000001];
int n, m, a[500010], Ans[500010];

struct node {
    int l, r, id;
}q[500010];

il bool cmp(node a, node b) {
    return (a.l / block) ^ (b.l / block) ? a.l < b.l : (((a.l / block) & 1) ? a.r<b.r : a.r>b.r);
}

il void add(int x) {
    if (!cnt[a[x]]) ans++;
    cnt[a[x]]++;
}

il void del(int x) {
    cnt[a[x]]--;
    if (!cnt[a[x]]) ans--;
}
int i;

int main() {
    n = read();
    for (i = 1; i <= n; ++i) a[i] = read();
    m = read();
    block = n / sqrt(m*2/3); // 这种大小随机情况下实测会更快
    for (i = 1; i <= m; ++i) {
        q[i].l = read();
        q[i].r = read();
        q[i].id = i;
    }
    sort(q + 1, q + m + 1, cmp);
    int l = 0, r = 0;
    for (i = 1; i <= m; ++i) {
        int ql = q[i].l, qr = q[i].r;
        while (l < ql) del(l++);
        while (l > ql) add(--l);
        while (r < qr) add(++r);
        while (r > qr) del(r--);
        Ans[q[i].id] = ans;
    }
    for (i = 1; i <= m; ++i)
        write(Ans[i]), printf("\n");
    return 0;
}
```

## 参考

- [OI-wiki - 普通莫队算法](https://oi-wiki.org/misc/mo-algo/)
