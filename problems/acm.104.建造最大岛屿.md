# 104. 建造最大岛屿
[题目链接](https://kamacoder.com/problempage.php?pid=1176)

---
### ***C++***
```CPP
#include<iostream>
#include<vector>
#include<unordered_map>
#include<unordered_set>
using namespace std;

int cnt;
int dir[4][2] = {0, 1, 0, -1, 1, 0, -1, 0};

void dfs(vector<vector<int> > & grid, int x, int y, int mark)
{
    if (grid[x][y] != 1)
        return;
    grid[x][y] = mark;
    cnt++;
    for (int i = 0; i < 4; ++i)
    {
        int dx = x + dir[i][0];
        int dy = y + dir[i][1];
        if (dx < 0 || dy < 0 || dx >= grid.size() || dy >= grid[0].size())
            continue;
        if (grid[dx][dy] == 1)
            dfs(grid, dx, dy, mark);
    }
}

int main(void)
{
    int result = 0;
    unordered_map<int, int> gridNum;
    unordered_set<int> st;

    int N, M;
    cin >> N >> M;
    int i, j;
    vector<vector<int> > grid(N, vector<int>(M));
    for (i = 0; i < N; ++i)
        for (j = 0; j < M; ++j)
            cin >> grid[i][j];

    int mark = 2;
    for (i = 0; i < N; ++i)
    {
        for (j = 0; j < M; ++j)
        {
            if (grid[i][j] == 1)
            {
                cnt = 0;
                dfs(grid, i, j, mark);
                gridNum[mark] = cnt;
                mark++;
            }
        }
    }

    for (i = 0; i < N; ++i)
    {
        for (j = 0; j < M; ++j)
        {
            if (grid[i][j] == 0)
            {
                cnt = 1;
                st.clear();
                for (int k = 0; k < 4; ++k)
                {
                    int dx = i + dir[k][0];
                    int dy = j + dir[k][1];
                    if (dx < 0 || dy < 0 || dx >= N || dy >= M)
                        continue;
                    if (grid[dx][dy] != 0 && !st.count(grid[dx][dy]))
                    {
                        cnt += gridNum[grid[dx][dy]];
                        st.insert(grid[dx][dy]);
                    }
                }
            }
            result = max(result, cnt);
        }
    }

    cout << result << endl;
    return 0;
}


/**************************************************************
    Problem: 1176
    User: wunaiqiezixin
    Language: C++
    Result: 正确
    Time:50 ms
    Memory:2184 kb
****************************************************************/
```

> 2026-07-24

```CPP
#include <iostream>
#include <vector>
using namespace std;
int dir[4][2]{-1,0,0,1,0,-1,1,0};

int area[2501]{};
int n, m;
vector<vector<int>> adj;
vector<vector<bool>> visited;

void dfs(int x, int y, int order) {
    if (visited[x][y] || adj[x][y] == 0) return;
    visited[x][y] = true;
    adj[x][y] = order;
    area[order]++;
    for (int k = 0; k < 4; ++k) {
        int nx = x + dir[k][0];
        int ny = y + dir[k][1];
        if (nx < 0 || ny < 0 || nx >= n || ny >= m) continue;
        if (!visited[nx][ny] && adj[nx][ny] == 1)
            dfs(nx, ny, order);
    }
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    cin >> n >> m;
    adj.resize(n, vector<int>(m));
    visited.resize(n, vector<bool>(m, false));
    bool flag = true;
    for (int i = 0; i < n; ++i)
        for (int j = 0; j < m; ++j) {
            cin >> adj[i][j];
            if (adj[i][j] == 0) flag = false;
        }
    if (flag) {
        cout << n * m << '\n';
        return 0;
    }

    int order = 1;

    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < m; ++j) {
            if (!visited[i][j] && adj[i][j] == 1) {
                dfs(i, j, order);
                ++order;
            }
        }
    }

    int max_area = 1;

    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < m; ++j) {
            if (adj[i][j] == 0) {
                int cur_area = 1;
                vector<bool> is_visited(order + 1, false);
                for (int k = 0; k < 4; ++k) {
                    int ni = i + dir[k][0];
                    int nj = j + dir[k][1];
                    if (ni < 0 || nj < 0 || ni >= n || nj >= m)
                        continue;
                    if (adj[ni][nj] != 0 && !is_visited[adj[ni][nj]]) {
                        is_visited[adj[ni][nj]] = true;
                        cur_area += area[adj[ni][nj]];
                    }
                }
                if (cur_area > max_area) max_area = cur_area;
            }
        }
    }

    cout << max_area << '\n';

    return 0;
}
```
