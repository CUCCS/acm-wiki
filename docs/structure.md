# 程序设计基本思路
输入 --> 计算处理 --> 输出  

# 基本结构
```cpp
#include <stdio.h> // 头文件

int main() // 主函数
{
    printf("Hello, world!"); // 调用 printf 函数
    return 0; // 返回0，主函数不能返回其他值
}
```
# 顺序结构
# 选择结构
## if
```cpp
    if(条件表达式) {
        语句1;
        语句2;
    }
    else if(条件表达式) {
        语句3;
    }
    else {
        语句4;
    }
```
## switch
```cpp
switch(表达式)
{
    case 常量表达式1：
        语句1；
        break;
    case 常量表达式2：
        语句2；
        语句3；
        break;
    ...
    default: //可省略
        语句n；
}
```
* 不要忘记```break```和每种情况后的```:```
* ```switch```语句的对象只能是```int``` ```char``` ```bool```类型的数据
# 循环结构
## for
```c
for ( init; condition; increment ) {
   statement(s);
}
```
## while
```c
while ( condition ) {
   statement(s);
}
```
## do-while
```c
do {
   statement(s); // 语句至少会被执行一次
}while( condition );
```

## 嵌套循环
## 死循环