# 102. 沉没孤岛
[题目链接](https://kamacoder.com/problempage.php?pid=1174)

## 思路
用`visited`数组标记周边的岛屿

### ***C++***
```CPP
#include<iostream>
#include<vector>
using namespace std;

int dir[4][2] = {0, 1, 0, -1, 1, 0, -1, 0};

void dfs(vector<vector<int> > & grid, vector<vector<bool> > & visited, int x, int y)
{
    if (grid[x][y])
    {
        if (!visited[x][y])
            visited[x][y] = true;
        for (int i = 0; i < 4; ++i)
        {
            int dx = x + dir[i][0];
            int dy = y + dir[i][1];
            if (dx < 0 || dy < 0 || dx >= grid.size() || dy >= grid[0].size())
                continue;
            if (grid[dx][dy] && !visited[dx][dy])
            {
                visited[dx][dy] = true;
                dfs(grid, visited, dx, dy);
            }
        }
    }
}

int main(void)
{
    int N, M;
    cin >> N >> M;
    vector<vector<int> > grid(N, vector<int>(M));
    vector<vector<bool> > visited(N, vector<bool>(M, false));
    int i, j;
    for (i = 0; i < N; ++i)
        for (j = 0; j < M; ++j)
            cin >> grid[i][j];
    for (i = 0; i < N; ++i)
    {
        if (grid[i][0])
            dfs(grid, visited, i, 0);
        if (grid[i][M - 1])
            dfs(grid, visited, i, M - 1);
    }
    for (j = 0; j < M; ++j)
    {
        if (grid[0][j])
            dfs(grid, visited, 0, j);
        if (grid[N - 1][j])
            dfs(grid, visited, N - 1, j);
    }
    for (i = 0; i < N; ++i)
    {
        for (j = 0; j < M; ++j)
        {
            if (grid[i][j] && !visited[i][j])
                cout << 0;
            else
                cout << grid[i][j];
            cout << " ";
        }
        cout << endl;
    }
    return 0;
}
/**************************************************************
    Problem: 1174
    User: wunaiqiezixin
    Language: C++
    Result: 正确
    Time:69 ms
    Memory:2180 kb
****************************************************************/
```

将周边的普通岛屿变为2
```CPP
#include<iostream>
#include<vector>
#include<queue>
using namespace std;

int dir[4][2] = {
    { 1, 0},
    {-1, 0},
    { 0, 1},
    { 0,-1}
};

void bfs(vector<vector<int> > & grid, int x, int y)
{
    if (grid[x][y] == 1)
    {
        queue<int> que;
        grid[x][y] = 2;
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
                if (grid[dx][dy] == 1)
                {
                    grid[dx][dy] = 2;
                    que.push(dx);que.push(dy);
                }
            }
        }
    }
}

//0为水， 1为孤岛， 2为普通陆地
int main(void)
{
    int N, M;
    cin >> N >> M;
    vector<vector<int> >grid(N, vector<int>(M));
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
    for (i = 0; i < N; ++i)
    {
        for (j = 0; j < M; ++j)
        {
            if (grid[i][j] == 2)
                cout << 1;
            else if (grid[i][j] == 1)
                cout << 0;
            else
                cout << grid[i][j];
            cout << " ";
        }
        cout << endl;
    }
    return 0;
}
/**************************************************************
    Problem: 1174
    User: wunaiqiezixin
    Language: C++
    Result: 正确
    Time:52 ms
    Memory:2180 kb
****************************************************************/
```
> 2026-07-23

```CPP
#include <iostream>
#include <vector>
#include <queue>
using namespace std;
int dir[4][2]{-1,0,0,1,0,-1,1,0};

vector<vector<char>> adj;
int n, m;

void dfs(int x, int y) {
    if (adj[x][y] != '1') return;
    adj[x][y] = '2';
    for (int k = 0; k < 4; ++k) {
        int nx = x + dir[k][0];
        int ny = y + dir[k][1];
        if (nx < 0 || ny < 0 || nx >= n || ny >= m) continue;
        if (adj[nx][ny] == '1') dfs(nx, ny);
    }
}

void bfs(int x, int y) {
    if (adj[x][y] != '1') return;
    queue<pair<int, int>> que;
    que.push({x, y});
    while (!que.empty()) {
        auto [curx, cury] = que.front();
        que.pop();
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

    for (int i = 0; i < n; ++i) {
        if (adj[i][0] == '1') dfs(i, 0);
        if (adj[i][m - 1] == '1') bfs(i, m - 1);
    }

    for (int j = 0; j < m; ++j) {
        if (adj[0][j] == '1') dfs(0, j);
        if (adj[n - 1][j] == '1') bfs(n - 1, j);
    }

    for (int i = 0; i < n; ++i) {
        cout << (adj[i][0] == '2' ? '1' : '0');
        for (int j = 1; j < m; ++j) {
            cout << " " << (adj[i][j] == '2' ? '1' : '0');
        }
        cout << '\n';
    }

    return 0;
}
```
