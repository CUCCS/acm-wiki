# set
## 用途
set译作集合，set可以实现内部元素自动排序并自动去重。
使用需添加
```cpp 
#include <set>
```
## 基本操作
### 定义
```cpp
set< type > name;
```
同map一样，因为他会自动排序，所以类型为自定义结构体的时候需要重载 **<** 。
### 访问元素
#### 只能通过迭代器访问！！
```cpp
#include <bits/stdc++.h>

using namespace std;
int main()
{
    set<int> s;
    s.insert(1);
    s.insert(2);
    s.insert(1);

    auto it = s.begin();
    cout << *(++ it) << " " << *( -- it ) << endl;
}
```
输出:
```cpp
2 1
```
#### 通过迭代器或auto遍历
```cpp
#include <bits/stdc++.h>

using namespace std;
int main()
{
    set<int> s;
    s.insert(1);
    s.insert(2);
    s.insert(1);
    for( auto i : s ) cout << i << endl;
}
```

### 常用函数

| 函数               | 含义                                                 |
| ------------------- | ---------------------------------------------------- |
| s.begin()           | 返回指向第一个元素的迭代器                           |
| s.end()             | 返回指向最后一个元素的迭代器                         |
| s.clear()           | 清除所有元素                                         |
| s.empty()           | 如果集合为空，返回true                               |
| s.count(val)        | 返回值为val的元素的个数                              |
| **s.erase(val)**        | 删除集合中**所有**值为val的元素                      |
| **s.erase(it)**         | 删除集合中迭代器it指向的元素                         |
| s.erase(first,last) | 删除由迭代器first和last所指定的子集[first, last)     |
| s.equal_range(val)  | 返回有序/升序集合中val元素第一次和最后一次出现的位置 |
| **s.find()**            | 返回一个指向被查找到元素的迭代器                     |
| s.insert(val)       | 在集合中插入值为val的元素                            |
| s.max_size()        | 返回集合能容纳的元素的最大限值                       |
| s.rbegin()          | 返回指向集合中最后一个元素的反向迭代器               |
| s.rend()            | 返回指向集合中第一个元素的反向迭代器                 |
| s.size()            | 集合中元素的数目                                     |
| s.swap(s2)          | 交换两个集合变量                                     |
| **s.upper_bound(val)**  | 返回大于val值元素的迭代器                            |
| **s.lower_bound(val)**  | 返回指向大于（或等于）val值的第一个元素的迭代器      |


注意使用内置的lower_bound()函数会比algorithm中的lower_bound()函数快，因为后者会重建一遍set。
## 常见使用情况
* 去重，排序
* 也可以用来实现离散化
## 时间复杂度
插入删除查找都是$O(logn)$,$n$为set大小。

# multiset
使用需添加
```cpp 
#include <set>
```
### 与set的区别
* 可以出现重复元素，能用来在保持原序列时钟保持顺序的情况下实现快速插入和快速查找。
# unordered_set
使用需添加
```cpp 
#include <unordered_set>
```
### 与set的区别
* 使用散列实现，不可以排序，仅能用来去重。
* 均摊复杂度$O(1)$
