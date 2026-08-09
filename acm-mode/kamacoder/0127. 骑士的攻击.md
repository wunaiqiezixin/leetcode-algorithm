# 127. 骑士的攻击
[题目链接](https://kamacoder.com/problempage.php?pid=1203)

---
>广度优先搜索优化算法

```CPP
#include <iostream>
#include <vector>
#include <queue>
#include <cstring>
using namespace std;
int moves[1001][1001];
int dir[8][2] = {-2, 1, -2, -1, -1, 2, -1, -2, 1, 2, 1, -2, 2, 1, 2, -1};
int b1, b2;//终点坐标
// F = G + H
// G = 从起点到该节点路径消耗
// H = 该节点到终点的预估消耗
struct Node
{
    int x, y;
    int f, g, h;
    //小顶堆
    bool operator<(const Node& other) const 
    {
        return other.f < f;
    }
};

int Heuristic(const Node& node)//欧拉距离
{
    return (node.x - b1)*(node.x - b1) + (node.y - b2)*(node.y - b2);
}

priority_queue<Node> que;

void aStar(const Node& node)//node到终点的距离
{
    que.push(node);
    //记录距离的数组初始化为-1
    memset(moves, -1, sizeof(moves));
    moves[node.x][node.y] = 0;
    while (!que.empty())
    {
        Node cur = que.top();
        que.pop();
        //如果当前节点即为终点，任务完成
        if (cur.x == b1 && cur.y == b2)
            return;
        //搜索周围
        for (int k = 0; k < 8; ++k)
        {
            int dx = cur.x + dir[k][0];
            int dy = cur.y + dir[k][1];
            if (dx < 1 || dy < 1 || dx > 1000 || dy > 1000)
                continue;//越界，跳过
            if (moves[dx][dy] == -1)//节点没有被访问过
            {
                //记录该节点到源点的距离
                moves[dx][dy] = moves[cur.x][cur.y] + 1;
                //将该节点加入到优先队列
                Node newNode;
                newNode.x = dx, newNode.y = dy;
                newNode.g = cur.g + 5;//源点到节点的实际距离
                newNode.h = Heuristic(newNode);//预估距离
                newNode.f = newNode.g + newNode.h;
                que.push(newNode);
            }
        }
    }
}

int main()
{
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    cin >> n;
    while (n --)
    {
        int a1, a2;
        cin >> a1 >> a2 >> b1 >> b2;
        Node Start;
        Start.x = a1, Start.y = a2;
        Start.g = 0;
        Start.h = Heuristic(Start);
        Start.f = Start.g + Start.h;
        aStar(Start);
        cout << moves[b1][b2] << '\n';
        while (!que.empty()) que.pop();
    }
}




```
