!!! note "Copyright"
    本页面贡献者：[zrz](https://github.com/BehindShadow)。
    本页面内容遵循 MIT 协议，转载请附上原文出处链接和本声明。

## 定义

图中所有最短路径的最大值即为「直径」，可以用两次 DFS 或者树形 DP 的方法在$O(n)$时间求出树的直径。

## 树上dfs

复杂度:$O(n)$

首先对任意一个结点做 DFS 求出最远的结点，然后以这个结点为根结点再做 DFS 到达另一个最远结点。第一次 DFS 到达的结点可以证明一定是这个图的直径的一端，第二次 DFS 就会达到另一端。

说明：对于树的直径问题，如果要求数直径的左右端点和树直径的中点用树dp不易求，解法是两次dfs或者两次bfs


**注意：不能在存在负边的情况下求直径:
大神解释：因为在第一次遍历后, 有的边变为了 -1, 然后你第一次bfs或dfs会因为选取的起点 s 不同, 而导致求出不同的 最远点,那么你在用这个不一定正确的最远点求出的 直径 也可能是错误的**

* [P5536【XR-3】核心城市](https://www.luogu.com.cn/problem/P5536)



## 树型dp

复杂度:$O(n)$

我们从如下角度考虑：
	遍历树上每一个点（从叶子节点开始，并把它看作根节点）维护数组D，数组D[x]表示x到子树最远节点的距离，我们一定是从v（x的子节点）转移过来的，有D[x] = max(D[x],D[v]+w)，然后就好办了，如果我们把x看作根节点，直径就是D[x]加一个次长路（到最子节点第二远的距离）然后有ans = max(ans,D[x]+D[v]+w)

**可用于有负权边求解直径的问题**

模版

```cpp
int D[N];
int ans = 0;
void dp(int x,int fa){
    for(int i  = head[x];~i;i = e[i].next){
        int v = e[i].v;
        int w = e[i].w;
        if(v==fa)continue;
        dp(v,x);
        ans = max(ans,D[x]+D[v]+w);
        D[x] = max(D[x],D[v]+w);
    }
}
```
- [树直径](https://www.acwing.com/problem/content/352/)