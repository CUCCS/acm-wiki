# map
## 用途
map翻译为映射。实际上数组也相当于映射，如double a[100]，则是建立100个从int到double的映射。但是数组如果要实现从字符串到int的映射，或者实现从一个结构体到另一个结构体的映射则就不那么方便了。而map就是为了解决这种情况而产生的。

使用需添加
```cpp 
#include<map>
```
## 基本操作
### 定义
```cpp
map< type1, type2 > name;
```
map可以实现从任意基本类型到任意基本类型。
但需要注意的一点是，如果type1要使用字符串的话，则必须使用string而不能使用char数组，因为数组不能作为键。但是type2则可以是数组。
### 元素访问
#### 通过键访问
通过之前插入元素时候的键来访问相应的值。
```cpp
#include <bits/stdc++.h>

using namespace std;

int main()
{
    map< string ,int > age;
    age["张三"] = 12;
    age["小王"] = 23;

    cout << age["小王"] << endl;
}
```
输出:
```cpp
23
```
#### 通过迭代器访问
**迭代器定义**
```cpp
map< type1, type2 >::iterator iterator_name;
```
当然也可以使用auto自动推断类型。
map的迭代器既可以使用iterator -> first的方式访问键，也可以通过iterator -> second的方式访问值。
```cpp
#include <bits/stdc++.h>

using namespace std;

int main()
{
    map< string ,int > age;
    age["San Zhang"] = 12;
    age["Wang Xiao"] = 23;
    age["Ann Xia"] = 128;

    for( map<string, int>::iterator it = age.begin(); it != age.end(); ++ it )
        cout << it->first << " " << it->second << endl;
}
```
输出:
```cpp
Ann Xia 128
San Zhang 12
Wang Xiao 23
```
发现输出的顺序和输入顺序不一样。其实map是自动按照字典序从小到大进行的排序。事实上，map是将type1相应的大小比较规则来进行排序。那么可以推知，如果是自定义结构体，那么就需要重载 **<** 号之后才可以使用map。
#### 使用auto遍历
```cpp
#include <bits/stdc++.h>

using namespace std;
void Print( map< string, int > &mp )
{
    for( auto &i : mp )
        cout << "Key = " << i.first << ", Value = " << i.second << endl;
}
int main()
{
    map< string ,int > age;
    age["San Zhang"] = 12;
    age["Wang Xiao"] = 23;

    Print(age);
}
```
输出：
```cpp
Key = San Zhang, Value = 12
Key = Wang Xiao, Value = 23
```
这里可以发现auto和迭代器有些许不同，在auto中访问元素使用的 **"."** 而在迭代其中是用的 **->** 。这是因为auto返回的是元素本身，而迭代器相当于指针。就像是结构体，使用auto返回的是结构体的元素，而迭代器返回的是指向元素的指针。
### 常用函数
| 用法                      | 含义                                                  |
| ---------------------------- | ----------------------------------------------------- |
| **mp[key] = x**                    | 利用数组方式插入数据，key是键，x是值                    |
| mp.at(key) = x                 | 利用at执行插入操作                                    |
| mp.insert(make_pair(key,x))  | 利用insert插入pair(键，值)数据                        |
| mp.emplace(make_pair(key,x)) | 在映射中不存在主键key时执行插入操作                   |
| **mp.size()**                    | 返回mp的大小                                          |
| **mp.count(key)**                | multimap中返回键为key的元素存在的映射数。map中存在key返回1，不存在返回0 |
| **mp.erase(it)**                 | 根据迭代器删除元素                                    |
| **mp.erase(key)**                 | 根据键删除元素                                    |
| **mp.clear()**                   | 清空映射                                              |
| **mp.empty()**                   | 判断映射是否为空                                      |
| **mp.find(key)**                 | 根据键key查找元素，找到以后返回迭代器,不存在返回end                 |
| mp.rbegin()                  | 返回反向迭代器                                        |
| mp.rend()                    | 返回反向迭代器                                        |
| mp.swap(mp2)                 | 将mp和mp2进行交换                                     |
| **mp.lower_bound(key)**          | 返回map中第一个大于或等于key的迭代器指针              |
| **mp.upper_bound(key)**         | 返回map中第一个大于key的迭代器指针                    |

map的键值对是唯一的，如果如果出现重复的键，那么后来者居上，后来的键值对会替换之前的。
## 常见使用情况
### 字符串，结构体与其他数据类型的映射
```cpp
#include <bits/stdc++.h>

using namespace std;
struct A
{
    int a, b;
    bool operator < ( const A &b) const
    {
        return a == b.a ? ( a < b.b ) : ( a < b.a );
    }
};
int main()
{
    map< A, int > mp;
    mp[{1,2}] = 3;
    mp[{-1, 4}] = 1;
    cout << mp.begin() -> first.a << endl;
}
```
可以看到，map按照重载的规则自动进行了排序。
### 可以当作bool数组用
例如：
```cpp
map< string ,bool > age;
```
这样就可以判断某个字符串是否出现过。
### 可以实现离散化
离散化就是说，将一些浮点值，通过一个映射操作，使他们的值映射成整数，但相对大小不变。
离散化同样也可以，将数据范围较大但是个数比较小的序列映射成数据范围比较小的序列。
以上两点在线段树中体现比较明显。
```cpp
#include <bits/stdc++.h>

using namespace std;
int main()
{
    map< double, int > mp;
    double a[] = { 1.1, 1.1, -10023, 123123, 12.23124 };
    for( auto &i : a ) mp.emplace( make_pair( i, 1) );
    int order = 0;
    for( auto &i : mp ) i.second = order ++;
    for( auto i : a )
        cout << "before = " << i << ", after = " << mp[i] << endl;
}
```
输出:
```cpp
before = 1.1, after = 1
before = 1.1, after = 1
before = -10023, after = 0
before = 123123, after = 3
before = 12.2312, after = 2
```
可见a数组中的数实现了离散化。
## 时间复杂度
插入、查找、删除的时间复杂度都是$O(logn)$,$n$是map的大小。
# multimap
使用需添加
```cpp 
#include<map>
```
## 与map的区别
* map的键值对唯一，而multi_map不唯一。
* multimap不能通过键访问元素，因为键值对不唯一([],at等均不可以)；只能通过迭代器访问。
* multimap不能使用mp[key] = value的形式添加元素，只能使用insert() 

# unordered_map
使用需添加
```cpp 
#include <unordered_map>
```
# 与map的区别
* map会自动排序，unordered_map不会
* map用红黑树实现，而unordered_map使用散列。
* unordered_map平均搜索插入复杂度为$O(1)$,即不需要排序时，使用unordered_map会快一些。