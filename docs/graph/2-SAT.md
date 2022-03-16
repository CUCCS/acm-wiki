!!! note "Copyright"
    本页面贡献者：[zrz](https://github.com/BehindShadow)。
    本页面内容遵循 MIT 协议，转载请附上原文出处链接和本声明。

先置知识:Tarjan,dfs

**2-satisfiability(2-可满足性)**

**描述：有N个变量，每个变量只有两种可能的取值。再给定M个条件，每个条件都是对两个变量取值的限制。求是否存在对N个变量的合法赋值，使M个条件均可满足。**



一般M个条件可以转化为如下语句，若A取值为0，则B取值为1····（如果可能的取值只有0或者1的情况下）

* 非常类似于扩展域的并查集，适用于更普遍的情况（指所在关系的原命题不等同于逆命题），因为并查集维护的全是无向图（命题等于逆命题），而2-SAT是从有向图出发的

- 无向图维护的是什么条件呢？是若p=0，则q=0 和 若q=0，则p=0等价，我们如果有条件若p则q，我们就可以吧p = 0和q = 0放在一起，p = 1和去= 1放在一起
- 有向图呢，就比如有 若p = 0，则q = 0，但是q = 0推不出p = 0
我们只能把 若p = 0，则q = 0，和若q = 1，则p = 1（逆否命题）放在一起（有向边）

* [Katu Puzzle](https://vjudge.net/problem/POJ-3678)

典型的2-SAT问题

- AND条件
    - c = 1
    若 a = 0 则 a = 1
    若 b = 0 则 b = 1
    - c = 0
    若 a = 1 则 b = 0
    若 b = 1 则 a = 0
    (很明显没有 若b = 0，则a = 1)
- OR 条件同理

- XOR条件
    - c = 1
    若 a = 0 则 b = 1
    若 b = 1 则 a = 0
    ······
    （满足无向图的条件）
> 详解请见蓝书P421页
```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 2e6+5;
const int N = 2e3+5;
int n,m;
struct edge{
    int v,next;
}e[maxn<<1];
int head[N],cnt;
void add(int u,int v){
    e[cnt].v = v;
    e[cnt].next = head[u];
    head[u] = cnt++;
}
void inits(){
    memset(head,-1,sizeof(head));
}
int Scc = 0;
int dfn[N],low[N],num;
int st[N],top = 0;
int ins[N];
int c[N];
void Tarjan(int x){
    dfn[x] = low[x] = num++;
    st[++top] = x;ins[x] = 1;
    for(int i = head[x];~i;i = e[i].next){
        int v = e[i].v;
        if(!dfn[v]){
            Tarjan(v);
            low[x] = min(low[x],low[v]);
        }
        else if(ins[v])low[x] = min(low[x],dfn[v]);
    }
    if(dfn[x]==low[x]){
        Scc++;
        int v;
        do{
            v = st[top--];
            c[v] = Scc;
            ins[v] = 0;
        }while(v!=x);
    }
}
int main(){
    cin>>n>>m;
    inits();
    for(int i = 0;i<m;i++){
        int a,b,c;
        char op[10];
        scanf("%d%d%d",&a,&b,&c);
        getchar();
        scanf("%s",op);
        if(op[0]=='A'){
            if(c==1){
                add(a,a+n);
                add(b,b+n);
            }
            else {
                add(a+n,b);
                add(b+n,a);
            }
        }
        else if(op[0]=='O'){
            if(c==1){
                add(a,b+n);
                add(b,a+n);
            }
            else {
                add(a+n,a);
                add(b+n,b);
            }
        }
        else {
            if(c==1){
                add(a,b+n);
                add(a+n,b);
                add(b,a+n);
                add(b+n,a);
            }
            else {
                add(a,b);
                add(a+n,b+n);
                add(b,a);
                add(b+n,a+n);
            }
        }
    }
    for(int i = 0;i<2*n;i++){
        if(!dfn[i]){
            Tarjan(i);
        }
    }
    for(int i = 0;i<n;i++){
        if(c[i]==c[i+n]){
            cout<<"NO"<<endl;
            return 0;
        }
    }
    cout<<"YES"<<endl;
    return 0;
}

```

## 常用方法-Tarjan

**注意这里是求强连通分量的Tarjan**

复杂度：$O(m)$

长话短说，直接在所建的图中跑一个Tarjan,然后依次判断某一个命题和他的否命题是否在同一个联通分量中，若是，则矛盾。

**构造方案数：**

在缩点后的图上，可行解可以使用拓扑序维护，把拓扑序较小的作为正解(注意这是逆向建图)

"注意到Tarjan算法的本质是一次DFS,它在回溯的时候会先取出有向图'底部'的SCC进行标记。故Tarjan算法得到的SCC编号本身就已经满足缩点后有向无环图中'自底向上'的拓扑序"——《算法竞赛进阶指南》

**注意这种方法不能维护字典序最小的可行解**
## 常用方法-dfs

这里的DFS就是暴力求解
复杂度:$O(n^2)$

沿着图上一条路径，如果一个点被选择了，那么这条路径以后的点都将被选择，那么，出现不可行的情况就是，存在一个集合中两者都被选择了。

**可以求得字典序最小的可行解**

模版
```cpp
vector<int>g[maxn];
int tag[maxn];
int vis[maxn];//记录这次搜索了那些点
int num = 0;
int n,m;
bool dfs(int x){
    if(tag[x^1])return false;
    if(tag[x])return true;
    vis[num++] = x;
    tag[x] = 1;
    for(int i = 0;i < (int)g[x].size();i++){
        int v = g[x][i];
        if(!dfs(v)){
            return false;
        }
    }
    return true;
}
bool _SAT(){
    for(int i = 0;i < 2*n; i+=2){
        if(!tag[i]&&!tag[i^1]){
            num = 0;
            if(!dfs(i)){
                while(num){
                    tag[vis[--num]] = 0;
                }
                if(!dfs(i^1))return false;
            }
        }
    }
    return true;
}
```
## 例题

##### Party
- [HDU - 3062](https://vjudge.net/problem/HDU-3062/origin)

题意：
n对夫妻被邀请参加一个聚会，因为场地的问题，每对夫妻中只有1人可以列席。在2n 个人中，某些人之间有着很大的矛盾（当然夫妻之间是没有矛盾的），有矛盾的2个人是不会同时出现在聚会上的。有没有可能会有n 个人同时列席？

思路：
2SAT板子题目，然后就是分两个域，第一个是在0-n表示妻子参加n-2n表示丈夫参加
然后注意建图
2SAT是按照离散数学命题和逆命题规律建立的，比如上题所说，如果我们0号和1号妻子之间有矛盾，那么我们推出的是如果选0号妻子，必选1号丈夫，如果选1号妻子，必选0号丈夫（互为逆否命题），然后关系是需要有传递型的，比如朋友关系就没有，如果我们选有传递性的或者逆命题等价原命题的，使用并查集。
然后可行解可以使用拓扑序维护，把拓扑序较小的作为正解，然后拓扑序，（因为逆向建图），然后就是字典序最小就是dfs暴力解，这个下面说

ps:这个题多组样例，但是题目上没说，wa麻了

```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 2005;
vector<int>g[maxn];
int dfn[maxn];
int ins[maxn];
int low[maxn];
int s[maxn],top = 0;
int num = 0;
int lis_nu = 0;
int tag[maxn];
void Tarjan(int x){
    dfn[x] = low[x] = ++num;
    s[++top] = x;
    ins[x] = 1;
    for(int i = 0;i<g[x].size();i++){
        int v = g[x][i];
        if(!dfn[v]){
            Tarjan(v);
            low[x] = min(low[v],low[x]);
        }
        else if(ins[v])low[x] = min(low[x],dfn[v]);
    }
    if(low[x]==dfn[x]){
        lis_nu++;
        int t;
        do{
            t = s[top--];
            ins[t] = 0;
            tag[t] = lis_nu;
        }while(t!=x);
    }
}
int n,m;
void inits(){
    for(int i = 0;i < n*2;i++){
        g[i].clear();
        dfn[i] = low[i] = ins[i] = 0;
    }
    top = 0;
    lis_nu = 0;
}
int main(){
    
    while(~scanf("%d%d",&n,&m)){
        inits();
        for(int i = 0;i < m;i++){
            int a1,a2,c1,c2;
            scanf("%d%d%d%d",&a1,&a2,&c1,&c2);
            if(c1==0&&c2==0){
                g[a1].push_back(a2+n);
                g[a2].push_back(a1+n);
            }
            else if(c1==0&&c2==1){
                g[a1].push_back(a2);
                g[a2+n].push_back(a1+n);
            }
            else if(c1==1&&c2==0){
                g[a1+n].push_back(a2+n);
                g[a2].push_back(a1);
            }
            else {
                g[a1+n].push_back(a2);
                g[a2+n].push_back(a1);
            }
        }
        for(int i = 0;i < n*2;i++){
            if(!dfn[i]){
                Tarjan(i);
            }
        }
        int flag = 0;
        for(int i = 0;i < n;i++){
            if(tag[i]==tag[i+n]){
                printf("NO\n");flag = 1; 
                break;
            }
        }
        if(!flag)printf("YES\n");
    }   
}
```

## Peaceful Commission
- [HDU - 1814](https://vjudge.net/problem/HDU-1814/origin)

题意：就是说的字典序最下的2SAT，就是n个政党，然后每个政党的有2人，然后有仇视关系，问能否每个政党出一个人，凑一个会议

思路：
2SAT和上面思路一样，不过我们是字典序最小，那么我们只能选择dfs,dfs是一种暴力解法，复杂度$N^2$，然后就是暴力先搜小的，然后由于这里题目给的特殊性我们的两个域，分别表示为x和x^1成对变换，如果是0-n和n-2n，我们需要在开一个opp数组

AC代码
基本是模版，以后可以拿来用
```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 26005;
vector<int>g[maxn];
int tag[maxn];
int vis[maxn];//记录这次搜索了那些点
int num = 0;
int n,m;
bool dfs(int x){
    if(tag[x^1])return false;
    if(tag[x])return true;
    vis[num++] = x;
    tag[x] = 1;
    for(int i = 0;i < (int)g[x].size();i++){
        int v = g[x][i];
        if(!dfs(v)){
            return false;
        }
    }
    return true;
}
bool _SAT(){
    for(int i = 0;i < 2*n; i+=2){
        if(!tag[i]&&!tag[i^1]){
            num = 0;
            if(!dfs(i)){
                while(num){
                    tag[vis[--num]] = 0;
                }
                if(!dfs(i^1))return false;
            }
        }
    }
    return true;
}

void inits(){
    for(int i = 0 ;i <= 2*n;i++){
        g[i].clear();
        tag[i] = 0;
        vis[i] = 0;
    }
}
int main(){
    
    while(~scanf("%d%d",&n,&m)){
        inits();
        for(int i = 0;i<m;i++){
            int a,b;
            scanf("%d%d",&a,&b);
            a--;b--;
            g[a].push_back(b^1);
            g[b].push_back(a^1);
        }
        if(_SAT()){
            for(int i = 0; i < n*2;i+=2){
                if(tag[i]){
                    printf("%d\n",i+1);
                }
                else printf("%d\n",i+2);
            }
        }
        else {
            printf("NIE\n");
        }
    }
    

}
```