# vector
## 用途
vector可以理解成可以变长的数组；也就是数组的长度是可以动态变化的。

vector常用于需要存储内容大小不确定，用普通数组存储会超内存的情况。

使用需添加
```cpp 
#include<vector>
```

## 基本操作

### 定义
```cpp
vector< type > name;
```
type可以是任何数据类型(int，struct，class...),甚至可以是STL。

例如，定义一个可以保存int类型的vector的vector:
```cpp
vector< vector<int> > v;
```
这个时候，vector就相当于两个维度都可以变化的数组。

### 元素访问
#### 使用下标
和普通数组一样，假设有一个vector变量名为v，那么访问使用v[i]就可以访问v种第i个元素。

注意这里下标的范围是**0 - v.size() - 1**，其中size()函数是返回STL中元素的个数。
```cpp
#include <bits/stdc++.h>

using namespace std;

int main()
{
    vector< int > v = {1,2,4,5};
    for( int i = 0; i < v.size(); ++ i ) cout << v[i] << endl;
}
```
输出：
```cpp
1
2
4
5
```
#### 使用迭代器
迭代器可以类比成指针，定义为
```cpp
STL<typename>::iterator iterator_name;
```
其中STL是所用STL的名称，如vector，以及后面要介绍到的map，set等。

如定义一个vector<int>类型的迭代器，则可以使用以下语句：
```cpp
vector<int>::iterator it;
```
使用迭代器来遍历元素：
```cpp
#include <bits/stdc++.h>

using namespace std;

int main()
{
    vector< int > v = {1,2,4,5};
    for( vector< int >::iterator it = v.begin(); it != v.end(); ++ it )
        cout << *it << endl;
}
```
输出：
```cpp
1
2
4
5
```
这种写法与下面这种写法等价:
```cpp
#include <bits/stdc++.h>

using namespace std;

int main()
{
    vector< int > v = {1,2,4,5};
    vector< int >::iterator it = v.begin();
    for(int i = 0; i < v.size(); ++ i)
        cout << *(it + i) << endl;
}
```
可以看到:
* v[i]与*(it + i)作用相同
* 迭代器实现了自增和自减的操作，与指针类似。
* begin()函数返回首元素迭代器，end()返回尾元素的下一个迭代器而不是首元素，左闭右开，其实后面有很多函数都是按照左闭右开的方式来定义的。

注意：
* 迭代器的类型必须和定义的类型一致，即我不能用vector< double >类型的迭代器访问vector< int >型的vector
* **只有vector和string才能使用it+i这种写法**

当然迭代器需要写这么长，不太方便，我们可以使用**auto**改写上述代码:
```cpp
#include <bits/stdc++.h>

using namespace std;

int main()
{
    vector< int > v = {1,2,4,5};
    for( auto it = v.begin(); it != v.end(); ++ it )
        cout << *it << endl;
}
```
#### 使用auto遍历

如果仅仅需要遍历所有元素，那么我们可以使用如下方式：
```cpp
#include <bits/stdc++.h>

using namespace std;

int main()
{
    vector< int > v = {1,2,4,5};
    for( int i: v ) cout << i << endl;
}
```
或者更简单一点，使用auto：
```cpp
int main()
{
    vector< int > v = {1,2,4,5};
    for( auto i: v ) cout << i << endl;
}
```
需要注意的一点是，如果需要修改其中的每个元素，那么需要这样写:
```cpp
#include <bits/stdc++.h>

using namespace std;

int main()
{
    vector< int > v = {1,2,4,5};
    for( auto &i: v )
    {
        cout << i << " ";
        i = i + 1;
    }
    cout << endl;

    for( auto i : v ) cout << i << " ";
}
```
输出：
```cpp
1 2 4 5
2 3 5 6
```
而如果不使用引用：
```cpp
#include <bits/stdc++.h>

using namespace std;

int main()
{
    vector< int > v = {1,2,4,5};
    for( auto i: v )
    {
        cout << i << " ";
        i = i + 1;
    }
    cout << endl;

    for( auto i : v ) cout << i << " ";
}
```
则会输出：
```cpp
1 2 4 5
1 2 4 5
```
原因是如果不适用引用，相当于对其中的每个元素建立了一个拷贝，而引用则是对其中的每个元素建立一个引用，当然可以修改真正的值。
### 常用函数
#### push_back()与pop_back()
push_back()故名思意，就是再当前vector的后边添加一个元素，而pop_back()就是再vector后边删除一个元素。
```cpp
#include <bits/stdc++.h>

using namespace std;
void Print( vector<int> &v )
{
    cout << "size = " << v.size() << ", elements = [ ";
    for( auto i : v ) cout << i << " ";
    cout << "]" << endl;
}
int main()
{
    vector< int > v;
    for( int i = 1; i < 5; ++ i )
        v.push_back(i);
    Print(v);
    
    v.pop_back();
    Print(v);
}
```
输出:
```cpp
size = 4, elements = [ 1 2 3 4 ]
size = 3, elements = [ 1 2 3 ]
```
#### clear()与empty()
clear()用于清空STL，empty()是用于判断STL是否为空，如果为空返回1，否则返回0；
```cpp
#include <bits/stdc++.h>

using namespace std;
void Print( vector<int> &v )
{
    cout << "size = " << v.size() << ", elements = [ ";
    for( auto i : v ) cout << i << " ";
    cout << "]" << endl;
}
int main()
{
    vector< int > v;
    for( int i = 1; i < 5; ++ i )
        v.push_back(i);
    Print(v);

    v.clear();
    Print(v);
}
```
输出:
```cpp
size = 4, elements = [ 1 2 3 4 ]
size = 0, elements = [ ]
```
clear()的功能这种写法与以下写法相同:
```cpp 
while( !v.empty() ) v.pop_back();
```
#### insert()
insert()有三种形式：
| 函数 |作用  |
|--|--|
| v.insert(it, val)         | 向迭代器it指向的元素前插入新元素val                                                                 |
| v.insert(it, n, x)        | 向迭代器it指向的元素前插入n个x                                                                      |
| v.insert(it, first, last) | 将由迭代器first和last所指定的序列[first, last)插入到迭代器it指向的元素前面    |


举几个例子:
插入一个元素：
```cpp
#include <bits/stdc++.h>

using namespace std;
void Print( vector<int> &v )
{
    cout << "size = " << v.size() << ", elements = [ ";
    for( auto i : v ) cout << i << " ";
    cout << "]" << endl;
}
int main()
{
    vector< int > v, v1;
    for( int i = 1; i < 5; ++ i )
        v.push_back(i), v1.push_back(i + 100);
    Print(v);

    auto it = v.begin() + 1;
    v.insert( it, 1E9);
    Print(v);
}
```
输出：
```cpp
size = 4, elements = [ 1 2 3 4 ]
size = 5, elements = [ 1 1000000000 2 3 4 ]
```
插入序列：
```cpp
#include <bits/stdc++.h>

using namespace std;
void Print( vector<int> &v )
{
    cout << "size = " << v.size() << ", elements = [ ";
    for( auto i : v ) cout << i << " ";
    cout << "]" << endl;
}
int main()
{
    vector< int > v;
    for( int i = 1; i < 5; ++ i )
        v.push_back(i);
    Print(v);

    auto it = v.begin() + 1;
    v.insert( v.begin(), it, it + 2 );
    Print(v);
}
```
输出:
```cpp
size = 4, elements = [ 1 2 3 4 ]
size = 6, elements = [ 2 3 1 2 3 4 ]
```
当然也可以是其他vector的迭代器，不过类型要一致。
```cpp
#include <bits/stdc++.h>

using namespace std;
void Print( vector<int> &v )
{
    cout << "size = " << v.size() << ", elements = [ ";
    for( auto i : v ) cout << i << " ";
    cout << "]" << endl;
}
int main()
{
    vector< int > v, v1;
    for( int i = 1; i < 5; ++ i )
        v.push_back(i), v1.push_back(i + 100);
    Print(v);

    auto it = v1.begin() + 1;
    v.insert( v.begin(), it, it + 2 );
    Print(v);
}
```
结果：
```cpp
size = 4, elements = [ 1 2 3 4 ]
size = 6, elements = [ 102 103 1 2 3 4 ]
```
#### erase()
erase()有两种形式：
|  函数|作用  |
|--|--|
| v.erase(it)               | 删除由迭代器it所指向的元素                                                                          |
| v.erase(first, last)      | 删除由迭代器first和last所指定的序列[first, last)                         |


例如:
```cpp
#include <bits/stdc++.h>

using namespace std;
void Print( vector<int> &v )
{
    cout << "size = " << v.size() << ", elements = [ ";
    for( auto i : v ) cout << i << " ";
    cout << "]" << endl;
}
int main()
{
    vector< int > v, v1;
    for( int i = 1; i < 5; ++ i )
        v.push_back(i), v1.push_back(i + 100);
    Print(v);

    auto it = v.begin() + 1;
    v.erase( it);
    Print(v);
}                   
```
结果：
```cpp
size = 4, elements = [ 1 2 3 4 ]
size = 3, elements = [ 1 3 4 ]
```
## 常见使用情况
### 用邻接表的形式存储图
例如用以下形式给出一个图：
$n$ $m$
$a_1$ $b_1$ $c_1$
$\vdots$
$a_m$ $b_m$ $c_m$
其中n是节点数，m是边数，下面有m行，表示从$a_i$到$b_i$有一条权值为$c_i$的边。
假设n,m很大，1E5左右，那么就不能使用邻接矩阵来存储，而是用vector则可以办得到。
```cpp
#include <bits/stdc++.h>

using namespace std;
const int maxn = 1E5 + 5;

struct edge { int to, weight; };
vector< edge > v[maxn];
int main()
{
    int n, m; cin >> n >> m;
    while( m -- )
    {
        int a, b, c; cin >> a >> b >> c;
        v[a].push_back( {b, c} );
    }
}
```
### 存储输入个数不定的数据
如输入许多字符串，直到遇到end时停止，那么可以使用vector将这些数据保存起来:
```cpp
#include <bits/stdc++.h>

using namespace std;

int main()
{
    string tmp;
    vector< string > v;
    while( cin >> tmp, tmp != "end" ) 
        v.push_back(tmp);
}
```
### 模拟stack(vector速度比stack要快)
stack是一种后进先出的数据结构，在实现Tarjan或者递归时会经常用到。因为vector速度要比真正的stack要快，所有常常使用vector来模拟stack。
如stack的push()可以用vector的push_back()，而pop()则可以用pop_back()；

## 时间复杂度
* 在vector后插入和删除、以及元素访问是$O(1)$
* 在任意位置插入时$O(k)$，k是插入位置之后的元素个数。
