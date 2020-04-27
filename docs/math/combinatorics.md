# 组合数学

## 组合数的定义
从 $n$ 个不同元素中，任取 $m$ ( $m\leq n$ ) 个元素组成一个集合，叫做从 $n$ 个不同元素中取出 $m$ 个元素的一个组合；从 $n$ 个不同元素中取出 $m$ ( $m\leq n$ ) 个元素的所有组合的个数，叫做从 $n$ 个不同元素中取出 $m$ 个元素的组合数。用符号 $\mathrm C_n^m$ 来表示。

数学上用 $\displaystyle \binom{n}{m}$ 表示 $C_n^m$

组合数计算公式

$$ \displaystyle \binom{n}{m}= \frac{\mathrm A_n^m}{m!} = \frac{n!}{m!(n - m)!} $$



## 性质
在此介绍一些组合数的性质。

$$ \binom{n}{m}=\binom{n}{n-m} （对称性）$$


$$ \binom{n}{k} = \frac{n}{k} \binom{n-1}{k-1} （定义）$$

$$ \binom{n}{m}=\binom{n-1}{m}+\binom{n-1}{m-1} （递推式）$$


$$ \binom{n}{0}+\binom{n}{1}+\cdots+\binom{n}{n}=\sum_{i=0}^n\binom{n}{i}=2^n （横向求和）$$


$$ \sum_{i=0}^n(-1)^i\binom{n}{i}=0 （带权横向求和）$$



$$ \sum_{l=0}^n\binom{l}{k} = \binom{n+1}{k+1}（斜向求和）$$

$$ \sum_{i=0}^m \binom{n}{i}\binom{m}{m-i} = \binom{m+n}{m}\ \ \ (n \geq m)$$

$$ \binom{n}{r}\binom{r}{k} = \binom{n}{k}\binom{n-k}{r-k}$$

$$ \sum_{k=1}^m\binom{m}{k}\binom{n}{k}=\binom{m+n}{m}$$

$$ \sum_{i=0}^n\binom{n-i}{i}=F_{n+1}，F_n是斐波那契数列第n项 $$



# 求组合数
### 引理：由卢卡斯定理

$$\large\tbinom{sp+q}{tp+r} = \tbinom{s}{t}\tbinom{q}{r} (mod \,p)，p为素数$$

则有

$$\large\tbinom{n}{m}\, mod \,p=\tbinom{n/p}{m/p}\tbinom{n \,mod\, p}{m \,mod\, p}\, mod\, p$$

复杂度$O(log_pn\cdot p)$，打表复杂度可降至$O(log_pn+p)$



### 实现
#### 根据定义实现  
   
复杂度$O(n)$。由于分子过大，int范围内只能计算到C(29,15)，如果只用阶乘只能计算到C(20,10)。



#### 杨辉三角打表  

复杂度$O(n^2)$
```cpp
for(i=0;i<n;i++)
    a[i][0]=a[i][i]=1;
for(i=2;i<n;i++)
    for(j=1;j<i;j++)
        a[i][j]=a[i-1][j-1]+a[i-1][j];
```
   特别注意：C(30,15)=155117520，C(64,32)=1.83x10^18



#### 卢卡斯定理

p是小素数（1e5）时使用。
```cpp
ll qpow(ll a,ll n);
ll C(ll n,ll m){
    if(n<m) return 0;
    if(m>n-m) m=n-m;
    ll a=1,b=1;
    for(int i=0;i<m;i++){
        a=(a*(n-i))%p;
        b=(b*(i+1))%p;
    }
    return a*qpow(b,p-2)%p; //费马小定理求逆元
}
ll Lucas(ll n,ll m){
    if(m==0) return 1;
    return Lucas(n/p,m/p)*C(n%p,m%p)%p;
}
```



#### 预处理阶乘逆元表。
使用定义式 `C(n,m)=n!/(m!*(n-m)!)`。 
 
(1) 用O(n)的时间预处理逆元表 inv[n]。 


(2) 预处理阶乘表  `fac[n]=(fac[n-1]*n)%p=(n!)%p`。  



(3) 预处理阶乘的逆元表 `invfac[n]=(invfac[n-1]*inv[n])%p`。

计算过程：`C(n,m)=(fac[n]*(invfac[m]*invfac[n-m]%p))%p`。  

注意：需要两次取模防止溢出。



## 应用
### 加法原理和乘法原理
加法原理  
完成一个工程可以有 $n$ 类办法， $a_i(1 \le i \le n)$ 代表第 $i$ 类方法的数目。那么完成这件事共有 $S=a_1+a_2+\cdots +a_n$ 种不同的方法。

乘法原理  
完成一个工程需要分 $n$ 个步骤， $a_i(1 \le i \le n)$ 代表第 $i$ 个步骤的不同方法数目。那么完成这件事共有 $S = a_1 \times a_2 \times \cdots \times a_n$ 种不同的方法。
