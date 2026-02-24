# 图的遍历：深度优先搜索 (DFS) 模板

深度优先搜索（DFS）是图论中最核心的遍历算法之一。它的核心思想是“一条路走到黑，不到黄河心不死”。在用 `vector` 存储邻接表时，结合递归实现 DFS 是最自然、代码最简洁的方式。



### 1. 核心逻辑

1. **全局访问标记**：使用全局数组 `visited` 记录节点是否已被访问，防止死循环（特别是在有环图中）。
2. **递归搜索**：访问当前节点后，遍历其所有相邻节点，如果相邻节点未被访问，则对其进行递归调用。
3. **保证字典序**：在搜索前对每个节点的出边进行 `sort` 排序，确保在有多条边时，优先访问编号较小的节点。
4. **处理非连通图**：在主函数中遍历所有节点，一旦发现未访问的节点，就以它为起点发起新的 DFS，确保图中的每一个连通块都能被遍历到。

### 2. 标准 C++ 模板

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

using namespace std;

// 全局 visited 数组，记录节点是否已被访问过
// 数组大小根据题目最大节点数而定
bool visited[200001];

// DFS 递归函数
// 参数 u: 当前正在访问的节点
// 参数 v: 存图的邻接表 (使用 const 引用避免不必要的拷贝)
void DFS(int u, const vector<vector<int>>& v) {
    visited[u] = 1;      // 标记当前节点已访问
    cout << u << " ";    // 处理当前节点 (例如直接输出)
    
    int len = v[u].size();
    // 遍历当前节点的所有相邻节点
    for(int i = 0; i < len; i++) {
        // 如果相邻节点没有被访问过，则继续深入搜索
        if(!visited[v[u][i]]) {
            DFS(v[u][i], v);
        }
    }
}

int main() {
    int n, e; // n 为节点数，e 为边数
    cin >> n >> e;
    
    // 初始化二维 vector 存图
    vector<vector<int>> v(n);
    
    // 读入所有有向边 (如果是无向图，需增加 v[b].push_back(a))
    for (int i = 0; i < e; i++) {
        int a, b;
        cin >> a >> b;
        v[a].push_back(b);
    }
    
    // 对每个节点的出边进行升序排序，保证 DFS 时按节点编号从小到大访问
    for (int i = 0; i < n; i++) {
        sort(v[i].begin(), v[i].end());
    }

    // 遍历所有节点，防止有孤立的节点或图不连通的情况
    for (int i = 0; i < n; i++) {
        // 如果该节点在之前的 DFS 中没有被访问到，则以它为起点开启新的 DFS
        if (!visited[i]) {
            DFS(i, v);
        }
    }
    
    return 0;
}
