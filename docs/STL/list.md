## 用途
就是链表，当需要快速$O(1)$的时间插入或者删除时需要使用。

使用需添加
```cpp 
#include <unordered_set>
```
## 基本操作
### 定义
```cpp
list< type > name;
```
### 访问元素
#### 仅能使用迭代器访问
```cpp
#include <iostream>
#include <list>

using namespace std;

int main()
{
    list< int > l;
    l.push_back(1);
    auto it = l.begin();
    cout << *it << endl;
}
```
输出:
```cpp
1
```
### 常用函数
| 函数                          | 含义                                                                  |
| ----------------------------- | --------------------------------------------------------------------- |
|l.begin()|返回链表首地址|
|l.end()|返回链表尾地址|
|l.front()|返回首元素|
|l.back()|返回尾元素|
|l.push_back(elem)|插入元素到链表尾部|
|l.push_front(elem)|插入元素到链表头部|
|l.empty()|判断链表是否为空|
|l.insert(it, val1, val2))|在指定位置插入一个或多个元素|
|l.resize(n)|调整链表大小为n，超出n删除，少于n补0|
|l.clear()|清除|
|l.assign(len, val)|替换所有元素|
|l.assign(l2.begin(), l2.end())|替换所有元素为链表l2|
|l.swap(l2)|交换链表|
|l.merge(l2)|合并两个**有序**链表中的元素，调用后l2为空，可用greater<int>()|
|l.erase(it)|删除（区域中的）元素|
|l.remove(val)|删除值为val 的元素|

## 常见使用情况
* 需要快速插入删除。
* 节省存储空间
* 模拟双端队列
## 时间复杂度
* 插入删除$O(1)$
* 查找$O(n)$，$n$为链表长度