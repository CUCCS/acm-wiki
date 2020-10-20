# 逻辑结构

!!! note "Copyright"
    本页面贡献者：[LyuLumos](https://github.com/LyuLumos), [Zhang1933](https://github.com/Zhang1933)。
    本页面内容遵循 MIT 协议，转载请附上原文出处链接和本声明。

## 程序设计基本思路
输入 --> 计算处理 --> 输出  

## 基本结构
```cpp
#include <stdio.h> // 头文件

int main() // 主函数
{
    printf("Hello, world!"); // 调用 printf 函数
    return 0; // 返回0，主函数不能返回其他值
}
```
## 顺序结构
## 选择结构
### if
```cpp
    if(条件表达式) 
    {
        语句1;
        语句2;
    }
    else if(条件表达式) 
    {
        语句3;
    }
    else 
    {
        语句4;
    }
```
### switch
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
## 循环结构
### for
```c
for ( init; condition; increment ) 
{
   statement(s);
}
```
### while
```c
while ( condition ) 
{
   statement(s);
}
```
### do-while
```c
do 
{
   statement(s); // 语句至少会被执行一次
}while( condition );
```

### 嵌套循环
&ensp;&ensp;就是在循环里面循环。
```c
for(init; condition; increment)
{
    while(condition)
    {
        statement(s);
    }
}
```
### 死循环
表示不会终止的循环。
```c
while(true){}
for(;;)
```
### break&continue;
* break表示跳出循环；
* continue表示结束本次循环，直接进行下一次循环，只能用于循环结构。

例：判断n是否为素数

```cpp
#include<stdio.h>
int main()
{
	int n,flg=0;
	scanf("%d",&n);
	for(int i=2;i<n;i++)
    {
		if(n%i!=0)continue;//不是因子继续循环
		flg=1;break;//跳出循环
	}
	if(flg==1)printf("NO\n");
	else printf("YES\n");
	return 0;
} 
```
## 算法竞赛中的输入输出
#### 处理到文件结束
例如，给出整数a和b的值，输出a+b，输入包含多组数据，处理到文件结束。

* C语言
EOF在C标准函数库中表示`文件结束符(end of file)`，在while循环中以EOF为文件结束标志。在命令行中输入Ctrl+z可以结束输入。
```cpp
#include<stdio.h>
int main()
{
	int a,b;
	while(scanf("%d%d",&a,&b)!=EOF)
	//或使用 while(~scanf("%d%d",&a,&b))
    {
		printf("%d\n",a+b);
	}
	return 0;
} 
```

* C++
```cpp
#include<iostream>
using namepsace std;
int main()
{
	int a,b;
	while(cin>>a>>b)
    {
		cout<<a+b<<endl;
	}
	return 0;
} 
```
当题目是多组数据且处理到文件结束的情况下，一定要写EOF或者~(`推荐使用`)，不然可能会报错。
#### T组样例输入输出
给出整数a和b的值，输出a+b，输入包含T组数据。输入的第一行为T，之后的T行分别是a和b的值。

```cpp
#include<stdio.h>
int main()
{
	int a,b,t;
	scanf("%d",&t);
	while(t--)
    {
		scanf("%d%d",&a,&b);
		printf("%d\n",a+b);
	}
	return 0;
}
```
