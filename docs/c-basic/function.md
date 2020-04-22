# 函数
## 定义
```
返回值类型 函数名(参数1类型 参数1名称, 参数2类型 参数2名称……)   
{
    语句组(函数体);
    返回值;
}
```
* **函数不允许嵌套定义**，但是允许嵌套使用。
```cpp
int sum(int a, int b)
{
    int ans = 0;
    ans = a + b;
    return ans;
}
```
* 一旦使用头文件后不可以定义库函数，比如，使用`<stdio.h>`后不能再自己定义`printf`函数。原则上，平常定义函数时也应尽量避免与库函数重名。
## 调用
`调用函数： 函数名（参数1, 参数2，……）`  
* 对函数的调用，也是一个表达式。函数调用表达式的值，由函数内部的`return`语句决定。  
* `printf`也是函数，它的声明在`<stdio.h>`头文件里。  

Q:  为什么大侠们使用绝招时都要先喊一声“降龙十八掌”之类的？  
A:  因为**函数调用之前要先声明**。  

```cpp
#include <stdio.h>
int sum(int a, int b); //声明
int main()
{
	int a = 1, b = 2;
	printf("%d\n", sum(a, b));
}
int sum(int a, int b)
{
	return a + b;
}
```
## 返回值
return语句语法如下：   
&emsp;`return 返回值；`  
* return语句的功能是**结束函数的执行，并将“返回值”作为结果返回**。“返回值”是常量、变量或复杂的表达式均可。  
* 如果函数返回值类型为`void`，return语句就直接写 `return ;`  
* 
```cpp
int a[] = {1, 1, 2, 3, 5, 8, 13};
void show(int n)
{
    for(int i = 0; i < n; i++) 
        printf("%d ", a[i]);
    return ;
}
int Max(int x,int y) // 求两个整型变量中的较大值
{
    if(x > y)
        return x;
    return y; // 除void外函数一定要有返回，返回意味着函数结束
}
```

## 全局变量与局部变量
* 在函数外部定义的变量称为全局变量，在函数内部定义的变量称为局部变量。
```cpp
#include <stdio.h>
int a[105]; //数组a-全局
int n; //n-全局
int muln(int x) //x-局部
{
    int t; //t-局部
    t = x * n;
    return t;
}
int main()
{
    scanf("%d", &n);
    for(int i = 0; i < n; i++) //i-局部
        scanf("%d", &a[i]);
}
```
### 全局变量
* 全局变量的作用域是从变量定义的位置开始到文件结束，可以在文件中位于全局变量定义后面的任何函数中使用。
* 过多地使用全局变量，会增加调试难度。因为多个函数都能改变全局变量的值。
* 全局变量在定义时默认初值为0。
### 局部变量
* 局部变量的作用域是在定义该变量的函数内部，局部变量只在定义它的函数内有效。
* 在不同的函数中局部变量名可以相同。
* 一个局部变量和一个全局变量是可以重名的，在相同的作用域内局部变量有效。但易出错！尽量避免使用。
* 在代码块中定义的变量的存在时间和作用域将被限制在该代码块中。如`for(int i; i<=n; i++) sum += i;`中的`i`是在该`for`循环语句中定义的，存在时间和作用域只能被限制在该for循环语句中。 
* 局部变量值是随机的，要**初始化初值**。
* 局部变量受栈空间大小限制，大数组需要注意。通俗地说，main函数里数组不能开很大（十万级别）。
## 函数参数的传递
原则： 用什么传什么
```
返回值类型 函数名(参数1类型 参数1名称, 参数2类型 参数2名称……) 
{
    函数体;
    返回值;
}
```
### 整型/实型变量作函数参数
```cpp
double dis(double x1, double y1, double x2, double y2)
{
    return sqrt((x1-x2)*(x1-x2) + (y1-y2)*(y1-y2));
}
int fun(double x)
{
    return x; //double -> int 类型转换
}
int Max(int a, int b)
{
    return a > b ? a : b;
}
```

### 一维数组作函数参数
写法如下： 
`函数类型 函数名(数组类型名 数组名[])`
不用写出数组的元素个数。
例如：
```cpp
void PrintArray(int a[]) 
{
   ...
}
int b[] = {1, 2, 3};
int main()
{
    PrintArray(b); //调用时只用写数组名
}
```
* 数组作为函数参数时是传址引用的，即形参数组改变了，实参数组也会改变。

### 二维数组作为函数的参数
* 二维数组作为形参时，必须写明数组有多少列，不用写明有多少行。
```cpp
void PrintArray(int a[][5]) 
{
    printf("%d", a[4][3]);
}
```

<br></br>
<br></br>

Contributed by [LyuLumos](https://github.com/LyuLumos)