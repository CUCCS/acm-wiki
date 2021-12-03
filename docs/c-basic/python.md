# Python 竞赛基础

!!! note "Copyright"
    本页面贡献者：[YanhuiJessica](https://github.com/YanhuiJessica)，[LyuLumos](https://github.com/LyuLumos)。
    本页面内容遵循 MIT 协议，转载请附上原文出处链接和本声明。

!!! done "推荐"
    强烈推荐 [YanhuiJessica](https://github.com/YanhuiJessica) 的 [TemPro](https://github.com/YanhuiJessica/TemPro/tree/master/Templates) 仓库。包括我个人也是在揭师姐的影响下一步步成为Python选手（jdltxdy! :P）。珠玉在前，不甚完善之处还希望后人继续添砖加瓦。

## 注意

本文档仅面向于「使用 `Python3` 的程序设计竞赛选手」。我希望可以为 `C++转Python` 选手提供一份快速入门的文档，两种语言相同的操作大都不会被提及。

## 输入输出

### 输入
请参照 [YanhuiJessica/Tempro/Python](https://github.com/YanhuiJessica/TemPro/blob/master/Templates/Python.md)。

### 输出

Python 输出非常简单，只需要

```py
print(a)
```

对于 `C++转Python` 选手，也可使用类似于 `printf` 的输出形式，在保留指定位小数结果的题目中非常有用。

```py
print('The length of %s is %d' % (s,x))
```

## 类型，运算符和流程控制

### int

与 `C++` 最大的不同是，`Python` 中的整数**以字符串形式**存储，这代表可以认为它没有存储上限。同时定义变量时不需要声明类型。

```py
a = 100
```

### float 和 除法

`Python` 没有 `double` 类型，它的 `float` 类型与前者类似。请注意除法是自动转为浮点数的，请严格区分 `/` 与 `//`。除法操作 `/` 永远返回浮点类型，同时仍存在浮点误差。

```py
4 / 3 # 1.3333333333333333
4 // 3 # 1
0.1 + 0.2 # 0.30000000000000004
```

### bool

```py
# 请顺便注意if语句的写法
flag = True
if flag not False:
    print('Right')
elif xxx:
    # ...
else:
    # ...
```

### switch?

Python 中没有 `switch` 语句，但在 `Python 3.10 beta` 中增加了 `match-case` 的实现。目前仍然只建议使用多个 `if-else` 语句解决。

## 缩进
Python是缩进敏感的语言，不严格按照指定缩进语法会报错。

## 拷贝

Python和C++的另一个显著不同点是，对象之间的「赋值」操作会被解释器理解为「引用」。

```py
newlist = list1 # 引用，更改一个对象会导致另一个出现变化。
```

如果需要建立一个新的对象，与原有对象的基本属性一致（除了地址），则需要使用深拷贝。

```py
import copy
list2 = copy.copy(list1)
```

在某些结构里面也可以使用特殊操作达到拷贝的效果。

```py
list2 = list1[:]
```



## 常用数据结构与算法的映射

|C++|Python|备注|
|----|----|----|
|array/vector|list|严格意义上应该是array对应array, vector对应list，但是我们建议更多地使用list|
|tuple(C++11)|tuple|Python的tuple是不可修改的|
|map|dict|请注意Python对于不存在键的插入操作|
|set|set||
|stack|list||
|queue|queue||
|deque|collections.deque||
|priority_queue|heapq||
|sort|sorted||
|lower_bound/upper_bound|bisect||
|×|pow(x,y,mod)| Python自带快速幂|


两者操作大致是相同的，具体语法请自行学习。

## 推导式

合理使用推导式会让你的代码看起来更 `pythonic`，但也会略微降低可读性。

```py
nums = [i*i for i in range(1, 101) if i%3 == 0]
dict2 = {k:v for k,v in enumerate(dict1)}
# 一个可读性较低的反例
print('\n'.join([' '.join(['%2d*%2d=%2d' % (col, row, col * row) for col in range(1, row + 1)]) for row in range(1, 10)]))
```




## Python 的优缺点

大部分竞赛选手不用Python的原因在于Python的速度太慢。尽管 `PyPI` 可以加速Python代码的运行，我个人还是不建议拿纯Python去参加程序设计竞赛。

但是我们不能否认的是，在某些特定问题下，Python具有明显的编码时间优势。比如在处理高精度计算等问题上，合理使用Python会起到事半功倍的效果。

## 应用

下面展示简单的应用，用于解决 [2019ICPC徐州站网络赛A题](https://nanti.jisuanke.com/t/41383)，题目有一步需要使用拓展中国剩余定理，计算过程会超出 `long long` 范围。


??? note "C++, by LyuLumos"
    
    ```cpp
    #include<iostream>
    using namespace std;
    typedef __int128 ll;

    void exgcd(ll a, ll b, ll& g, ll& x, ll& y) {
        if (b == 0) {
            g = a;
            x = 1;
            y = 0;
            return;
        }
        exgcd(b, a % b, g, y, x);
        y -= (a / b) * x;
    }

    bool flag = false;
    ll a1, a2, n1, n2;

    ll abs(ll x) {
        return x > 0 ? x : -x;
    }

    void china() {
        ll d = a2 - a1;
        ll g, x, y;
        exgcd(n1, n2, g, x, y);
        if (d % g == 0) {
            x = ((x * d / g) % (n2 / g) + (n2 / g)) % (n2 / g);
            a1 = x * n1 + a1;
            n1 = (n1 * n2) / g;
        }
        else
            flag = true;
    }

    int n;
    long long as[100001];
    long long ns[100001];

    ll realchina() {
        a1 = as[0];
        n1 = ns[0];
        for (ll i = 1; i < n; i++) {
            a2 = as[i];
            n2 = ns[i];
            china();
            if (flag)
                return -1; //无解 
        }
        return a1;
    }

    int main() {
        cin >> n;
        flag = false;
        for (ll i = 0; i < n; i++)
            cin >> ns[i] >> as[i];
        cout << (long long)realchina() << endl; //不到输出不换int128 
    }
    ```



??? note "Python, by YanhuiJessica"
    
    ```py
    def gcd(a,b):
        if(b==0):
            return a
        else:
            return gcd(b,a%b)

    def exgcd(a,b,x,y):
        x,y=0,0
        if(b==0):
            x=1
            y=0
            return a,x,y
        d,x,y=exgcd(b,a%b,x,y)
        tmp=x
        x=y
        y=tmp-int(a/b)*y
        return d,x,y

    def inv(a,b):
        a,x,y=exgcd(a,b,a,b)
        while(x<0):
            x+=b
        return x

    if __name__ == '__main__':
        N,M=map(int,input().split())
        Divider=[0]
        Remainder=[0]
        for i in range(1,N+1):
            a,b=map(int,input().split())
            Divider.append(a)
            Remainder.append(b)
        flag=True
        for i in range(2,N+1):
            M1,M2=Divider[i-1],Divider[i]
            t1,t2=Remainder[i-1],Remainder[i]
            T=gcd(M1,M2)
            if((t2-t1)%T!=0):
                flag=False
                break
            Divider[i]=(M1*M2)//T
            Remainder[i] = (inv(M1//T, M2//T) * (t2-t1)//T) % (M2//T) * M1 + t1
            Remainder[i] = (Remainder[i] % Divider[i] + Divider[i]) % Divider[i]
        if(flag):
            ans=Remainder[N]
        else:
            ans=-1
    ```

上面C++的代码无法进行超出 `__int128` 范围的计算，需要进行手写高精度才能实现与Python相同的效果。
