# 尺取

首先，尺取法就是形如一把尺子的方法，去一块一块的截取你所需要的序列。<br/><br/>
> 给你一个n和s，然后给出n个数，求这n个数中和大于等于s的最小连续序列。_<br/><br/>

看一下一组数据<br/>
10 15<br/>
5 1 3 5 10 7 4 9 2 8<br/><br/>
在不考虑时间的情况下可以这样干
```cpp
for(l=1;l<=n;l++)            //>从左边第一个数开始，取区间下限l
    {
        for(r=l;l<=n;l++)        //在区间下限右边取区间上限r
        {
            check(l,r);          //判断区间[l,r]中数的和是否大于等于s,是就和最小长度比较。
            if(check) minlen=min(minlen,r-l+1)
        }
    }
```
时间复杂度为 O($n^{2}$)


尺取法是这样做的<br/>
<table>
    <tr>
        <td bgcolor=#FF69B4>5</td> 
        <td bgcolor=#FF69B4>1</td>
       <td bgcolor=#FF69B4>3</td>
       <td bgcolor=#FF69B4>5</td>
       <td bgcolor=#FF69B4>10</td>
       <td>7</td>
       <td>4</td>
       <td>9</td>
       <td>2</td>
       <td>8</td> 
  </tr>
    <tr>
        <td>5</td> 
        <td bgcolor=#FF69B4>1</td>
       <td bgcolor=#FF69B4>3</td>
       <td bgcolor=#FF69B4>5</td>
       <td bgcolor=#FF69B4>10</td>
       <td>7</td>
       <td>4</td>
       <td>9</td>
       <td>2</td>
       <td>8</td> 
  </tr>
    <tr>
        <td>5</td> 
        <td>1</td>
       <td bgcolor=#FF69B4>3</td>
       <td bgcolor=#FF69B4>5</td>
       <td bgcolor=#FF69B4>10</td>
       <td>7</td>
       <td>4</td>
       <td>9</td>
       <td>2</td>
       <td>8</td> 
  </tr>
    <tr>
        <td>5</td> 
        <td>1</td>
       <td>3</td>
       <td bgcolor=#FF69B4>5</td>
       <td bgcolor=#FF69B4>10</td>
       <td>7</td>
       <td>4</td>
       <td>9</td>
       <td>2</td>
       <td>8</td> 
  </tr>
    <tr>
        <td>5</td> 
        <td>1</td>
       <td>3</td>
       <td>5</td>
       <td bgcolor=#FF69B4>10</td>
       <td bgcolor=#FF69B4>7</td>
       <td>4</td>
       <td>9</td>
       <td>2</td>
       <td>8</td> 
  </tr>
    <tr>
        <td>5</td> 
        <td>1</td>
       <td>3</td>
       <td>5</td>
       <td>10</td>
       <td bgcolor=#FF69B4>7</td>
       <td bgcolor=#FF69B4>4</td>
       <td bgcolor=#FF69B4>9</td>
       <td>2</td>
       <td>8</td> 
  </tr>
    <tr>
        <td>5</td> 
        <td>1</td>
       <td>3</td>
       <td>5</td>
       <td>10</td>
       <td>7</td>
       <td bgcolor=#FF69B4>4</td>
       <td bgcolor=#FF69B4>9</td>
       <td bgcolor=#FF69B4>2</td> 
       <td>8</td> 
  </tr>
    <tr>
        <td>5</td> 
        <td>1</td>
       <td>3</td>
       <td>5</td>
       <td>10</td>
       <td>7</td>
       <td>4</td>
       <td bgcolor=#FF69B4>9</td>
       <td bgcolor=#FF69B4>2</td>
       <td bgcolor=#FF69B4>8</td> 
  </tr>
</table>

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N=1e5+5;
int main()
{
    int i,t,l,r;
    int n,s;
    int sum,len;
    int a[N];
    scanf("%d",&t);
    while(t--)
    {
        sum=0;
        scanf("%d%d",&n,&s);
        len=n+1;
        for(i=1;i<=n;i++) scanf("%d",&a[i]);
        for(l=1,r=1;r<=n;r++)        //设定左右区间初始化为1
        {
            sum+=a[r];               //不断扩大右区间
            if(sum<s) continue;      //直到sum的值大于给出的s
            while(sum-a[l]>=s) sum-=a[l++];    //然后缩减区间，即扩大左区间，把 多余部分踢掉
                                                //使区间最小
            len=min(len,r-l+1);      //得到区间[l,r]，判断长度
        }                            //往复
        if(len==n+1) printf("%d\n",0);
        else printf("%d\n",len);
    }
```

<br/>
<br/>
<br/>
<br/>


