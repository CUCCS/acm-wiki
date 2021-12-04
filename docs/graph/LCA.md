## 关于离线和在线

* 离线算法其实就是将多个询问一次性解决。离线算法往往是与在线算法相对的。例如求LCA的算法中，树上倍增属于在线算法，在对树进行O(n)预处理后，每个询问用$O(log_2n)$复杂度回答。而离线的Tarjan算法则是用$O(n+q)$时间将询问一次性全部回答。


## 朴素算法
向上标记法


### Tarjan

Tarjan的基本思想其实的就是想上标记法的优化，使用并查集优化，就是把回溯的点的合并到父节点，难点其实在于记录闻讯和dfs函数的结构
复杂度$O(n+m)$

* [How far away ？ ](https://vjudge.net/problem/HDU-2586)

```cpp
#include <bits/stdc++.h>
#define ms(x,y) memset(x,y,sizeof(x))
using namespace std;
const int N = 40005;
int n,m;
struct edge{
    int v,next,w;
}e[N<<1];
int head[N],cnt;
int vis[N],dis[N];
int ans[N];
void add(int u,int v,int w){
    e[cnt].v = v;
    e[cnt].w = w;
    e[cnt].next = head[u];
    head[u] = cnt++;
}
int fa[N];
int get(int x){
    if(x==fa[x])return x;
    else return fa[x] = get(fa[x]);
}
vector<int>qu[N];
vector<int>qu_id[N];
void inits(){
    cnt = 0;
    for(int i = 0;i<=n;i++){
        fa[i] = i;ans[i] = 0;
        head[i] = -1;vis[i] = 0;dis[i] = 0;
        qu[i].clear();
        qu_id[i].clear();
    }
}
void add_query(int x,int y,int id){
    qu[x].push_back(y);qu_id[x].push_back(id);
    qu[y].push_back(x);qu_id[y].push_back(id);
}

void Tarjan(int x,int father){
    for(int i = head[x];~i;i = e[i].next){
        int v = e[i].v;
        int w = e[i].w;
        if(v==father)continue;
        dis[v] = dis[x]+w;
        Tarjan(v,x);
        fa[v] = x;
    }
    for(int i =0;i < qu[x].size();i++){
        int y = qu[x][i];
        if(vis[y] == 1){
            int lca = get(y);
            ans[qu_id[x][i]] = dis[x]+dis[y]-2*dis[lca];
        }
    }
    vis[x] = 1;//回溯的时候要标记这个点已经遍历过并且回溯了
}
int main(){
    int T;
    cin>>T;
    while(T--){
        scanf("%d%d",&n,&m);
        inits();
        for(int i = 0;i < n-1; i++){
            int u,v,w;
            scanf("%d%d%d",&u,&v,&w);
            add(u,v,w);add(v,u,w);
        }
        for(int i = 0;i < m;i++){
            int x,y;
            scanf("%d%d",&x,&y);
            add_query(x,y,i);
        }
        Tarjan(1,0);
        for(int i = 0;i<m;i++){
            printf("%d\n",ans[i]);
        }
    }

}
```




 * [P3379【模板】最近公共祖先(LCA)](https://www.luogu.com.cn/problem/P3379)

- Tarjan(离线)算法

直接的模板
```c++
#include <bits/stdc++.h>
using namespace std;
const int maxn = 500005;
int head[maxn],cnt=0;
int fa[maxn],vis[maxn],ans[maxn];
struct edge{
	int u,v,next;
}e[maxn<<1];
struct note { int node, id; }; //询问以结构体形式保存
vector<note> ques[maxn];
void add(int u,int v){
	e[cnt].u = u;
	e[cnt].v = v;
	e[cnt].next = head[u];
	head[u] = cnt++;
}
int get(int x){
	if(fa[x]==x)return x;
	else return fa[x]=get(fa[x]);//路径压缩需要
}
void dfs(int u,int from){
	for(int i=head[u];~i;i=e[i].next){
		int v = e[i].v;
		if(v==from)continue;
		dfs(v,u);
		fa[v]=u;
	}
	int len = ques[u].size();
	 for (int i = 0; i < len; i++) 
        if (vis[ques[u][i].node]) 
            ans[ques[u][i].id] = get(ques[u][i].node); 
    
    //访问完毕回溯
    vis[u] = 1; 
}
int main(){
	memset(head,-1,sizeof(head));
	int n,m,s;
	cin>>n>>m>>s;
	for(int i=0;i<=n;i++)fa[i]=i;
	for(int i=0;i<n-1;i++){
		int x,y;
		scanf("%d%d",&x,&y);
		add(x,y);
		add(y,x);
	}
	for(int i=0;i<m;i++){
		int x,y;
		scanf("%d%d",&x,&y);
		note no;                    //说说这里，id记录了第几个被询问
		no.node=y;                  //node表示第x点与那个点有关联
		no.id=i;                        
		ques[x].push_back(no);
		no.node=x;                  //要入队两次
		no.id = i;
		ques[y].push_back(no);
	}
	dfs(s,0);
	for(int i=0;i<m;i++){
		cout<<ans[i]<<endl; 
	}
} 
```


##  在线倍增lca

复杂度：初始化$O(n)$问讯$O(logn)$

```cpp
int n,m;
int dis[N];//这个是记录到根节点的距离（可以不要）
int d[N];//这个是记录层数（深度），用于lca向上倍增
int f[N][30];//倍增数组
int t ;
void bfs(int x){//遍历一遍预处理
    queue<int >q;
    q.push(x);
    d[x] = 1;
    while(!q.empty()){
        int now = q.front();
        q.pop();
        for(int i = head[now];~i;i = e[i].next){
            int v = e[i].v;
            int w = e[i].w;
            if(d[v])continue;
            d[v] = d[now]+1;
            dis[v] = dis[now] + w;
            f[v][0] = now;
            for(int i = 1;i < t; i++){
                f[v][i] = f[f[v][i-1]][i-1];
            }
            q.push(v);
        }
    }
}
int lca(int x, int y){
    if(d[x] > d[y])swap(x,y);
    for(int i = t;i >= 0;i--){
        if(d[f[y][i]] >= d[x])y = f[y][i];
    }
    if(x==y)return x;
    for(int i = t;i >= 0;i--){
        if(f[x][i]!=f[y][i]){
            x = f[x][i];
            y = f[y][i];
        }
    }
    return f[x][0];
}

```