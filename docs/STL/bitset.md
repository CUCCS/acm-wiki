## 用途
类似数组的结构，它的每一个元素只能是０或１，每个元素仅用１bit空间。用它存储数据回比用数组省空间。

使用需添加
```cpp 
#include <bitset>
```
## 基本操作
### 定义
```cpp
bitset<length> name;
```
即定义有length位的bitset。
### 元素访问
#### 通过下标访问某一位
左边位低位，右边为高位。
```cpp
#include <iostream>
#include <bitset>
using namespace std;
int main()
{
    bitset<8> bit(25);
    cout << bit[0] << endl;
}
```
输出
```cpp
1
```
#### 直接输出全部位数
```cpp
#include <iostream>
#include <bitset>
using namespace std;
int main()
{
    bitset<8> bit(25);
    cout << bit << endl;
}
```
输出：
```cpp
00011001
```
#### 构造函数
|构造函数|作用  |
|--|--|
| bitset< length > bit | 无参构造函数，默认全为0 |
|bitset< length > bit(string)|将string转换成二进制，要求string必须由01构成，长度不足定义长度时，按照放在低位|
|bitset< length > bit( char[] ) | 同上 |
|bitset< length > bit(val)|将val转化成二进制放在bitset中| 

### 常用函数
|函数|作用  |
|-----------------|--|
bit.size()       |返回大小（位数）
bit.count()  |   返回1的个数
bit.any()   |    返回是否有1
bit.none()|      返回是否没有1
bit.set() |      全都变成1
bit.set(p)|      将第位置p变成1
bit.set(p, x)|   将位置p变成x
bit.reset() |    全都变成0
bit.reset(p)|    将位置p变成0
bit.flip()   |   全都取反
bit.flip(p) |    将位置p取反
bit.to_ulong() | 返回它转换为unsigned long的结果，如果超出范围则报错
bit.to_ullong()| 返回它转换为unsigned long long的结果，如果超出范围则报错
bit.to_string()| 返回它转换为string的结果

## 常见使用情况
* 二进制相关操作
*  节省空间