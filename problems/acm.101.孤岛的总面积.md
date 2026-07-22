# 101. 孤岛的总面积
[题目链接](https://kamacoder.com/problempage.php?pid=1173)

---
## 思路
对矩阵`grid`边缘的陆地(1)进行搜索，将与其连着的整座岛屿置为0

### ***C++***
>dfs
```CPP
#include<iostream>
#include<vector>
using namespace std;

int dir[4][2] = {0, 1, 0, -1, 1, 0, -1,0};

void dfs(vector<vector<int> > & grid,int x, int y)
{
        if (grid[x][y])
        grid[x][y] = 0;
        for (int i = 0; i < 4; ++i)
        {
            int dx = x + dir[i][0];
            int dy = y + dir[i][1];
            if (dx < 0 || dy < 0 || dx >= grid.size() || dy >= grid[0].size())
                continue;
            if (grid[dx][dy])
            {
                grid[dx][dy] = 0;
                dfs(grid, dx, dy);
            }
        }

}

int main(void)
{
    int N, M;
    cin >> N >> M;
    vector<vector<int> > grid(N, vector<int>(M));
    int i, j;
    for (i = 0; i < N; ++i)
        for (j = 0; j < M; ++j)
            cin >> grid[i][j];
    for (i = 0; i < N; ++i)
    {
        if (grid[i][0])
            dfs(grid, i, 0);
        if (grid[i][M - 1])
            dfs(grid, i, M - 1);
    }
    for (j = 0; j < M; ++j)
    {
        if (grid[0][j])
            dfs(grid, 0, j);
        if (grid[N - 1][j])
            dfs(grid, N - 1, j);
    }
    int cnt = 0;
    for (i = 0; i < N; ++i)
        for (j = 0; j < M; ++j)
            if (grid[i][j])
                cnt++;
    cout << cnt << endl;
    return 0;
}
/**************************************************************
    Problem: 1173
    User: wunaiqiezixin
    Language: C++
    Result: 正确
    Time:47 ms
    Memory:2176 kb
****************************************************************/
```

>bfs

```CPP
#include<iostream>
#include<vector>
#include<queue>
using namespace std;

int dir[4][2] = {0, 1, 0, -1, 1, 0, -1,0};


void bfs(vector<vector<int> > & grid, int x, int y)
{
    if (grid[x][y])
    {
        grid[x][y] = 0;
        queue<int> que;
        que.push(x);que.push(y);
        while (!que.empty())
        {
            int xx = que.front();que.pop();
            int yy = que.front();que.pop();
            for (int i = 0; i < 4; ++i)
            {
                int dx = xx + dir[i][0];
                int dy = yy + dir[i][1];
                if (dx < 0 || dy < 0 || dx >= grid.size() || dy >= grid[0].size())
                    continue;
                if (grid[dx][dy])
                {
                    grid[dx][dy] = 0;
                    que.push(dx);que.push(dy);
                }
            }
        }
    }
}

int main(void)
{
    int N, M;
    cin >> N >> M;
    vector<vector<int> > grid(N, vector<int>(M));
    int i, j;
    for (i = 0; i < N; ++i)
        for (j = 0; j < M; ++j)
            cin >> grid[i][j];
    for (i = 0; i < N; ++i)
    {
        if (grid[i][0])
            bfs(grid, i, 0);
        if (grid[i][M - 1])
            bfs(grid, i, M - 1);
    }
    for (j = 0; j < M; ++j)
    {
        if (grid[0][j])
            bfs(grid, 0, j);
        if (grid[N - 1][j])
            bfs(grid, N - 1, j);
    }
    int cnt = 0;
    for (i = 0; i < N; ++i)
        for (j = 0; j < M; ++j)
            if (grid[i][j])
                cnt++;
    cout << cnt << endl;
    return 0;
}
/**************************************************************
    Problem: 1173
    User: wunaiqiezixin
    Language: C++
    Result: 正确
    Time:45 ms
    Memory:2180 kb
****************************************************************/
```

> 2026-07-22

```CPP
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

int dir[4][2]{-1,0,0,1,0,-1,1,0};
vector<vector<char>> adj;
int n, m;

void dfs(int x, int y, int& area) {
    if (adj[x][y] != '1') return;
    ++area;
    adj[x][y] = '2';
    for (int k = 0; k < 4; ++k) {
        int nx = x + dir[k][0];
        int ny = y + dir[k][1];
        if (nx < 0 || ny < 0 || nx >= n || ny >= m) continue;
        if (adj[nx][ny] == '1') dfs(nx, ny, area);
    }
}

void bfs(int x, int y, int& area) {
    if (adj[x][y] != '1') return;
    queue<pair<int, int>> que;
    que.push({x, y});
    while (!que.empty()) {
        auto [curx, cury] = que.front();
        que.pop();
        if (adj[curx][cury] != '1') continue;
        ++area;
        adj[curx][cury] = '2';
        for (int k = 0; k < 4; ++k) {
            int nx = curx + dir[k][0];
            int ny = cury + dir[k][1];
            if (nx < 0 || ny < 0 || nx >= n || ny >= m) continue;
            if (adj[nx][ny] == '1') que.push({nx, ny});
        }
    }
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);

    cin >> n >> m;
    adj.resize(n, vector<char>(m));

    for (int i = 0; i < n; ++i)
        for (int j = 0; j < m; ++j)
            cin >> adj[i][j];

    int area = 0;
    for (int i = 0; i < n; ++i) {
        dfs(i, 0, area);
        bfs(i, m - 1, area);
    }
    for (int j = 0; j < m; ++j) {
        dfs(0, j, area);
        bfs(n - 1, j, area);
    }

    int total_area = 0;

    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < m; ++j) {
            if (adj[i][j] == '1') {
                area = 0;
                dfs(i, j, area);
                total_area += area;
            }
        }
    }

    cout << total_area << '\n';

    return 0;
}
```
