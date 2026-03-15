# 算法笔记：k倍区间 (前缀和+同余定理)

### 1. 题目链接：[https://www.acwing.com/file_system/file/content/whole/index/content/3650/](https://www.luogu.com.cn/problem/P8649)
分析：其实很好想到O（n^2）的做法，但是由于数据范围是1e5，平方必然超时，因此必须想O（n）的做法。
实际上，我们需要的是(s[r]-s[l-1])%k==0,等价于求与s[r]余数相同的s[l-1]有多少个。换言之，对于一个确定的r，在0~r-1之间有多少个余数和它相同的呢？
必然不可能重复，因为r变化，区间就发生了变化。
有一种类似”桶“的思想！

---

### 2. 代码如下

```cpp

#include<iostream>
using namespace std;

const int N=1e5+10;
//前缀和可能超过了1e9,因为从1一直加到500000，数量级是1e10，同时cnt也可能超出，因为可能出现全是k的情况。
long long a[N],s[N],cnt;
//记录余数
int mod[N];
int n,k;

int main(){
    cin>>n>>k;
    for(int i=1;i<=n;i++)cin>>a[i];

    for(int i=1;i<=n;i++)s[i]=s[i-1]+a[i];
    //0%k=0
    mod[0]=1;
    //枚举右端点
    for(int i=1;i<=n;i++){
        //从0到这个右端点之间有多少个区间
        cnt+=mod[s[i]%k];
        //当前这个区间的余数为s[i]%k,更新
        mod[s[i]%k]++;
    }
    cout<<cnt<<endl;
    return 0;
}
