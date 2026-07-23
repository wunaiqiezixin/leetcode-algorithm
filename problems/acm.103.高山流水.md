# 103. 高山流水
[题目链接](https://kamacoder.com/problempage.php?pid=1175)

---
### ***C++***
```CPP
#include<iostream>
#include<vector>
using namespace std;

int dir[4][2] = {1, 0, -1, 0, 0, 1, 0, -1};

/* 从边界点(x, y)逆流而上，标记所有路径 */
void dfs(vector<vector<int> > & grid, vector<vector<bool> > & visited, int x, int y)
{
    if (!visited[x][y])
        visited[x][y] = true;
    for (int i = 0; i < 4; ++i)
    {
        int dx = x + dir[i][0];
        int dy = y + dir[i][1];
        if (dx < 0 || dy < 0 || dx >= grid.size() || dy >= grid[0].size())
            continue;
        if (!visited[dx][dy] && grid[x][y] <= grid[dx][dy])
        {
            visited[dx][dy] = true;
            dfs(grid, visited, dx, dy);
        }
    }
}

int main(void)
{
    int N, M;
    cin >> N >> M;
    int i, j;
    vector<vector<int> > grid(N, vector<int>(M, 0));
    vector<vector<bool> > firstBoundary(N, vector<bool>(M, false));
    vector<vector<bool> > secondBoundary(N, vector<bool>(M, false));
    for (i = 0; i < N; ++i)
        for (j = 0; j < M; ++j)
            cin >> grid[i][j];

    for (i = 0; i < N; ++i)
    {
        dfs(grid, firstBoundary, i, 0);
        dfs(grid, secondBoundary, i, M - 1);
    }
    for (j = 0; j < M; ++j)
    {
        dfs(grid, firstBoundary, 0, j);
        dfs(grid, secondBoundary, N - 1, j);
    }

    for (i = 0; i < N; ++i)
        for (j = 0; j < M; ++j)
            if (firstBoundary[i][j] && secondBoundary[i][j])
                cout << i << " " << j << endl;
    return 0;
}
/**************************************************************
    Problem: 1175
    User: wunaiqiezixin
    Language: C++
    Result: 正确
    Time:58 ms
    Memory:2312 kb
****************************************************************/
```

> 2026-07-23

```CPP
#include <iostream>
#include <vector>
using namespace std;
int dir[4][2]{-1,0,0,1,0,-1,1,0};

int n, m;
vector<vector<int>> adj;

void dfs(int x, int y, vector<vector<bool>>& visited) {
    if (visited[x][y]) return;
    visited[x][y] = true;
    for (int k = 0; k < 4; ++k) {
        int nx = x + dir[k][0];
        int ny = y + dir[k][1];
        if (nx < 0 || ny < 0 || nx >= n || ny >= m) continue;
        if (!visited[nx][ny] && adj[nx][ny] >= adj[x][y])
            dfs(nx, ny, visited);
    }
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    cin >> n >> m;
    adj.resize(n, vector<int>(m));
    for (int i = 0; i < n; ++i)
        for (int j = 0; j < m; ++j)
            cin >> adj[i][j];
    vector<vector<bool>> is_first(n, vector<bool>(m, false));
    vector<vector<bool>> is_second(n, vector<bool>(m, false));
    for (int i = 0; i < n; ++i) {
        dfs(i, 0, is_first);
        dfs(i, m - 1, is_second);
    }
    for (int j = 0; j < m; ++j) {
        dfs(0, j, is_first);
        dfs(n - 1, j, is_second);
    }
    for (int i = 0; i < n; ++i)
        for (int j = 0; j < m; ++j)
            if (is_first[i][j] && is_second[i][j])
                cout << i << " " << j << '\n';
    return 0;
}
```
