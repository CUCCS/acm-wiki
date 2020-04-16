# 数论

## 目标

**复习 + 拓展**

## 质数

### 素数间隔-Prime gap

- The first 60 prime gaps are: 1, 2, 2, 4, 2, 4, 2, 4, 6, 2, 6, 4, 2, 4, 6, 6, 2, 6, 4, 2, 6, 4, 6, 8, 4, 2, 4, 2, 4, 14, 4, 6, 2, 10, 2, 6, 6, 4, 6, 6, 2, 10, 2, 4, 2, 12, 12, 4, 2, 4, 6, 2, 10, 6, 6, 6, 2, 6, 4, 2
- 有无穷对素数，之间存在着一定的间隔。间隔从被证明为7000万以内，一直到如今的246。如果该常数改进到2，相当于证明孪生素数猜想
- 素数之间间隔可以有多远，[The 80 known maximal prime gaps](https://en.wikipedia.org/wiki/Prime_gap)  

### 素数定理-Prime Number Theorem

- 质数分布密度
- 不超过x的质数的总数π(x)近似于x/ln(x)
- π(2)=1，π(3.5)=2，π(10)=4
  
  <img src="https://cdn.britannica.com/14/77714-004-D28E8805/Prime-number-theorem.jpg">

### 素数筛法

- 求$1$到$n$之间内的所有素数
  
  | 方法 | 时间复杂度 |
  | ---- | --------- |
  | $sqrt(n)$的判别 | $O(n*sqrt(n))$ |
  | 普通筛 / 埃氏筛法 | $O(nloglogn)$ |
  | 线性筛 / 欧拉筛法 | $O(n)$ |

### 素数测试-Miller Rabin

- 哥德巴赫猜想：任何大于2的偶数都能够写成两个质数相加的形式
- 当题目给出的偶数达到$10^{18}$，此时的质数可能非常大，用上述的筛法可能会超时，用Miller Rabin快速判断一个$<2^{63}$的数是不是素数
- 时间复杂度：$O(klog_2(n))$，$n$为检测的数值，$k$为自己设定的检测的次数
- 不确定算法，单次测试有不超过$\frac{1}{4}$的概率会将一个合数误判为一个素数

#### 依据

- $费马小定理：若p是质数，则对于任意0<a<p，有a^{p−1}≡1(modp)$
- $二次探测定理：若p是质数，且x^2≡1(modp)，那么x≡1 (modp)和x≡p−1(modp)中的一个成立$
  
#### 算法过程
  
1. 偶数、0、1、2直接判断
2. 假设要测试的数为$n$，选取整数$r$和奇数$d$，满足$n-1=2^rd$
3. 选取$a \in (1,...,n-1)$
4. 如果$a^d=1(modn)$或者$a^d=n-1(modn)$，即满足二次探测定理，则调回Step 3继续验证
5. 对于$i=0,...,r-1$，验证$a^{2^id}$是否满足$a^{2^id}=n-1(modn)$，满足则跳回Step 3继续验证，不满足则$n$为合数
6. 经过$k$次验证后，$n$可能是素数
7. 实验的伪码如下所示：
    ```
    Input #1: n > 3, an odd integer to be tested for primality
    Input #2: k, the number of rounds of testing to perform
    Output: “composite” if n is found to be composite, “probably prime” otherwise

    write n as 2^r·d + 1 with d odd (by factoring out powers of 2 from n − 1)
    WitnessLoop: repeat k times:
    pick a random integer a in the range [2, n − 2]
    x ← a^d mod n
    if x = 1 or x = n − 1 then
        continue WitnessLoop
    repeat r − 1 times:
        x ← x^2 mod n
        if x = n − 1 then
            continue WitnessLoop
    return “composite”
    return “probably prime”
    ```
- [video tutorial](https://www.youtube.com/watch?v=qdylJqXCDGs)

#### Miller Rabin模板

```cpp
typedef unsigned long long ll;
//typedef long long ll;
//ll*ll可能会溢出，所以乘法化加法
/* *************************************************
* Miller_Rabin 算法进行素数测试
* 速度快可以判断一个 < 2^63 的数是不是素数
*
**************************************************/
#include<time.h>
#include<stdlib.h>
const int S = 8; //随机算法判定次数一般 8～10 就够了
// 计算 ret = (a*b)%c a,b,c < 2^63
ll mult_mod(ll a,ll b,ll c){
	a%=c;
	b%=c;
	ll ret=0;
	ll tmp=a;
	while(b){
		if(b&1){
			ret+=tmp;
			if(ret>c)ret-=c;//直接取模慢得多 
		}
		tmp<<=1;
		if(tmp>c)tmp-=c;
		b>>=1;
	}
	return ret;
}
// 计算 ret = (a^n)%mod
ll pow_mod(ll a,ll n,ll mod){
	ll ret=1;
	ll tmp=a%mod;
	while(n){
		if(n&1)ret=mult_mod(ret,tmp,mod);
		tmp=mult_mod(tmp,tmp,mod);
		n>>=1;
	}
	return ret;
}
// 通过 a^(n-1)=1(modn)来判断 n 是不是素数
// n - 1 = x * (2^t)
// 中间使用二次判断
// 是合数返回 true, 不一定是合数返回 false
bool check(ll a,ll n,ll x,ll t){
	ll ret = pow_mod(a,x,n);
	ll last = ret;
	for(int i = 1;i <= t;i++){
		ret = mult_mod(ret,ret,n);
		if(ret == 1 && last != 1 && last != n-1)return true;//合数
		last = ret;
	}
	if(ret != 1)return true; // 费马小定理
	else return false;
}
//**************************************************
// Miller_Rabin 算法
// 是素数返回 true,(可能是伪素数)
// 不是素数返回 false
//**************************************************
bool Miller_Rabin(ll n){
	if( n < 2)return false;
	if( n == 2)return true;
	if( (n&1) == 0)return false;//偶数
	ll x = n - 1;
	ll t = 0;
	while( (x&1)==0 ){x >>= 1; t++;}
	
	srand(time(NULL));/* *************** */
	
	for(int i = 0;i < S;i++){
		ll a = rand()%(n-1) + 1;
		if( check(a,n,x,t) )
			return false;
	}
	return true;
}
```

### 大数的质因子分解-Pollard-Rho

- $O(n^{\frac{1}{4}})$的期望时间复杂度内计算合数$n$的某个非平凡因子(平凡因子指$1$和$n$，非平凡因子指$x \in [2,n-1]，n mod x=0$)
- 试除法：$n$的因数对称分布，遍历区间$[1,\sqrt N]$，时间复杂度为$O(\sqrt N)$
- 不直接寻找因子，而是寻找因子的倍数，然后通过GCD找到因子本身

#### 思路

- 对于$N⩾10^{18}$，使用**随机算法**-猜因数
- 组合随机采样-生日悖论：满足答案的组合比单个个体要多一些
  - 假如一个班上有$k$个人，如果找到一个人的生日是x月x日，这个概率会相当低；如果想找两个生日相同，当$k=23$，两个人在同一天生日的概率至少有$50\%$，$k=60$时，生日有重复的现象的概率$\text{P}(k) ≈0.9999$
- 最大公约数一定是某个数的约数。通过选择适当的$k$使得$\gcd(k,n)>1$，则求得的$\gcd(k,n)$是$n$的约数。则选取一组数$x_1,x_2,x_3,...x_n$，若有$gcd(|x_i-x_j|,n)>1$，则称$gcd(|x_i-x_j|,n)$是$n$的一个因子。
- 构造一个**伪随机数序列**，然后取相邻的两项来求gcd。Pollard设计了一个函数: $f(x)=(x^2+c)\mod N$ 其中c是一个随机的常数。选取$x_1$，令$x_2=f(x_1),x_3=f(x_2),...,x_i=f(x_{i-1})$
- Floyd判圈。在一定范围内，这个数列是随机的；但也有死循环的情况。龟兔赛跑：兔子比乌龟快一倍，同起点同时开始，当兔子“追上”乌龟时，兔子一定跑了刚好一圈。
  
  <img src="https://img-blog.csdn.net/20150503163841553" width=50%>

6. [brent判环(更高效)](https://www.cnblogs.com/book-book/p/6349362.html)

#### kuangbin的模板

```cpp
//**************************************************
// Miller_Rabin 算法
// 是素数返回 true,(可能是伪素数)
// 不是素数返回 false
//**************************************************
bool Miller_Rabin(ll n){
	if( n < 2)return false;
	if( n == 2)return true;
	if( (n&1) == 0)return false;//偶数
	ll x = n - 1;
	ll t = 0;
	while( (x&1)==0 ){x >>= 1; t++;}
	
	srand(time(NULL));/* *************** */
	
	for(int i = 0;i < S;i++){
		ll a = rand()%(n-1) + 1;
		if( check(a,n,x,t) )
			return false;
	}
	return true;
}
//**********************************************
// pollard_rho 算法进行质因素分解
//*********************************************
ll factor[100];//质因素分解结果（刚返回时时无序的）
int tol;//质因素的个数，编号 0～tol-1
ll gcd(ll a,ll b){
	ll t;
	while(b){
		t = a;
		a = b;
		b = t%b;
	}
	if(a >= 0)return a;
	else return -a;
}
//找出一个因子
ll pollard_rho(ll x,ll c){
	ll i = 1, k = 2;
	srand(time(NULL));
	ll x0 = rand()%(x-1) + 1;
	ll y = x0;
	while(1){
		i++;
		x0 = (mult_mod(x0,x0,x) + c)%x;//不断调整x2 
		ll d = gcd(y - x0,x);
		if( d != 1 && d != x)return d;//找到因子，返回 
		if(y == x0)return x;//判圈 出现循环，返回 
		if(i == k){y = x0; k += k;}
	}
}
// 对 n 进行素因子分解，存入 factor. k 设置为 107 左右即可
// 如果n 本身就是素数，那么将 n 存放在 factor 便可结束并返回
// 如果 n 不是素数，那么通过 pollard_rho()函数 找到 n 的一个因子 p(不一定是素因子)，递归 findFac(p)和 findFac(n/p)
void findfac(ll n,int k){
	if(n == 1)return;//递归出口 
	if(Miller_Rabin(n))
	{
		factor[tol++] = n;
		return;
	}
	ll p = n;
	int c = k;
	//值变化，防止死循环
	while( p >= n) // 改变常数c，不断找因子，返回n说明没找到 
		p = pollard_rho(p,c--);
	findfac(p,k);
	findfac(n/p,k);
}
```

- [洛谷 P4718【模板】Pollard-Rho算法](https://www.luogu.com.cn/problem/P4718) 
  - TLE-题解的博客中有一步步优化的过程

## 欧拉降幂

### 背景

- 给三个正整数，$a,m,b$，需要求：$a^b mod m$
- 数据范围：$1\le a \le 10^9，1\le b \le 10^{20000000}，1\le m \le 10^8$
- **指数爆炸**

### 理论依据

- 欧拉定理：$a^{\varphi(p)}≡1 \ mod \ p，a和p互质$
- 拓展欧拉降幂
  
  $a^b\equiv \begin{cases} a^{b\bmod\varphi(p)},&\gcd(a,p)=1\\ a^b,&\gcd(a,p)\ne1,b<\varphi(p)\\ a^{b\bmod\varphi(p)+\varphi(p)},&\gcd(a,p)\ne1,b\ge\varphi(p) \end{cases} \pmod p$

- 假设$k=\frac{b}{\varphi(p)},h=bmod\varphi(p)$，则$a^b=a^{k*\varphi(p)+h}=(a^{\varphi(p)})^k*a^h=a^h(modp)$
- [欧拉降幂公式的证明](https://blog.csdn.net/weixin_38686780/article/details/81272848)

### 代码模板

- 求单个数的欧拉函数
    ```cpp
    long long eular(long long n){
        long long ans = n;
        for(int i = 2;i*i <= n;i++){
            if(n % i == 0){
                ans -= ans/i;
                while(n % i == 0)n /= i;
            }
        }
        if(n > 1)ans −= ans/n;
        return ans;
    }
    ```
- 拓展欧拉函数
  - 以字符串形式读入大数，处理得到$bmod\varphi(p)$
  - 需要判断$b$和$\varphi(p)$的大小，否则会出错
  - [洛谷 P5091 模板题](https://www.luogu.com.cn/problem/P5091)
  
    ```cpp
    // char b[maxn]
    ll ans=0;
    c=eular(p);
    ll len = strlen(b);
    for(ll i = 0;i < len; i++)
	    ans = (ans*10 + b[i]-'0')%c;
    ans+=p;
    // 快速幂计算 a是底数，ans是指数，p是模数
    qPow(a,ans,p);
    ```
### 欧拉函数常用性质和公式

- $对于质数p，\varphi(p)=p-1$
- $\sum_{d|n}\varphi(d)=n\quad，包括1和n本身$
- $\sum_{gcd(d,n)==1}d=\varphi(n)*n/2$
- $\sum_{i=1}^{n-1}gcd(i,n)=\sum_{d|n}d\varphi(n|d)$
- $若p为质数，n=p^k,\varphi(n)=p^k-p^{k-1}$
- $积性性质：若m,n互质，\varphi(m*n)=\varphi(m)*\varphi(n)$

## RSA：公钥密码算法

### 算法流程

- $随机选择两个不相等的质数p和q$
- $计算p和q的乘积n$
- $计算n的欧拉函数φ(n)=(p-1)(q-1)$
- $随机选择一个整数e，满足1<e<φ(n)，且e与φ(n)互质$
- $求出整数d，使得ed ≡ 1 (mod φ(n))$
- $(n,e)为公钥，d为私钥$
- $加密：明文消息为m，满足0<m<n，计算密文c=m^e mod n$
- $解密：接受到密文消息为c，解密明文消息m=c^d mod n$

### 知识点

- 求逆元(欧拉定理/拓展欧几里得) + 快速幂
  
  > 若(a*x)%mod=1，则x是正整数a在模mod下的逆元 

  | 方法 | 限定 | 时间复杂度 |
  | ---- | ---- | -------- |
  | 线性打表法 | 只要求mod是质数 | O(n) |
  | 费马小定理 | mod是质数且与a互质，快速幂优化 | O(log(n)) |
  | 欧拉定理 | 只要求a与mod互质，需要欧拉函数与快速幂 | O(sqrt(n)+log(n)) |
  | 拓展欧几里德 | 只要求a与mod互质 | O(log(n)) |

## 拓展中国剩余定理

### 中国剩余定理

- 韩信点兵，三个三个一排少1个人，五个五个一排又少1个人，七个七个一排还少1个人
  
  > $对于一组同余方程$
  >
  > $x≡a_1 (mod n_1)$
  >
  > $x≡a_2 (mod n_2)$
  >
  > $...$
  >
  > $x≡a_k (mod n_k)$
  > 
  > $模数n_1,n_2...n_k两两互质，求最小的x$

1. $计算N=n_1×n_2×⋯×n_k$
2. $对于i=1,2,…,k，计算y_i=\frac{N}{n_i}=n_1n_2...n_{i-1}n_{i+1}...n_k$ 
3. $对于i=1,2,…,k，计算z_i=y_i^{-1}(modn_i)，即计算y_i在模n_i下的逆元$
4. $x=\sum_{i=1}^ka_iy_iz_i，最后计算x=x(modN)得到结果$

### 模数两两不互质

- 思路

  1. $通过先解出前两个方程的解，如将前两个方程$
  2. $x≡a_1 (mod n_1)，x≡a_2 (mod n_2)化为x≡A(mod N)$
  3. $将此方程和x≡a_3 (mod n_3)$
  4. $继续联立求解，直到最后一个方程解完为止$

- [洛谷 P4777 模板题](https://www.luogu.com.cn/problem/P4777)
- 求解
  
    > $x≡a_1 (mod n_1)$
    > 
    > $x≡a_2 (mod n_2)$

    - $可化为 x=a_1+k_1*n_1 ①; x=a_2+k_2*n_2;$
    - $消x，可得a_1+k_1*n_1=a_2+k_2*n_2$ 
        
      $移项得到k_1*n_1+(-k_2)*n_2=a_2-a_1$
    - $令d=a_2-a_1, x=k_1, y=-k_2;$
    
      $上式化为 x*n_1+y*n_2=d ③$
    - $令g=gcd(n_1,n_2),用拓展欧几里得解线性方程$
    
      $(此处求解x_1，y_1)，x_1*n_1+y_1*n_2=g$
    - $③式可化为  x_1*(d/g)*n_1+y_1*(d/g)*n_2 = g*(d/g)$
    - $即x=x_1*(d/g)=k_1 ; y=y_1*(d/g)=-k_2;$
    
      $即k_1=x_1*(d/g); k_2=-y_1*(d/g)$
    - $一组通解为 k_1=k_1+(n_2/g)*T; k_2=k_2-(n_1/g)*T$
    - $要求使所求得的解最小且为正整数，$
    
      $则可以根据 k_1的通解形式求得(消掉T的影响)$
    - $k_1=(k_1 mod (n_2/g)+(n_2/g)) mod (n_2/g) ②，$
    
      $即k_1=((x_1*(d/g)) mod (n_2/g)+(n_2/g)) mod (n_2/g)$
    - $将求出的k_1带入①，可得x的解，$
    
      $作为下一次的A，N为lcm(n1,n2)，$
    
      $即A为合并后的a，N为合并后的n$

### 模板[不唯一]

```cpp
typedef long long ll;
ll exgcd(ll a,ll b,ll &x,ll &y){
    if(a==0&&b==0)return -1;
    if(b==0){x=1;y=0;return a;}
    ll d=exgcd(b,a%b,y,x);
    y-=a/b*x;
    return d;
}
ll excrt(){
    ll a1=b[0],n1=a[0],a2,n2,d,x,y,gcd;//余数 b[] 除数 a[]
    // 返回的是最小非负整数解，有些题目需要特判
    //若当余数为0的时候 题目要求求正整数 所以0不算在内，应该加上下面的注释，即余数等于除数，同理后面的板子
    //if(a1==0)a1=a[0]
    for(int i=1;i<n;i++){
        a2=b[i];n2=a[i];
        d=a2-a1;
        gcd=exgcd(n1,n2,x,y);
        if(d%gcd)return -1;
        x=((x*d/gcd)%(n2/gcd)+(n2/gcd))%(n2/gcd);
        a1=x*n1+a1;
        n1=n1*n2/gcd;
    }
    return a1;
}
```

## 定理&猜想&公式

- 费马大定理：
  
  $当整数n >2时，$

  $关于x, y, z的方程 x^n + y^n = z^n 没有正整数解$
- 实数域不可拆分多项式：
  
  $一次多项式和二次多项式(b^2<4ac)$
  - 艾森斯坦因判别法：有理数域不可约，即一定要整数解
- 勾股数
  - 任意大于2的整数都可以找出另外两个数构成勾股数
  - 本原勾股数
- 四色猜想
- 康威常数
- 日期转化成星期
  - 蔡勒公式
  - 基姆拉尔森计算公式
- 斯特林公式 - 阶乘