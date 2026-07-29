# [多余的边](https://kamacoder.com/problempage.php?pid=1181)

---

```CPP
#include <iostream>
#include <vector>
using namespace std;

const int MAXN = 1001; // 节点编号范围 1~1000

vector<int> father(MAXN);

// 初始化并查集：每个节点独立为一个集合
void init() {
    for (int i = 0; i < MAXN; ++i) {
        father[i] = i;
    }
}

// 查找根节点 + 路径压缩优化
int find(int x) {
    return father[x] == x ? x : father[x] = find(father[x]);
}

// 合并两个节点所在的集合
void join(int u, int v) {
    u = find(u);
    v = find(v);
    if (u != v) {
        father[v] = u;
    }
}

// 判断两个节点是否连通（根节点是否相同）
bool same(int u, int v) {
    u = find(u);
    v = find(v);
    return u == v;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    init();

    int n;
    cin >> n;

    while (n--) {
        int u, v;
        cin >> u >> v;
        if (same(u, v)) {
            // 两端点已连通，当前边就是形成环的冗余边
            cout << u << " " << v << '\n';
            return 0;
        } else {
            join(u, v);
        }
    }

    return 0;
}
```
