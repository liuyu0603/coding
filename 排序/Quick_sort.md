# 快速排序 (Quick Sort)

> **核心思想：** 分治 (Divide and Conquer)  
> **时间复杂度：** 平均 $O(n \log n)$  
> **空间复杂度：** $O(\log n)$ (递归栈空间)

## 1. 算法思路
快速排序的主要步骤分三步：
1.  **确定分界点 (Pivot)**：在区间 `[l, r]` 中任选一个值 `x`（通常取中间值、左边界或右边界）。
2.  **调整区间 (Partition)**：重新排列数组，使得：
    * **左半边**的所有数都 $\le x$
    * **右半边**的所有数都 $\ge x$
    * 此时，`x` 的位置不一定在正中间，只是保证了性质。
3.  **递归处理 (Recursion)**：分别对左右两段区间进行递归排序。

## 2. 代码实现

```cpp
#include<iostream>
using namespace std;

const int N = 1e6 + 10;
int q[N];

void quick_sort(int q[], int l, int r) {
    // 递归结束条件：区间只有一个数或没有数
    if(l >= r) return;

    // 1. 确定分界点 x
    // 注意：不要取 q[r]，否则配合下面 j 的递归划分可能会死循环
    // 使用 l + r >> 1 (即除以2) 可以有效避免有序数组导致的最坏情况 O(n^2)
    int x = q[(l + r) >> 1];
    
    // 两个指针，i 从左边开始，j 从右边开始
    int i = l - 1, j = r + 1;

    // 2. 调整区间
    while(i < j) {
        // i 往右走，直到找到一个 >= x 的数
        do i++; while (q[i] < x);
        // j 往左走，直到找到一个 <= x 的数
        do j--; while (q[j] > x);
        
        // 如果指针没相遇，就交换这两个不符合条件的数
        if(i < j) swap(q[i], q[j]);
    }

    // 3. 递归处理左右两边
    // 注意：如果用 j 作为分界，递归区间必须是 [l, j] 和 [j + 1, r]
    quick_sort(q, l, j);
    quick_sort(q, j + 1, r);
}

int main() {
    // 加速 C++ IO
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    cin >> n;
    for(int i = 0; i < n; i++) cin >> q[i];

    quick_sort(q, 0, n - 1);

    for(int i = 0; i < n; i++) cout << q[i] << " ";
    
    return 0;
}
