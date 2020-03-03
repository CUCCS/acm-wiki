# queue
## 用途
是一个先进先出的数据结构，与stack正好相反。

使用需添加
```cpp 
#include <queue>
```

## 基本操作
### 定义
```cpp
queue<type> name;
```
### 元素访问
queue的访问比较特殊，queue没有迭代器，每次只能访问队首元素，即使用front()函数。
如果要遍历则需要像这样使用：
```cpp
#include <iostream>
#include <queue>
using namespace std;
int main()
{
    queue< int > q;
    q.push(1);
    q.push(2);
    q.push(3);

    while( !q.empty() )
    {
        cout << q.front() << " ";
        q.pop();
    }
}
```

### 常用函数
|函数      | 作用                     |
| --------- | ------------------------ |
| q.push()  | 入队                     |
| q.pop()   | 出队                     |
| q.front() | 返回首元素               |
| q.back()  | 返回末元素               |
| q.size()  | 输出现有元素的个数       |
| q.empty() | 队列为空返回1，反之返回0 |

## 常见使用情况
### bfs
假设以邻接表的形式存图。
```cpp
#include <iostream>
#include <queue>

using namespace std;

const int maxn = 1E5;
bool vis[maxn];
vector<int> G[maxn];
void BFS()
{
    int s;
    queue< int > q;
    q.push(s);
    while( !q.empty() )
    {
        int ft = q.front(); vis[ft] = 1; q.pop();
        for( auto to : G[ft] )
            if( !vis[to] ) q.push(to);
    }
}
```
### 最原始的Bellman-Fold
```cpp
struct node
{
    int to;
    long long val;
    node( int to = 0, long long val = 0 ) : to(to), val(val) {}
};
vector< node > G[maxn];
long long dis[maxn];
bool inq[maxn];
void SPFA( int s )
{
    memset( inq, 0, sizeof( inq ) );
    for( int i = 1; i < maxn; ++ i) dis[i] = 1E18;
    dis[s] = 0, inq[s] = 1;
    queue< int > q; q.push(s);
    while( !q.empty() )
    {
        int x = q.front(); q.pop();
        inq[x] = 0;
         //这里和堆优化的区别就显现出来了，堆优化版本只会入队一次，而SPFA则不是
        for( auto to : G[x] )
        {
            if( dis[to.to] > dis[x] + to.val )
            {
                dis[to.to] = dis[x] + to.val;
                if( !inq[to.to] ) q.push(to.to), inq[to.to] = 1;
            }
        }
    }
}
```
## 时间复杂度
push和pop均为$O(1)$
# priority_queue
## 用途
保证队首元素优先级最大，相当于大根堆。

使用需添加
```cpp 
#include <queue>
```
## 常见操作
### 定义
```cpp
priority_queue< type > name;
```
### 常用函数
* 基本与queue相同，只不过priority_queue始终保证队首元素优先级最大，而不是先进先出。
* 队首元素需要使用top()而不是front()
### 优先级的设置
同map类似，需要重载 **<** 号。
但是，排序结果时相反的，因为他是按照优先级来的，所以越大的数，优先级越大，会排在前边。
```cpp
#include <iostream>
#include <queue>

using namespace std;

struct pet
{
    string species;
    int price;
    bool operator < ( const pet& b ) const
    {
        return price < b.price;
    }
};
int main()
{
    priority_queue< pet > q;
    q.push({"cat",100} );
    q.push({"dog", 50 } );
    q.push({"Tarjan", 10000} );

    while( !q.empty() )
    {
        cout << q.top().species << endl;
        q.pop();
    }
}
```
输出:
```cpp
Tarjan
cat
dog
```
而如果时int等基本类型需要设置越小的数优先级越高，则需要添加
```cpp
#include <functional>
```
并用如下定义方式:
```cpp
priority_queue< int, vector<int>, greater<int> > q;
```
如：
```cpp
#include <iostream>
#include <queue>
#include <functional>
using namespace std;

int main()
{
    priority_queue< int, vector<int>, greater<int> > q;
    q.push(2);
    q.push(50);
    q.push(1);

    while( !q.empty() )
    {
        cout << q.top() << endl;
        q.pop();
    }
}
```
输出:
```cpp
1
2
50
```
## 常见使用情况
### 堆优化的Dijkstra和Bellman-Fold
```cpp
struct edge
{
    int pos, val;
    edge( int pos = 0, int val = 0 ) : pos(pos), val(val) {}
    bool operator < ( const edge &e ) const
    {
        return val > e.val;
    }
};
vector< edge > G[maxn];
int dis[maxn];
bool vis[maxn];
void Dijkstra( int s )
{
    memset( dis, 0x3f, sizeof( dis ) );
    priority_queue< edge > q;
    dis[s] = 0; q.push({s,dis[s]});
    while( !q.empty() )
    {
        auto tp = q.top( ); q.pop( );
        if ( vis[tp.pos] ) continue;
        inq[tp.pos] = 1;
        for ( auto v : G[tp.pos] )
        {
            if( dis[v.pos] > dis[tp.pos] + v.val )
            {
                dis[v.pos] = dis[tp.pos] + v.val;
                q.push( {v.pos, dis[v.pos]} );
            }
        }
    }
}
```
### 一些需要贪心的情况