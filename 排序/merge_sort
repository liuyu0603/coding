# 归并排序 (Merge Sort)

## 1. 算法思想
归并排序是建立在**归并**操作上的一种有效的排序算法，是采用**分治法 (Divide and Conquer)** 的一个典型应用。
- **核心逻辑**：将已有序的子序列合并，得到完全有序的序列（即先使每个子序列有序，再使子序列段间有序）。
- **时间复杂度**：稳定 $O(n \log n)$。
- **稳定性**：是**稳定**的排序算法（如果不涉及交换相等元素）。
- **空间复杂度**：需要额外 $O(n)$ 的空间（即代码中的 `tmp` 数组）。

## 2. 代码实现

```cpp
#include <iostream>
using namespace std;

const int N = 1e6 + 10;
int q[N], tmp[N]; // tmp 数组用于在合并时暂存数据

// 核心思想：分治
void merge_sort(int q[], int l, int r) {
    if (l >= r) return;

    // 1. 确定分界点
    int mid = (l + r) >> 1; // 等价于 (l + r) / 2

    // 2. 递归处理 (Divide)
    merge_sort(q, l, mid);
    merge_sort(q, mid + 1, r);

    // 3. 归并
    int i = l, j = mid + 1, k = 0;
    
    // 双指针算法：比较左右两边的值，小的放入 tmp
    while (i <= mid && j <= r) {
        if (q[i] <= q[j]) tmp[k++] = q[i++];
        else tmp[k++] = q[j++];
    }

    // 处理剩余元素：如果左边没循环完，直接接在后面
    while (i <= mid) tmp[k++] = q[i++];
    // 如果右边没循环完，直接接在后面
    while (j <= r) tmp[k++] = q[j++];

    // 4. 物归原主：将 tmp 中的有序序列复制回原数组 q
    // 注意：q 从 l 开始，tmp 从 0 开始,终点起点不一样
    for (i = l, k = 0; i <= r; i++, k++) q[i] = tmp[k];
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    cin >> n;
    for (int i = 0; i < n; i++) cin >> q[i];

    merge_sort(q, 0, n - 1);

    for (int i = 0; i < n; i++) cout << q[i] << " ";
    
    return 0;
}
