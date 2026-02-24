# 图的遍历：广度优先搜索 (BFS) 模板

广度优先搜索（BFS）是一种“层序遍历”算法。它总是先访问距离起点最近的节点，然后再向外扩展。在 C++ 中，我们通常使用 `std::queue` 来实现这种“先进先出”的逻辑。

BFS 最经典的实战用途之一是：**求解无权图（或所有边权相等）的单源最短路径**。它的时间复杂度为 O(V + E)，比 Dijkstra 快得多。

### 1. 核心逻辑

1. **队列 `queue`**：用于存放当前层的节点。起点首先入队。
2. **访问标记数组 `visited` / 距离数组 `dist`**：为了防止走回头路陷入死循环，必须标记已经访问过的节点。如果是求最短路，可以直接用 `dist` 数组代替 `visited`（初始化为 -1，如果不为 -1 说明访问过了）。
3. **层层向外扩展**：每次从队头取出一个节点，遍历它的所有相邻节点。如果相邻节点未被访问过，则将其标记（或记录距离）并推入队尾。

### 2. 标准 C++ 模板

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <algorithm>

using namespace std;

// 广度优先搜索函数
// n: 节点总数 (假设编号从 0 到 n-1)
// start: 搜索的起点
// graph: 存图的邻接表
void BFS(int n, int start, const vector<vector<int>>& graph) {
    // 距离数组，初始化为 -1，不仅记录最短距离，还充当 visited 数组的作用
    vector<int> dist(n, -1);
    queue<int> q;

    // 1. 初始化起点
    q.push(start);
    dist[start] = 0; // 起点到自己的距离是 0

    cout << "BFS 遍历顺序: ";

    // 2. 队列主循环
    while (!q.empty()) {
        // 取出队头节点
        int curr = q.front();
        q.pop();

        cout << curr << " "; // 处理当前节点

        // 3. 遍历当前节点的所有邻居
        for (int i = 0; i < graph[curr].size(); i++) {
            int next_node = graph[curr][i];

            // 如果邻居还没有被访问过 (距离为 -1)
            if (dist[next_node] == -1) {
                // 邻居的距离 = 当前节点的距离 + 1
                dist[next_node] = dist[curr] + 1;
                // 将邻居推入队列，等待后续扩展
                q.push(next_node);
            }
        }
    }
    cout << endl;

    // 输出起点到各个节点的最短距离 (仅限无权图)
    cout << "起点 " << start << " 到各点的最短距离:" << endl;
    for (int i = 0; i < n; i++) {
        if (dist[i] != -1) {
            cout << "-> 节点 " << i << " : " << dist[i] << endl;
        } else {
            cout << "-> 节点 " << i << " : 无法到达" << endl;
        }
    }
}

int main() {
    int n, e; // n 为节点数，e 为边数
    cin >> n >> e;

    // 初始化二维 vector 存图
    vector<vector<int>> graph(n);

    // 读入所有边 (这里以无向图为例)
    for (int i = 0; i < e; i++) {
        int a, b;
        cin >> a >> b;
        graph[a].push_back(b);
        graph[b].push_back(a); // 无向图双向建边
    }

    // 可选：对邻接表进行排序，保证每次按节点编号从小到大的顺序扩展
    for (int i = 0; i < n; i++) {
        sort(graph[i].begin(), graph[i].end());
    }

    int start;
    cout << "请输入 BFS 起点: ";
    cin >> start;

    BFS(n, start, graph);

    return 0;
}
