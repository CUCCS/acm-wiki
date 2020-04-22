## 基本格式
```c
type arrayName[arraySize];
```
## 一维数组
```c
int a[1005];
int b[3] = {1, 2, 3};
int c[] = {1, 2, 3};
scanf("%d", &a[0]);
printf("%d", c[3]); // 越界
```
## 二维数组
```c
double mp[105][105];
```
## 字符数组
```c
char d[105] = {'\0'};
char parr[] = "zifushuzu";
char charr[] = { 'z','i','f','u','s','h','u','z','u' };
```
* 读入时注意回车与空格