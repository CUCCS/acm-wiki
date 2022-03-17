!!! note "Copyright"
    本页面贡献者：[zrz](https://github.com/BehindShadow)。
    本页面内容遵循 MIT 协议，转载请附上原文出处链接和本声明。


### djikstra 单源最短路
* 适用条件：无负权边
* 算法思想： 贪心
* 复杂度：mlog(n)

从bfs演化而来，是最短路最好的算法，（是优先队列的bfs，我们每次寻找的点就是当下最好的，贪心的扩大图中的点
每次找到的num就是在vis集合中能到达的且离s（源点）最近的点，他的最短路就可以确认。
一个点可以进队多次，但是取出来只有剩下的在队伍中作废（因为vis置为1了

PS：

1. 可以用P数组记录路径，p[i]就是记录到扩展到i点的顶点
2. 在无向图不可以有负权边，也不能有负圈

**邻接矩阵版**
复杂度$O(n^2)$
不建议使用
```cpp
int a[1003][1003];//存图
int  d[maxn];//存距离
int p[maxn];//可以用来求一个路径
int vis[maxn];//维护我们已经知道最短路的点集
void dijkstra(int s){
    memset(d,INF,sizeof(d));
    d[s] = 0;
    for(int i = 0;i < n;i++){//总共循环n次，因为我们求n个点的最短路，每次循环必求出一个点的最短路
        int num = -1,minn = INF;
        for(int j=1;j<=n;j++){
            if(!vis[j]&&d[j]<minn){
                num = j;
                minn = d[j];
            }
        }
        vis[num] = 1;//每次选出的点肯定是最短路
        for(int j=1;j<=n;j++){
            if(!vis[j]){
                d[j] = min(d[j],d[num]+a[num][j]);
            }
        }
    }
}
```
**链式前向星版**
建议使用
```cpp
struct dis{
  int id,d;
  bool operator <(const dis &x)const{//优先列对这个结构体的排队规则，注意内部如果是>号是最小的在队头
      return d>x.d;
  }   
};
int vis[maxn];
int d[maxn];//也要一个d数组
struct edge{
    int v,w,next;
}e[maxn<<1];
int cnt,head[maxn];
void add(int u,int v,int w){
    e[cnt].v = v;
    e[cnt].w = w;
    e[cnt].next = head[u];
    head[u] = cnt++;
}
void dijkstra(int s){
    memset(d,INF,sizeof(d));
    priority_queue<dis>q;
    dis ss = {s,0};
    q.push(ss);
    d[s] = 0;
    while(!q.empty()){
        dis now = q.top();
        q.pop();
        if(vis[now.id])continue;//如果已经出过队了就不用在看了
        vis[now.id] = 1;
        for(int i = head[now.id];~i;i = e[i].next){
            int v = e[i].v;
            if(d[v]>now.d+e[i].w){
                d[v] = now.d + e[i].w;
                dis nx = {v,d[v]};
                q.push(nx);
            }
        }
    }
}
```

### Bellman-Ford与SPFA

* 算法思想： 多次迭代收敛答案
* 复杂度：$n^2$

Bellman-Ford，其实大多数书上就说他是迭代扩展，标号修正，其实很难理解，大致流程就是从u出发先找距离为一的点我们把这些点的距离更新，再找距离为2条边的点，从距离为1条边的点推过来，然后距离为3的。。。从其他的点推过来，类似动态规划的flody思想。

蓝书上有另一个说法，就是所有点满足三角不等式，所以我们就从关于源点最近的点开始推，然后推到其他所有边.

SPFA其实是Bellman-Ford的队列优化方法（在国内比较流行），因为我们找下一层边时，队列中的点就是全部带扩展的边，（如果我们不这样的话我我们要扫到其他所有边
PS : 
    1.可以判断负权环，差分约束需要
    2.复杂度比较玄学，能不用就不用
    3.可以用于求最长路，比如我们把边权全部取负，然后跑最短路，结果取负就是最长路

```cpp
int dis[N],vis[N],ct[N];
queue<int>q;//队列和数组记得初始化
int SPFA(int s,int h){//h其实也不需要，我写的这道题需要
	q.push(s);
	vis[s] = 1;ct[s] = 1;dis[s] = 0;
	mxh[s] = h;
	while(!q.empty()){
		int now = q.front();
		q.pop();
		vis[now] = 0;
		for(int i = head[now];~i;i=e[i].next){
			int v = e[i].v;
			int w = e[i].w;
			int limt = e[i].limt;
			if(mxh[v] < min(mxh[now],limt)){//按照最短路的化这里要修改转移方程
				mxh[v]  = min(mxh[now],limt);
				if(!vis[v]){
					ct[v]++;
					vis[v] = 1;
					q.push(v);
					if(ct[v]>n)return 1;//错误返回
				}
			}
		}
	}
	return 0;
}
```



### Folyd
* 适用条件：可以求出任意两点之间的最短路
* 算法思想：dp
  * 最外层枚举可利用的前k个节点，之后利用新加入的节点来更新其他节点之间的最短路
  * 其实和Bellman-Ford很像，只不过前者固定了原点
  * 复杂度$n^3$

Folyd一般不需要模版，这里给出一种记录路径的方法
```cpp
vector<int>path;
int pos[N][N];//记录路径表示i与j经过需要经过的编号最大点（k）
void get_path(int x,int y){//递归找路，加上x与y就能获得整个路径
    if(pos[x][y]==0)return ;
    get_path(x,pos[x][y]);
    path.push_back(pos[x][y]);
    get_path(pos[x][y],y);
}
for(int k = 1;k <= n;k++){
        for(int i = 1;i <= n;i++){
            for(int j = 1;j <= n;j++){
                if(d[i][j] > d[i][k]+d[k][j]){
                    d[i][j] = d[i][k] + d[k][j];
                    pos[i][j] = k;
                }
            }
        }
    }
```

### 最小环

不要想着dfs乱搞过最小环，如果出现环套环，这种情况，dfs标记深度，判环是不可取的，并查集维护集合大小也不可取，dfs搜索到自身也不可取

#### 暴力思想

我们枚举每条边e{u，v, w}，从图中删除这条边e，再从v到u使用dijkstra跑一次最短路，然后将最短路累加w得到环的大小，最后累计一下最小，复杂度$O(m*(m+n)log(n))$，注意这种方法在稀疏图的结果好于Folyd。

#### Folyd


我么给出了一种复杂度比较大的方法，Floyd方法：

众所周知，Floyd算法求最短路的过程是三重循环。当最外层恰好循环到 $k$ 时，代表着目前所求出的最短路所含的点集为$[1,k]$ ,在第k次循环时$dp[i][j]$是i到j的最短路，并且不经过k,我们看k这个点，他经过了两个点，然后这两个点的最短路是$dp[i][j]$，那说明经过至少有k,i,j三个点的最小环就可以求出来了。

复杂度$O(n^3)$

ps:注意初始化dp数组的值

##### 例题
- [Shortest Cycle ](https://vjudge.net/contest/462233#problem/E)

题解：

```cpp
#include <bits/stdc++.h>
#define INF 0x3f3f3f3f
using namespace std;
typedef long long ll;
const int maxn = 1e5+5;
ll a[maxn];
ll g[200][200];
ll dp[200][200];
ll ans = INF;
int main(){
    int n;
    int cnt = 0;
    scanf("%d",&n);
    for(int i = 0;i<n;i++){
        ll x;scanf("%lld",&x);
        if(x!=0)a[cnt++]=x;
    
    }
    if(cnt>64*2){
        printf("3\n");
        return 0;
    }
    for(int i =0;i < cnt;i++){
        for(int j = 0 ;j<cnt;j++){
            dp[i][j] = 100;
            g[i][j] = 100;
        }
    }
    for(int i = 0;i<cnt;i++){
        dp[i][i] = 0;
        for(int j = 0;j < i;j++){
            if((a[i]&a[j])!=0){
                g[i][j] = g[j][i] = 1;
                dp[i][j] = dp[j][i] = 1;
            }
        }
    }
    for(int k = 0;k < cnt;k++){
        for(int i = 0;i < k; i++){
            for(int j = i+1;j<cnt;j++){
                ans = min(ans,dp[i][j] + g[i][k] + g[k][j]);
            }
        }

        for(int i=0;i<cnt;i++){
            for(int j = 0;j<cnt;j++){
                dp[i][j] = min(dp[i][j],dp[i][k] + dp[k][j]);
            }
        }
    }
    
    if(ans>=100){
        cout<<-1<<endl;
    }
    else cout<<ans<<endl;
}
```

* [P1629 邮递员送信](https://www.luogu.com.cn/problem/P1629)

```cpp
#include <bits/stdc++.h>
#define INF 0x3f3f3f3f
using namespace std;
typedef long long ll; 
const int maxn = 1e3+5; 
int n,m;
struct node {
	int id;//点的编号 
	int d;//到起点的距离 
	bool operator <(const node b)const{
		return d>b.d;
	} 
};
int dis[maxn];
int g[maxn][maxn];
int vis[maxn];

struct Ques{
	int u,v,w;
}ques[100005];
void dijkstra(int s){
	priority_queue<node>q;
	dis[s]=0;
	node nod;
	nod.id=s;
	nod.d=0;
	q.push(nod);
	while(!q.empty()){
		int u=q.top().id;
		q.pop();
		if(vis[u])continue;
		vis[u]=1;
		for(int i=1;i<=n;i++){
			if(dis[i]>=dis[u]+g[u][i]){
				dis[i]=dis[u]+g[u][i];
				node nxt;
				nxt.id=i;
				nxt.d=dis[i];
				q.push(nxt);
			}
		}
	}
}
int main(){
	memset(dis,INF,sizeof(dis));
	memset(g,INF,sizeof(g));//初始化的注意
	cin>>n>>m;
	for(int i=0;i<m;i++){
		scanf("%d%d%d",&ques[i].u,&ques[i].v,&ques[i].w);
		g[ques[i].u][ques[i].v] = min(ques[i].w,g[ques[i].u][ques[i].v]);//重边的坑
	}
	dijkstra(1);
	ll ans = 0;
	for(int i=2;i<=n;i++){
		ans +=dis[i];
		
	}
//	cout<<ans<<endl;
	memset(dis,INF,sizeof(dis));
	memset(g,INF,sizeof(g));
	memset(vis,0,sizeof(vis));
	for(int i=0;i<m;i++){
		g[ques[i].v][ques[i].u] = min(ques[i].w,g[ques[i].v][ques[i].u]);
	}
	dijkstra(1);
	for(int i=2;i<=n;i++){
		ans += dis[i];
	}
	cout<<ans;
} 
```