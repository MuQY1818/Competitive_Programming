## 基本思路

1.  **初始化:** 源点距离为0，其他点无穷大。源点入队，标记在队中。
2.  **循环：** 只要队列不空：
    *   取出队头点 `u`，标记不在队中。
    *   对 `u` 的每个邻居 `v`：
        *   如果通过 `u` 能让 `v` 的距离变短 (`dist[u] + w(u,v) < dist[v]`)：
            *   更新 `dist[v]`。
            *   如果 `v` 不在队中，让 `v` 入队并标记。
3.  **结束：** 队列为空，`dist` 数组即为最短路径。

从代码中可以看出，这是Bellman_Ford算法的优化，优化点在于不会对更新后不变小的点进行松弛操作，减少了松弛的次数。spfa算法酷似BFS，可以用BFS的形式来记忆算法模板

## Code
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 1e5 + 10;
typedef pair<int, int> PII;
vector<vector<PII>> e(N);
int dist[N], st[N]; // 距离、队中标记
int n, m;

void spfa()
{
	queue<int> q;
	dist[1] = 0;
	q.push(1);
	st[1] = true;
	while(q.size())
	{
		int t = q.front();
		q.pop();
		st[t] = false;
		for(auto edge: e[t])
		{
			int to_id = edge.first, w = edge.second;
			if(dist[to_id] > dist[t] + w)
			{
				dist[to_id] = dist[t] + w;
				if(!st[to_id]){
				st[to_id] = true;
				q.push(to_id);					
				}
			}
		}
	}
}

int main()
{
	ios::sync_with_stdio(false);
	cin.tie(nullptr);
	memset(dist, 0x3f, sizeof dist);
	cin >> n >> m;
	for(int i = 0; i < m; i++)
	{
		int a, b, c;
		cin >> a >> b >> c;
		e[a].push_back({b, c});
	}
	spfa();
	if(dist[n] == 0x3f3f3f3f) puts("impossible");
	else cout << dist[n] << endl;
	return 0;
}
```

## spfa判断负环
判断负环利用了 Bellman-Ford 算法的一个性质：在一个包含 `n` 个节点的图中，如果不存在负环，那么任意两点之间的最短路径最多包含 `n-1` 条边。
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 1e5 + 10;
typedef pair<int, int> PII;
vector<vector<PII>> e(N);
int dist[N], st[N]; // 距离、队中标记
int cnt[N]; // cnt[i]表示从1走到i经过的边的个数
int n, m;

void spfa()
{
	queue<int> q;
	/*
	 * 这里与spfa求最短路是不一样的！
	 * 由于有的点是从1开始走不到的，
	 * 所以我们需要吧所有的点放入队列！
	*/
	for(int i = 1; i <= n; i++)
	{
		q.push(i);
		st[i] = true;
	}
	while(q.size())
	{
		int t = q.front();
		q.pop();
		st[t] = false;
		for(auto edge: e[t])
		{
			int to_id = edge.first, w = edge.second;
			if(dist[to_id] > dist[t] + w)
			{
				dist[to_id] = dist[t] + w;
				cnt[to_id] = cnt[t] + 1;// 多了这一步
				if(cnt[to_id] >= n)
				{
					puts("Yes");
					return;
				}
				if(!st[to_id]){
				st[to_id] = true;
				q.push(to_id);					
				}
			}
		}
	}
	puts("No");
	return;
}

int main()
{
	ios::sync_with_stdio(false);
	cin.tie(nullptr);
	memset(dist, 0x3f, sizeof dist);
	cin >> n >> m;
	for(int i = 0; i < m; i++)
	{
		int a, b, c;
		cin >> a >> b >> c;
		e[a].push_back({b, c});
	}
	spfa();
	return 0;
}
```