# 107. 寻找存在的路线
[题目链接](https://kamacoder.com/problempage.php?pid=1179)

---

### ***C++***
```CPP
#include<iostream>
#include<vector>
using namespace std;

int n = 101;

vector<int> father = vector<int>(n);

void init()
{
    int i;
    for (i = 0; i < n; ++i)
        father[i] = i;
}

int find(int x)
{
    if (father[x] == x)
        return x;
    return father[x] = find(father[x]);
}

void join(int u, int v)
{
    u = find(u);
    v = find(v);
    if (u == v)
        return;
    father[v] = u;
}

bool isSame(int u, int v)
{
    u = find(u);
    v = find(v);
    return u == v;
}

int main(void)
{
    init();
    int N, M;
    cin >> N >> M;
    int s, t;
    while (M --)
    {
        cin >> s >> t;
        join(s, t);
    }
    cin >> s >> t;

    cout << isSame(s, t) << endl;
    return 0;
}



/**************************************************************
    Problem: 1179
    User: wunaiqiezixin
    Language: C++
    Result: 正确
    Time:42 ms
    Memory:2176 kb
****************************************************************/
```

> 2026-07-29
```CPP
#include <iostream>
#include <vector>
using namespace std;

vector<int> father(101);
void init(){
    for (int i = 0; i < 101; ++i){
        father[i] = i;
    }
}

int find(int x){
    return father[x] == x ? x : find(father[x]);
}

void join(int u, int v){
    u = find(u);
    v = find(v);
    if (u != v) father[v] = u;
}

bool same(int u, int v){
    u = find(u);
    v = find(v);
    return u == v;
}

int main(){
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    int n, m;
    cin >> n >> m;
    init();
    while (m--){
        int u, v;
        cin >> u >> v;
        join(u, v);
    }
    int u, v;
    cin >> u >> v;
    cout << same(u, v) << '\n';
    return 0;
}
```
