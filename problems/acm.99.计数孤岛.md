# 99. 计数孤岛
[题目链接](https://kamacoder.com/problempage.php?pid=1171)

---
## 思路
>bfs、dfs、并查集

用深度优先搜索或广度优先搜索：
如果grid[x][y] == 1(为陆地), 就将上下左右都遍历一遍，标记为true
```cpp
//深度优先搜索
void dfs(vector<vector<int> > & grid, vector<vector<bool> > & visited, int x, int y);
```
```cpp
//广度优先搜索
void bfs(vector<vector<int> > & grid, vector<vector<bool> > & visited, int x, int y);
```


### ***C++***
>bfs
```CPP
#include<iostream>
#include<vector>
#include<queue>
using namespace std;

int dir[4][2] = {0, 1, 0, -1, 1, 0, -1, 0};

void bfs(vector<vector<int> > & grid, vector<vector<bool> > & visited, int x, int y)
{
    //遇到一个1，就将所有上下左右的都标记为true
    if (grid[x][y])
    {
        visited[x][y] = true;
        queue<pair<int, int> > que;
        que.push({x, y});
        while (!que.empty())
        {
            auto cur = que.front();que.pop();
            for (int i = 0; i < 4; ++i)
            {
                int dx = cur.first + dir[i][0];
                int dy = cur.second + dir[i][1];
                if (dx < 0 || dy < 0 || dx >= grid.size() || dy >= grid[0].size())
                    continue;
                if (!visited[dx][dy] && grid[dx][dy])
                {
                    visited[dx][dy] = true;
                    que.push({dx, dy});
                }
            }
        }
    }
}

int main(void)
{
    int N, M;
    cin >> N >> M;
    vector<vector<int> > grid(N, vector<int>(M, 0));
    vector<vector<bool> > visited(N, vector<bool>(M, false));
    int i, j;
    for (i = 0; i < N; ++i)
        for (j = 0; j < M; ++j)
            cin >> grid[i][j];
    int result = 0;
    for (i = 0; i < N; ++i)
    {
        for (j = 0; j < M; ++j)
        {
            if (!visited[i][j] && grid[i][j])
            {
                result++;
                bfs(grid, visited, i, j);
            }
        }
    }
    cout << result << endl;
}
/**************************************************************
    Problem: 1171
    User: wunaiqiezixin
    Language: C++
    Result: 正确
    Time:47 ms
    Memory:2180 kb
****************************************************************/
```

>dfs

```CPP
#include<iostream>
#include<vector>
using namespace std;

int dir[4][2] = {0, 1, 0, -1, 1, 0, -1, 0};
void dfs(vector<vector<int> > & grid, vector<vector<bool> > & visited, int x, int y)
{
    if (grid[x][y])//遇到一个陆地标记，就将周围的都标记
    {
        visited[x][y] = true;
        for (int i = 0; i < 4; ++i)
        {
            int dx = x + dir[i][0];
            int dy = y + dir[i][1];
            if (dx < 0 || dx >= grid.size() || dy < 0 || dy >= grid[0].size())
                continue;
            if (!visited[dx][dy])
            {
                visited[dx][dy] = true;
                dfs(grid, visited, dx, dy);
            }
        }
    }
}
int main(void)
{
    int result = 0;
    int N, M;
    cin >> N >> M;
    vector<vector<int> > grid(N, vector<int>(M, 0));
    vector<vector<bool> > visited(N, vector<bool>(M, false));
    int i, j;
    for (i = 0; i < N; ++i)
        for (j = 0; j < M; ++j)
            cin >> grid[i][j];

    for (i = 0; i < N; ++i)
    {
        for (j = 0; j < M; ++j)
        {
            if (grid[i][j] && !visited[i][j])
            {
                result++;
                dfs(grid, visited, i, j);
            }
        }
    }
    cout << result << endl;
    return 0;
}
/**************************************************************
    Problem: 1171
    User: wunaiqiezixin
    Language: C++
    Result: 正确
    Time:56 ms
    Memory:2180 kb
****************************************************************/
```
> 2026-07-21

```CPP
#include <iostream>
#include <vector>
#include <queue>
using namespace std;
int dir[4][2]{-1,0,0,1,0,-1,1,0};

int n, m;
vector<vector<char>> adj;
char mark = '2';

// dfs:
void dfs(int x, int y) {
    if (adj[x][y] != '1') return;
    adj[x][y] = mark;

    for (int k = 0; k < 4; ++k) {
        int nx = x + dir[k][0];
        int ny = y + dir[k][1];
        if (nx < 0 || ny < 0 || nx >= n || ny >= m) continue;
        if (adj[nx][ny] == '1') dfs(nx, ny);
    }
}

// bfs:
void bfs(int x, int y) {
    if (adj[x][y] != '1') return;
    queue<pair<int, int>> que;
    que.push({x, y});

    while (!que.empty()) {
        auto [curx, cury] = que.front();
        que.pop();
        if (adj[curx][cury] != '1') continue;
        adj[curx][cury] = mark;
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
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < m; ++j) {
            cin >> adj[i][j];
        }
    }

    int ans = 0;

    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < m; ++j) {
            char ch = adj[i][j];
            if (ch == '1') {
                ++ans;
                bfs(i, j);
//              dfs(i, j);

            }
        }
    }


    cout << ans << '\n';

    return 0;
}
```
