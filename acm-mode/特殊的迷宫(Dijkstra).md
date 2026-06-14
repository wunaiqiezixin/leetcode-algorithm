# 388.特殊的迷宫
[题目链接](https://kamacoder.com/problempage.php?pid=1468)

---

# Dijkstra + 关键剪枝优化

这道题的核心挑战在于处理传送移动带来的指数级边数爆炸问题。直接为所有格子对建立传送边会导致 O ((nm)²) 的边数，完全无法在时间限制内运行。我们需要一个巧妙的剪枝思路来将边数控制在可接受范围内。  

## 核心思路：传送代价的上限剪枝

#### 关键观察

在没有传送的情况下，网格中任意两点之间的最大普通移动代价是n+m-2（即从左上角到右下角的曼哈顿距离）。这意味着：  

>如果一次传送的代价 a[i][j] ^ a[x][y] >= n+m，那么这次传送绝对不划算 —— 直接走路过去的花费都不会超过 n+m-2。  

## 算法设计  

### 1. 数据预处理：  

- 将二维网格转换为一维节点编号：`u = r * m + c`，方便处理
- 建立数值到节点的映射表 `val_to_idx：val_to_idx[x]` 表示数值 x 所在的节点编号，不存在则为 -1。利用题目中 “所有数值唯一” 的条件，可以 O (1) 查询。

### 2. Dijkstra 最短路径算法:  

因为所有移动代价都是非负的（普通移动代价 1，传送代价异或结果 >=0），Dijkstra 算法是最优选择。  

对于每个弹出的当前节点 `u`：  
1. **普通移动扩展：** 向上下左右四个方向移动，代价为 1
2. **传送移动扩展：** 枚举所有可能的有效传送代价 k（从 0 到 n+m），计算目标数值 target_val = a[u] ^ k，通过映射表快速查找是否存在该数值的节点。如果存在，则添加一条代价为 k 的边。
3. **提前终止优化：** Dijkstra 算法的性质保证：**第一次弹出终点节点时，得到的距离就是最短距离**。因此我们可以立即输出结果并返回，无需处理剩余节点。

## 代码实现：

```CPP
#include <iostream>
#include <vector>
#include <queue>
#include <cstdint>
using namespace std;
#define INF 2e18
#define MAX_VAL 200005
//边结构体
struct Edge {
    int v;
    int64_t w;
    Edge(int a, int64_t b) : v(a), w(b) {}
};
//小顶堆
struct mycomparison {
    bool operator()(const pair<int, int64_t>& lhs, 
                    const pair<int, int64_t>& rhs) const {
        return lhs.second > rhs.second;                    
    }
};

//一组数据
void solve() {
    int n, m;
    cin >> n >> m;
    int num_cells = n * m;
    vector<int64_t> a(num_cells);
    vector<int> val_to_idx(MAX_VAL, -1);
    for (int i = 0; i < num_cells; ++i) {
        cin >> a[i];
        val_to_idx[a[i]] = i;
    }

    priority_queue<pair<int, int64_t>,
                   vector<pair<int, int64_t>>,
                   mycomparison> pq;
    
    vector<int64_t> dist(num_cells, INF);
    dist[0] = 0;

    pq.push({0, dist[0]});

    while (!pq.empty()) {
        auto [u, d] = pq.top();
        pq.pop();
        if (d > dist[u]) continue;
        //第一次弹出终点节点时，得到的距离就是最短距离
        if (u == num_cells - 1) {
            cout << d << '\n';
            return;
        }
        
        //普通移动:
        int dir[4][2]{-1, 0, 0, 1, 0, -1, 1, 0};
        int x = u / m, y = u % m;
        for (int i = 0; i < 4; ++i) {
            int dx = x + dir[i][0];
            int dy = y + dir[i][1];
            if (dx < 0 || dy < 0 || dx >= n || dy >= m)
                continue;
            int v = dx * m + dy;
            if (dist[v] > d + 1) {
                dist[v] = d + 1;
                pq.push({v, dist[v]});
            }
        }

        //传送移动:
        const int64_t MAX_COST = m + n;
        for (int64_t k = 0; k < MAX_COST; ++k) {
            int64_t target_val = k ^ a[u];
            if (target_val >= MAX_VAL || 
                val_to_idx[target_val] == -1) 
                continue;
            int v = val_to_idx[target_val];
            if (dist[v] > d + k) {
                dist[v] = d + k;
                pq.push({v, dist[v]});
            }
        }
    }
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    int t;
    cin >> t;
    while (t -- ) {
        solve();
    }
    return 0;
}
```

## 代码思考：

```CPP
if (d > dist[u]) continue;
```

- 它解决了优先队列中存在多个相同节点条目的问题
- 它确保我们只处理每个节点的最短路径版本
- 它是算法正确性和效率的关键保障
