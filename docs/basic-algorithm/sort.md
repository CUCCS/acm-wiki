!!! note "Copyright"
    本页面贡献者：[rachelwo](https://github.com/rachelwo)。
    本页面内容遵循 MIT 协议，转载请附上原文出处链接和本声明。

## 冒泡排序  

### **基本思想**  
冒泡排序（Bubble Sort）也是一种简单直观的排序算法。它重复地走访过要排序的数列，一次比较两个元素，如果他们的顺序错误就把他们交换过来。走访数列的工作是重复地进行直到没有再需要交换，也就是说该数列已经排序完成。这个算法的名字由来是因为越小的元素会经由交换慢慢"浮"到数列的顶端。  
### **动图**  
[演示](https://www.runoob.com/w3cnote/bubble-sort.html)   
### **算法分析**  
* 最好情况：已经有序  
* 最坏情况：待排序记录按关键字的逆序进行排列，此时，每一趟冒泡排序需进行$i$次比较，$3i$次移动。经过$n−1$趟冒泡排序后，总的比较次数为$\sum _{i=1}^{n} i=\frac {n(n−1)}{2}$次, 总的移动次数为$\frac{3n(n−1)}{2}$次，因此该算法的时间复杂度为$O(n^{2})$  

## 选择排序  
### **基本思想**  
首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置。

再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。

重复第二步，直到所有元素均排序完毕。  
### **动图**  
[演示](https://www.runoob.com/w3cnote/selection-sort.html)  
### **算法分析**  
任何序列（不管是否有序）都是$O(n^{2})$   

### **代码实现**  
```cpp
void selection_sort(int arr[], int len)
{
    int i,j;

    for (i = 0 ; i < len - 1 ; i++)
    {
        int min = i;
        for (j = i + 1; j < len; j++)     //走访未排序的元素
            if (arr[j] < arr[min])    //找到目前最小值
                min = j;    //记录最小值
        int temp=arr[min];
        arr[min]=arr[i]
        arr[i]=temp;   //交换
    }
} 
```

## 插入排序  
### **基本思想**  
把元素插入到有序序列的合适位置  
### **动图**  
[演示](https://www.runoob.com/w3cnote/insertion-sort.html)  
### **算法分析**  
复杂度：$O(n^{2})$  

### **代码实现**  
```cpp
void insertion_sort(int arr[], int len){
    int i,j,key;
    for (i=1;i<len;i++){
            key = arr[i];
            j=i-1;
            while((j>=0) && (arr[j]>key)) {
                arr[j+1] = arr[j];
                j--;
            }
            arr[j+1] = key;
    }
}  
```

## 快速排序  
### **算法思想**  
  从数列中挑出一个元素，称为 "基准"（pivot）;

  重新排序数列，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准的后面（相同的数可以到任一边）。在这个分区退出之后，该基准就处于数列的中间位置。这个称为分区（partition）操作；

  递归地（recursive）把小于基准值元素的子数列和大于基准值元素的子数列排序；  
### **动图**  
[演示](https://www.runoob.com/w3cnote/quick-sort-2.html)  
### **算法分析**  
时间复杂度：$O(nlogn)$  

### **代码实现**  
```cpp
int a[maxn];
int b[maxn];
void quick_sort(int l,int r)
{
	if(r - l <= 0)return;
	int now = a[l];
	int p = l,q = r;
	for(int i = l + 1;i <= r;i++){
		if(a[i] <= now)b[p++] = a[i];
		else b[q--] = a[i];
	}
	b[p] = now;
	for(int i = l;i <= r;i++){
		a[i] = b[i];
	}
	quick_sort(l , p - 1);
	quick_sort(p + 1 , r);
}  
```

### 归并排序    
### **算法思想**  
将两个或两个以上有序表合并成一个新的有序表。
### **动图**
[演示](https://www.runoob.com/w3cnote/merge-sort.html)  
### **算法分析**  
  递归深度 ： $log(n)$

  每层 ： $n$

  复杂度 ： $O(nlogn)$  

### **代码实现**  
```cpp
int a[maxn];
int b[maxn];
void merge_sort(int l,int r)
{
    if(r - l <= 0)return ;
    int mid = (l + r) >> 1;
    int p = l,q = mid + 1;
    int pos = l;
    merge_sort(l , mid);
    merge_sort(mid + 1,r);
    while(p <= mid || q <= r)
    {
        if(q > r || (p <= mid && a[p] < a[q])){
            b[pos++] = a[p++];
        }else{
            b[pos++] = a[q++];
        }
    }
    for(int i = l;i <= r;i++){
        a[i] = b[i];
    }
}
```
* 引入逆序对

### sort函数
* 参数和用法：
  sort函数有三个参数
  ```cpp
  sort(first,last,cmp);
  ```

  其中，first是元素的起始地址，last是结束地址，cmp是排序的方式。对[first，last)（一定要注意这里的区间是左闭又开）区间内数据根据cmp的方式进行排序。也可以不写第三个参数，此时按默认排序，从小到大进行排序。  

* 自定义排序规则  
    ``` cpp 
    bool cmp(int a, int b)
    {
        return b<a;
    }

    sort(a,a+n,cmp);//降序
    ```

* 重载运算符  
    ```cpp
    bool operator< (const Student& s1, const Student& s2)
    {
        if(s1.age==s2.age)
            return s1.name <s2.name;//年龄相同时，按姓名小到大排
            else  return s1.age > s2.age; //从年龄大到小排序
    }
    sort(a,a+n); //用于结构体排序 
    ```

* 声明比较类  
    ```cpp 
    struct cmp
    {
        bool operator() (const Student& s1, const Student& s2)
        {
            if(s1.age==s2.age)
                return s1.name <s2.name;
            else  eturn s1.age < s2.age;
        }
    };

    sort(a,a+n,cmp());  
    ```







