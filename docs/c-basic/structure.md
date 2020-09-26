# 逻辑结构
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
for ( init; condition; increment ) {
   statement(s);
}
```
### while
```c
while ( condition ) {
   statement(s);
}
```
### do-while
```c
do {
   statement(s); // 语句至少会被执行一次
}while( condition );
```

### 嵌套循环
&ensp;&ensp;就是在循环里面循环...
```c
for(;;){
    while(条件){
        ...
    }
}
```
### 死循环

```c
while(true){}
for(;;)
```
### break&continue;
* break表示跳出循环
* continue表示结束本次循环，直接进行下一次循环，只能用于循环结构

例子：判断n是不是素数

```cpp
#include<stdio.h>
int main(){
	int n,flg=0;
	scanf("%d",&n);
	for(int i=2;i<n;i++){
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
输入 #1 
```
1 2 
2 3 
3 4 
```
输出 #1
```
3 
5 
7 
```
* C语言
EOF在C标准函数库中表示`文件结束符(end of file)`，在while循环中以EOF为文件结束标志。在命令行中输入Ctrl+z可以结束输入。
```cpp
#include<stdio.h>
int main(){
	int a,b;
	while(scanf("%d%d",&a,&b)!=EOF){
		printf("%d\n",a+b);
	}
	return 0;
} 
```
```cpp
#include<stdio.h>
int main(){
	int a,b;
	while(~scanf("%d%d",&a,&b)){
		printf("%d\n",a+b);
	}
	return 0;
} 
```

* c++
```c
#include<iostream>
using namepsace std;
int main(){
	int a,b;
	while(cin>>a>>b){
		cout<<a+b<<endl;
	}
	return 0;
} 
```
当题目是多组数据且处理到文件结束的情况下，一定要写EOF或者~(`推荐使用`)，不然可能会报错
#### T组样例输入输出
给出整数a和b的值，输出a+b，输入包含T组数据。输入的第一行为T，之后的T行分别是a和b的值。
输入 #1
```
2
1 2
2 3
```
输出 #1
```
3
5
```
代码：
```cpp
#include<stdio.h>
int main()
{
	int a,b,t;
	scanf("%d",&t);
	while(t--){
		scanf("%d%d",&a,&b);
		printf("%d\n",a+b);
	}
	return 0;
}
```
#### 以特殊输入作为结束标志
例如，给出正整数a和b的值，输出a+b，当a为0或者b为0时，结束输入。
输入 #1 
```
1 2 
2 3 
0 1
```
输出 #1
```
3 
5 
```
* 当a为0或者b为0时，输入结束。
```cpp
#include<stdio.h>
int main()
{
	int a,b;
	while(scanf("%d%d",&a,&b)){
		if(a==0||b==0)break;\\break在此处派上用场
		printf("%d\n",a+b);
	}
	return 0;
}
```

* 当a和b同时为0的的时候，输入结束。
输入 #1
``` 
2 0 
0 1
0 0
```
输出 #1
```
2
1
```
```cpp
#include<stdio.h>
int main()
{
	int a,b;
	while(scanf("%d%d",&a,&b)){
		if(a==0&&b==0)break;\\break在此处派上用场
		printf("%d\n",a+b);
	}
	return 0;
}
```