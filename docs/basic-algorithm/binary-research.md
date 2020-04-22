# 二分

## 整数集合上的二分
 在单调递增序列a中查找 $\geq$x 的数中最小的一个数<br/><br/>
```cpp
while(l<r)
{
	int mid=(l+r)>>1;
	if(a[mid]>=x)
		r=mid;
	else
		l=mid+1;
}
```
在单调递增序列a中查找 $\leq$x 的数中最大的一个数<br/>
```cpp
while(l<r)
{
	int mid=(l+r+1)>>1;
	if(a[mid]<=x)
		l=mid;
	else
		r=mid-1;
}
```

## 实数域上的二分
确定好所需的精度eps，以 l+eps<r 为循环条件每次根据在 mid 上的判定选择 r=mid 或 l=mid即可。一般需要保留 k 位小数时，则取  $eps=10^{-(k+2)}$
```cpp
while(l+eps<r)
{
    double mid=(l+r)/2;
    if(check(mid)) r=mid; else l=mid;
}
```
有时精度不容易确定或表示，就干脆采用循环固定次数的二分方法，也是一种相当不错的策略。这种方法得到的结果的精度通常比设置eps更高。<br/>
```cpp
for(int i=0;i<100;i++)
{
	double mid=(l+r)/2;
	if(check(mid)) r=mid; else l=mid;
}
```

## 二分答案转化为判定
把求最优解的问题，转化为给定一个值mid，判定是否存在一个可行方案评分达到mid的问题。<br/><br/>
> 有N本书排成一行，已知第i本书的厚度是Ai。把他们分成连续的M组，使T最小化，其中T表示厚度之和最大的一组的厚度。

题目中描述中出现了类似于“最大值最小”的含义，这是答案具有单调性，可用二分转化为判定的最常见、最典型的特征之一。<br/>
```cpp
bool check(int size)
{
	int group=1,rest=size;
	for(int i=1;i<=n;i++)
	{
		if(rest>a[i]) rest-=a[i];
		else group++,rest=size-a[i];
	}
	return group<=m;
}
int main()
{
	int l=0,r=sum_of_ai;
	while(l<r)
	{
		int mid=(l+r)/2;
		if(check(mid)) r=mid;
		else l=mid+1;
	}
	cout<<l<<endl;
}
```


<br/>

## 两个常用的函数
lower_bound( )和upper_bound( )都是利用二分查找的方法在一个排好序的数组中进行查找的。<br/><br/>
在从小到大的排序数组中，<br/><br/>
lower_bound( begin,end,num)：从数组的begin位置到end-1位置二分查找第一个大于或等于num的数字，找到返回该数字的地址，不存在则返回end。通过返回的地址减去起始地址begin,得到找到数字在数组中的下标。<br/><br/>
upper_bound( begin,end,num)：从数组的begin位置到end-1位置二分查找第一个大于num的数字，找到返回该数字的地址，不存在则返回end。通过返回的地址减去起始地址begin,得到找到数字在数组中的下标。<br/><br/>
在从大到小的排序数组中，重载lower_bound()和upper_bound()<br/><br/>
lower_bound( begin,end,num,greater<type>() ):从数组的begin位置到end-1位置二分查找第一个小于或等于num的数字，找到返回该数字的地址，不存在则返回end。通过返回的地址减去起始地址begin,得到找到数字在数组中的下标。<br/><br/>
upper_bound( begin,end,num,greater<type>() ):从数组的begin位置到end-1位置二分查找第一个小于num的数字，找到返回该数字的地址，不存在则返回end。通过返回的地址减去起始地址begin,得到找到数字在数组中的下标。<br/>

