# 图论进阶：Dijkstra 算法 (堆优化版) 模板

Dijkstra 算法用于求解**所有边权均为非负数**的单源最短路径问题。
朴素版 Dijkstra 的时间复杂度为 O(V^2)，适合稠密图；而**堆优化版 Dijkstra** 利用优先队列（小根堆）维护当前最短距离，时间复杂度降为 O(E log V)，是处理大规模稀疏图的绝对主力。

### 1. 核心逻辑

1. **距离数组 `dist`**：记录起点到每个节点的最短距离，初始全部设为无穷大（如 `INT_MAX` 或 `0x3f3f3f3f`），起点距离设为 0。
2. **访问标记数组 `st`**：记录哪些节点的最短路径已经确定。
3. **优先队列（小根堆）**：队列里存放 `pair<距离, 节点编号>`。C++ 中默认的 `priority_queue` 是大根堆，我们需要通过 `greater<pair<int, int>>` 将其翻转为小根堆，保证每次弹出的都是当前距离最近的节点。
4. **松弛操作**：取出堆顶节点，遍历它的所有出边。如果通过该节点到达邻居的距离，比邻居原本记录的距离更短，就更新邻居的距离，并将 `{新距离, 邻居编号}` 压入堆中。
5. **冗余过滤**：由于同一个节点可能会随着距离的不断更新被多次压入堆中，我们在堆顶取出节点时，如果发现它已经被标记过（最短路已确定），直接 `continue` 跳过即可。

### 2. 标准 C++ 模板

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <climits> // 引入 INT_MAX

using namespace std;

// 定义边结构体
struct edge {
    int to;
    int weight;
};

// 使用 typedef 简化 pair 的写法
// pair 的 first 存当前最短距离，second 存节点编号
// 这样优先队列默认就会按照 first (距离) 进行排序
typedef pair<int, int> PII;

// 堆优化 Dijkstra 算法
// n: 节点总数 (假设节点编号从 1 到 n)
// start: 起点编号
// graph: vector 存图的邻接表
void dijkstra(int n, int start, const vector<vector<edge>>& graph) {
    // dist 数组初始化为无穷大
    vector<int> dist(n + 1, INT_MAX);
    // st 数组记录该节点的最短路径是否已经确定
    vector<bool> st(n + 1, false);

    // 定义小根堆
    priority_queue<PII, vector<PII>, greater<PII>> heap;

    // 1. 初始化起点
    dist[start] = 0;
    heap.push({0, start});

    // 2. 主循环
    while (!heap.empty()) {
        // 取出当前距离起点最近的节点
        auto t = heap.top();
        heap.pop();

        int distance = t.first; // 当前最短距离
        int ver = t.second;     // 当前节点编号

        // 如果这个节点的最短路已经确定过了，直接跳过 (过滤冗余数据)
        if (st[ver]) continue;
        
        // 标记为已确定
        st[ver] = true;

        // 3. 遍历该节点的所有出边进行松弛操作
        for (auto& ed : graph[ver]) {
            int j = ed.to;
            int w = ed.weight;
            
            // 如果通过 ver 到达 j 的距离更短，就更新距离并入堆
            if (dist[ver] + w < dist[j]) {
                dist[j] = dist[ver] + w;
                heap.push({dist[j], j});
            }
        }
    }

    // 输出起点到所有点的最短距离
    for (int i = 1; i <= n; i++) {
        if (dist[i] == INT_MAX) {
            cout << "起点 " << start << " 无法到达节点 " << i << endl;
        } else {
            cout << "起点 " << start << " 到节点 " << i << " 的最短距离为: " << dist[i] << endl;
        }
    }
}

int main() {
    int n, e; // n 个点，e 条边
    cin >> n >> e;

    vector<vector<edge>> graph(n + 1);

    for (int i = 0; i < e; i++) {
        int a, b, c;
        cin >> a >> b >> c;
        // 有向图加边。如果是无向图，再加一句 graph[b].push_back({a, c});
        graph[a].push_back({b, c});
    }

    int start;
    cout << "请输入起点: ";
    cin >> start;

    dijkstra(n, start, graph);

    return 0;
}
