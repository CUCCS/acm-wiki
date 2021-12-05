!!! note "Copyright"
    本页面贡献者：[zrz](https://github.com/BehindShadow)。
    本页面内容遵循 MIT 协议，转载请附上原文出处链接和本声明。

- **先置知识，dfs树**

### 无向图
* 在无向图中，如果顶点Vi到顶点Vj有路径，则称顶点Vi和Vj连通。

* 如果无向图中任意两个顶点之间都连通，则称为连通图。

* 如果不是连通图，则图中的极大连通子图称为连通分量。

**重点区分：极大连通子图 和 极小连通子图**

* 极大连通子图是无向图的连通分量，极大要求该连通子图包含其所有的边。

* 极小连通子图是既要保持图连通，又要使得边数最少的子图。

**进一步，到有向图中，概念就变为强连通，强连通图，强连通分量**
### 有向图
* 在有向图中，如果从Vi到Vj  和  从Vj到Vi之间都有路径，则称这两个顶点是强连通的

* 若图中任何一对顶点都是强连通的，则称此图为强连通图

* 有向图中的极大强连通子图称为有向图的强连通分量

#### 基本的知识
判断图的连通性方法（DFS，BFS，并查集）



### 有向图Tarjan与强连通分量

求强连通分量，应该是不能用并查集去搞这个


变量说明
```cpp
#define ms(a,v)  memset(a,v,sizeof(a))
int n,m;
const int maxn = 10005; //点数
int head[maxn],cnt = 0;
struct {
	int u,v,next;
}e[100005];
void add(int u,int v){
	e[cnt].u = u;
	e[cnt].v = v;
	e[cnt].next = head[u];
	head[u] = cnt++;
}
int low[maxn],dfn[maxn],vis[maxn];//vis数组是记录点是否在栈内  dfn是记录每个点dfs序
stack<int> s;
int num = 0;//dfs序计数，或者理解为时间戳 
int lis_num = 0;//强连通分量的个数 
int tag[maxn];//tag是记录每个点的属于几号连通分量
```
> 初始化代码
```c++
void inits(){
	lis_num = 0;num = 0;cnt = 0;
	ms(head,-1);
	ms(vis,0);
	ms(tag,0);
	ms(dfn,0);
	ms(low,0);
}
```
Tarjan
```c++
void Tarjan(int now){
	s.push(now);//栈可以数组代替
	vis[now] = 1;
	dfn[now] = low[now]= ++num;
	for(int i=head[now];~i;i=e[i].next){
		int v = e[i].v;
		if(!dfn[v]){
			Tarjan(v);
			low[now]  = min(low[now],low[v]);
		}
		else if(vis[v]){
			low[now] = min(low[now],dfn[v]);
		}
	}
	if(dfn[now]==low[now]){//出栈
		lis_num++;
		int t;
		do{
			t = s.top();
			vis[t] = 0;
			tag[t] = lis_num;//这个可以没有如果不需要记录联通分量的序号
			s.pop();	
		}while(t!=now);
	}
}
```
这个还可用于DAG的缩点（有别于并查集的缩点）
例如：
```c++
 for(int i=1; i<=n; i++)
    {
        int sz=g[i].size();
        for(int j=0; j<sz; j++)
        {
            int v=g[i][j];
            if(color[v]!=color[i])
            {
                du[color[i]]++;
				//在这里可以建一个新的图
            }
        }
        cnt[color[i]]++;//统计每一个分量的点数
    }
```


### 无向图求连通分量，割点（关节点）


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