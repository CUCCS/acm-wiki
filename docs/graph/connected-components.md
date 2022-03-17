!!! note "Copyright"
    本页面贡献者：[zrz](https://github.com/BehindShadow)。
    本页面内容遵循 MIT 协议，转载请附上原文出处链接和本声明。

先置知识:dfs树,图论基本知识

### 无向图
* 在无向图中，如果顶点$V_i$到顶点$V_j$有路径，则称顶点$V_i$和$V_j$连通。

* 如果无向图中任意两个顶点之间都连通，则称为连通图。

* 如果不是连通图，则图中的极大连通子图称为连通分量。

**重点区分：极大连通子图 和 极小连通子图**

* 极大连通子图是无向图的连通分量，极大要求该连通子图包含其所有的边。

* 极小连通子图是既要保持图连通，又要使得边数最少的子图。

**进一步，到有向图中，概念就变为强连通，强连通图，强连通分量**
### 有向图
* 在有向图中，如果从$V_i$到$V_j$  和  从$V_j$到$V_i$之间都有路径，则称这两个顶点是强连通的

* 若图中任何一对顶点都是强连通的，则称此图为强连通图

* 有向图中的极大强连通子图称为有向图的强连通分量

#### 基本的知识
判断图的连通性方法（DFS，BFS，并查集）



### 有向图Tarjan与强连通分量

参考[强联通分量](./strongly-connected-components.md)



### 无向图求连通分量，割点（关节点）


对于一个无向图，如果把一个点删除后这个图的极大连通分量数增加了，那么这个点就是这个图的割点（又称割顶）。

**割点判定法则：**

**若x不是搜索树的根节点(dfs起点)**，则$x$是割点当且仅当搜索树上存在$x$的一个子节点$y$,满足：

$$dfn[x] \leq low[y]$$

**思考:**

- 为何起点不是割点
- 为何要求小于等于


```cpp
vector<int>g[maxn];
int dfn[maxn],low[maxn];
int dep=0,child=0;
int cut[maxn];
int n,m;
void Tarjan(int u,int fa)
{
    dfn[u]=low[u]=++dep;
    for(int i=0;i<g[u].size();i++)
    {
        int v=g[u][i];
        if(!dfn[v])
        {
            Tarjan(v,u);
            low[u]=min(low[u],low[v]);
            //if(u==root) child++;    //对于根结点是否为割点的判定：记录子树个数
            if(low[v]>=dfn[u]){		//注意割点是 >=
				cut[u]++;//或者用iscut可以判断是否为割点但是根节点得特判			//其他结点u若符合该条件，u就是割点				
			}                       
        }
        else if(v!=fa) low[u]=min(low[u],dfn[v]);
    }
}
```

### 无向图求连通分量，割边（桥）

对于一个无向图，如果删掉一条边后图中的连通分量数增加了，则称这条边为桥或者割边。严谨来说，就是：假设有连通图$G = \{V,E\}$，$e$是其中一条边（即$e\in E$），如果$G-e$是不连通的，则边$e$是图$G$的一条割边（桥）。

**割边判定法则：**

无向边$(x,y)$是桥，当且仅当搜索树上存在$x$的一个子节点$y$,满足:

$$dfn[x] < low[y]$$

```cpp
const int N = 1005;
const int maxn = N*N+5;
struct edge{
    int v,next,w;
}e[maxn<<1];
int n,m;
int head[N],cnt;
int ans = INF;
void add(int u,int v,int w){
    e[cnt].v = v;
    e[cnt].w = w;
    e[cnt].next = head[u];
    head[u] = cnt++;
}
int dfn[N],low[N];
int num = 0;
void inits(){
    // memset(head,-1,sizeof head);
    for(int i = 0; i <= n;i++){
        dfn[i] = low[i] = 0;
        head[i] = -1;
    }
    cnt = 0;
    num = 0;
    ans = INF;
}

void Tarjan(int x,int in_edge){
    dfn[x] = low[x] = ++num;
    for(int i = head[x];~i;i=e[i].next){
        int v = e[i].v;
        if(!dfn[v]){
            Tarjan(v,i);
            low[x] = min(low[v],low[x]);
            if(low[v]>dfn[x]){ //注意这里是>
                ans = min(e[i].w,ans);
            }
        }
        else if(i!=(in_edge ^ 1))low[x] = min(low[x],dfn[v]); //成对变换，配合存边下标从0开始
    }   
}
```
主函数内，如果图不连通就多次Tarjan
```cpp
Tarjan(1,-1); 
```

### 双联通分量

在一张连通的无向图中，对于两个点$u$和$v$，如果无论删去哪条边（只能删去一条）都不能使它们不连通，我们就说$u$和$v$边双连通。

在一张连通的无向图中，对于两个点$u$和$v$，如果无论删去哪个点（只能删去一个，且不能删$u$和$v$自己）都不能使它们不连通，我们就说$u$和$v$点双连通。

