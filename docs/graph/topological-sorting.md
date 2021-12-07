!!! note "Copyright"
    本页面贡献者：[zrz](https://github.com/BehindShadow)。
    本页面内容遵循 MIT 协议，转载请附上原文出处链接和本声明。

## 有向无环图
边有向，无环。

英文名叫 Directed Acyclic Graph，缩写是 DAG。

能 拓扑排序 的图，一定是有向无环图；

如果有环，那么环上的任意两个节点在任意序列中都不满足条件了。

有向无环图，一定能拓扑排序；

（归纳法）假设节点数不超过$k$的 有向无环图都能拓扑排序，那么对于节点数等于$k$的，考虑执行拓扑排序第一步之后的情形即可。

## 拓扑排序

概念：对于一张有向无环图(DAG)而言，该图的拓扑排序是一个由该图所有顶点组成的线性序列。使得图中任意一对顶点u和v，若存在边 从 u 指向 v，则u在线性序列中出现在v之前。
(对一个有向无环图(Directed Acyclic Graph简称DAG)G进行拓扑排序，是将G中所有顶点排成一个线性序列，使得图中任意一对顶点u和v，若边(u,v)∈E(G)，则u在线性序列中出现在v之前。

拓扑序模版
```cpp
// 预先处理in数组，ans 是存储拓扑序的数组，如果要方便查找，再加一个pos数组
vector<int>g[maxn];//这里我用的是vector存的图
	queue<int> q;
	for(int i=1;i<=n;i++){
		if(in[i]==0){
			q.push(i);
		}
	}
	int ct = 0;
	while(!q.empty()){
		int f = q.front();
		q.pop();
		ans[++ct] = f;
		pos[f] = ct;
		for(int i=0;i<g[f].size();i++){
			int v = g[f][i];
			in[v]--;
			if(in[v]==0){
				q.push(v);
			}
		}
	}
	if(ct<n)这里如果成立说明图是有环的所以无法排序
```
链式前向行的拓扑排序版本
```c++
struct node{
    int to,next;
}e[maxn<<1];
void init(){
    vec.clear();
    cnt=tot=0;
    for(int i=1;i<=n;i++){
        pos[i]=deg[i]=head[i]=0;
    }
}
void add(int u,int v){
    e[++cnt].to=v;
    e[cnt].next=head[u];
    head[u]=cnt;
}
//


while(!que.empty()){    
    int x=que.front();
    que.pop();
    pos[x]=++tot;
    for(int i=head[x];i;i=e[i].next){
        deg[e[i].to]--;
        if(deg[e[i].to]==0){
            que.push(e[i].to);
        }
    }
}
if(tot!=n){//有环
    printf("NO\n");
}else{

```

- [P4017 最大食物链计数](https://www.luogu.com.cn/problem/P4017)
    
- [Directing Edges](https://vjudge.net/problem/CodeForces-1385E)

- [B-Rank of Tetris](https://vjudge.net/contest/399479#problem/B)


```cpp
#include <bits/stdc++.h>
#define INF 0x3f3f3f3f
using namespace std;
const int maxn = 10005;
int in[maxn];
vector<int> g[maxn];
int fa[maxn];
int n,m;
int a[maxn<<1],b[maxn<<1];
char ch[maxn<<1];
int get(int x){
	if(fa[x]==x)return x;
	else return fa[x] = get(fa[x]);//路径压缩
}
void merge(int x,int y){
	int p = get(x);
	int q = get(y);
	if(p!=q)fa[p] = q;
}
int ans[maxn];//其实这个题不大需要
void inits(){
	memset(ans,0,sizeof(ans));
	memset(in,0,sizeof(in));
	for(int i=0;i<=n;i++){
		g[i].clear();
	}
}
int main(){
	while(~scanf("%d%d",&n,&m)){
		inits();
		for(int i=0;i<n;i++)fa[i]=i;
		for(int i=0;i<m;i++){
			scanf("%d %c %d",&a[i],&ch[i],&b[i]);
			if(ch[i]=='='){
				merge(a[i],b[i]);//我们得记得提前合并然后在处理
			}
		}
		for(int i=0;i<m;i++){
			if(ch[i]=='<'){
				g[get(b[i])].push_back(get(a[i]));
				in[get(a[i])]++;
			}
			else if(ch[i]=='>'){
				g[get(a[i])].push_back(get(b[i]));
				in[get(b[i])]++;
			}
		}
		int nn=0;//记录合并之后的点数
		for(int i=0;i<n;i++){
			if(fa[i]==i)nn++;
		}
		queue<int>q;
		for(int i=0;i<n;i++){
			if(in[get(i)]==0&&i==get(i)){//这个点没有合并才可以算
				q.push(get(i));
			}
		}
		int ct=0,flag=0;
		while(!q.empty()){
			int f = q.front();
			//cout<<"k";
			int cnt = q.size();
			if(cnt>1){
				flag=1;
			}
			q.pop();
			ans[++ct] = f;
			for(int i=0;i<g[f].size();i++){
				int v = g[f][i];
				in[v]--;
				if(!in[v]){
					q.push(v);
				}
			}
		}
		if(ct<nn){
			printf("CONFLICT\n");//有环是矛盾
		}
		else {
			if(flag)printf("UNCERTAIN\n");//对内有两个是信息不足
			else printf("OK\n");
		}
	}
} 
```
## 参考资料
- [图论 OI Wiki](https://oi-wiki.org/graph/mst/)