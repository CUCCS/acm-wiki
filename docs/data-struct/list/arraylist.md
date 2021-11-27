# 数组表

!!! note "Copyright"
    本页面贡献者：[DodoZhang](github.com/DodoZhang)。
    本页面内容遵循 MIT 协议，转载请附上原文出处链接和本声明。

## 简介
**数组表**（Array List）是一种基于**数组**实现的**列表**。数组表拥有极高的随机读取效率，但是插入或删除特定元素效率较低。  
在 STL 中，`std::vector`就是数组表。

## 性能
性能指该数据结构在进行某种操作时的时间复杂度。  
对于一个长度为`n`的数组表：

|操作|时间复杂度|
|:--:|:--:|
|随机查询|$O(1)$|
|顺序查询|$O(1)$|
|向后追加|$O(1)$|
|在第m位插入|$O(n-m)$|
|删除末项|$O(1)$|
|删除第m位|$O(n-m)$|

## 成员变量
成员变量指该数据结构中用于储存数据的变量。  
`a`: `int *` 用于储存列表数据的数组；  
`cap`: `int` 数组表中数组的长度；  
`len`: `int` 数组表中元素的个数。

## 参数
通过调整参数，可以一定程度的影响一些操作的效率。  
`init_cap` : `int` 数组表的数组在构造时的初始长度。

## 方法
#### 构造函数
```cpp
ArrayList()
{
    a = (int*) malloc(init_cap * sizeof(int));  //初始化储存数据的数组
    cap = init_cap;
    len = 0;
}
```

#### 析构函数
```cpp
~ArrayList()
{
    free(a);
}
```

#### 获取元素的值
```cpp
int get(int index)
{
    return a[index];
}
```

#### 设置元素的值
```cpp
void set(int index, int value)
{
    a[index] = value;
}
```

#### 获取长度
```cpp
int length()
{
    return len;
}
```

#### 扩容
在数组表容量不足时，需要进行扩容。
```cpp
void grow()
{
    cap += cap >> 1;  //将容量扩充为原来的1.5倍
    a = realloc(a, cap * sizeof(int));
}
```

#### 向后追加元素
!!! Warning "Warning"
    在向后追加时要注意，如果数组表的长度已经等于数组表的容量，需要对数组表进行扩容，否则会导致越界。

```cpp
void append(int value)
{
    if (len >= cap) grow();
    a[len ++] = value;
}
```

#### 删除末端元素
不需要真正意义的删除最后一个元素，只需要标记其不存在即可。
```cpp
void removeLast()
{
    len --;
}
```

#### 插入元素

!!! Warning "Warning"
    在插入时同样要注意扩容。
    
```cpp
void insert(int index, int value)
{
    if (len >= cap) grow();
    memmove(a + index, a + index + 1, len - index);
    a[index] = value;
    len ++;
}
```

#### 删除末端元素
```cpp
void remove()
{
    len --;
    memmove(a + index + 1, a + index, len - index);
}
```