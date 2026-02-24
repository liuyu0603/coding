# 图论进阶：Prim 算法 (堆优化版最小生成树) 模板

最小生成树 (Minimum Spanning Tree, MST) 用于在带权无向连通图中，找出一棵包含所有顶点的树，使得树上所有边的权值之和最小。
Prim 算法的核心思想是“加点法”：从任意一个点开始，每次贪心地挑选一条一端在树内、另一端在树外，且权值最小的边，将新的点加入树中，直到所有点都被加入。

利用优先队列（小根堆）优化后，时间复杂度为 O(E log V)，与堆优化的 Dijkstra 代码结构几乎完全一致。

### 1. 核心逻辑与 Dijkstra 的唯一区别

在遍历相邻节点进行松弛操作时：
* **Dijkstra 更新逻辑**：如果 `起点到当前点的距离 + 边权 < 起点到邻居的距离`，则更新。(`dist[j] = dist[ver] + w`)
* **Prim 更新逻辑**：如果 `这根边的边权 < 邻居到树的距离`，则更新。(`dist[j] = w`)

### 2. 标准 C++ 模板

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <climits>

using namespace std;

// 定义边结构体
struct edge {
    int to;
    int weight;
};

// first 存节点到当前最小生成树的最短距离，second 存节点编号
typedef pair<int, int> PII;

// 堆优化 Prim 算法
// 返回最小生成树的权值之和，如果图不连通则返回 -1
int prim(int n, const vector<vector<edge>>& graph) {
    // dist 记录每个节点到“当前生成树”的最短距离
    vector<int> dist(n + 1, INT_MAX);
    // st 记录每个节点是否已经加入到了生成树中
    vector<bool> st(n + 1, false);

    // 小根堆
    priority_queue<PII, vector<PII>, greater<PII>> heap;

    // 1. 初始化：从节点 1 开始生成树
    dist[1] = 0;
    heap.push({0, 1});

    int res = 0; // 记录最小生成树的总权值
    int cnt = 0; // 记录已经加入生成树的节点个数

    // 2. 主循环
    while (!heap.empty()) {
        auto t = heap.top();
        heap.pop();

        int d = t.first;     // 当前节点到树的距离
        int ver = t.second;  // 当前节点编号

        // 如果该节点已经在树中了，跳过 (过滤冗余)
        if (st[ver]) continue;

        // 3. 将节点真正加入生成树
        st[ver] = true;
        res += d;  // 累加权值
        cnt++;     // 节点数 +1

        // 4. 用刚加入的新节点，更新其他不在树中的节点到树的距离
        for (auto& ed : graph[ver]) {
            int j = ed.to;
            int w = ed.weight;
            
            // 【核心区别】: 只要不在树中，且边权小于目前记录的到树的距离，就更新并入队
            if (!st[j] && dist[j] > w) {
                dist[j] = w;
                heap.push({dist[j], j});
            }
        }
    }

    // 如果最终加入树的节点数小于总节点数，说明图是不连通的，不存在最小生成树
    if (cnt < n) return -1;
    
    return res;
}

int main() {
    int n, e; // n 个点，e 条无向边
    cin >> n >> e;

    vector<vector<edge>> graph(n + 1);

    // 最小生成树是在无向图上运行的，切记正反双向加边！
    for (int i = 0; i < e; i++) {
        int a, b, c;
        cin >> a >> b >> c;
        graph[a].push_back({b, c});
        graph[b].push_back({a, c});
    }

    int mst_weight = prim(n, graph);

    if (mst_weight == -1) {
        cout << "图不连通，无法构成最小生成树 (impossible)" << endl;
    } else {
        cout << "最小生成树的总权值为: " << mst_weight << endl;
    }

    return 0;
}
