!!! note "Copyright"
    本页面贡献者：[zrz](https://github.com/BehindShadow)。
    本页面内容遵循 MIT 协议，转载请附上原文出处链接和本声明。
    
> 详细请见算法竞赛进阶指南P41页

在RMQ问题(区间最值问题)，ST表是基于倍增方法，实现了区间查询$O(1)$**优于线段树，树状数组等查询**，但是不能实现区间修改或者单点修改
基本是预处理一个二维数组，复杂度$O(nlogn)$

ps : 下面代码给出的Log数组是求出$log_2n$的所有值，速度上优于c++内部的log函数

预处理模版：
```cpp
int f[maxn][30];//第二维是一个长度范围，一般开到30就够大了，例如f[i][j]代表从i开始长度为2^j区间的最值
int Log[maxn];
void inits(){
    Log[1]=0;
    for(int i=2;i<=n;i++)
        Log[i]=Log[i/2]+1;
}
void RMQ_ST(){
    for(int i = 1;i <= n; i++){
        f[i][0] = a[i];
    }
    int t =Log[n]+1;        //预处理Log数组要比自带的log2快一些，如果要进行大量的区间查询，最好是预处理一下
    // int t = log2(n)+1;
    for(int j = 1;j < t;j++){
        for(int i = 1;i<= n-(1<<j)+1;i++){
            f[i][j] = max(f[i][j-1],f[i+(1<<(j-1))][j-1]);
        }
    }
}
```

查询模版：
复杂度$O(1)$
```c++
int kk = Log[x-i+1];//同样是使用预处理数组
mx = max(f[i][kk],f[x-(1<<kk)+1][kk]);
```