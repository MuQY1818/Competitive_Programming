一句话总结`Dijkstra`算法就是：
从**单一源点**出发，迭代地选择当前**已知距离最短**的**未访问**顶点，并用其**更新邻居顶点**的距离
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 2e5;
int n, m;
typedef pair<int, int> PII;
vector<vector<PII>> e(N);
int dist[N]; // dist[i]表示i号节点到源点的位置
bool vis[N]; // 是否已经找到最短路径

void Dijkstra()
{
	priority_queue<PII, vector<PII>, greater<PII>> heap;
	memset(dist, 0x3f, sizeof dist);
	dist[1] = 0;
	heap.push({0, 1}); // heap存储的pair是{dist, id}, pair根据第一个值进行排列 
	while(heap.size())
	{
		auto t = heap.top();
		heap.pop(); // 一定要记住！取出后就要pop出去！！
		int dis_val = t.first, now_id = t.second;
		if(vis[now_id]) continue; // 如果当前节点已经找到过最短路，则跳过 
		vis[now_id] = true;
		for(auto edge: e[now_id])
		{
			int to_id = edge.first, w = edge.second;
			if(dist[to_id] > dis_val + w)
			{
				dist[to_id] = dis_val + w;
				heap.push({dist[to_id], to_id});
			}
		}
	}
	if(dist[n] == 0x3f3f3f3f) puts("-1");
	else cout << dist[n] << endl;
	return;
}

int main()
{
	ios::sync_with_stdio(false);
	cin.tie(nullptr);
	cin >> n >> m;
	for(int i = 0; i < m; i++)
	{
		int a, b, c;
		cin >> a >> b >> c;
		e[a].push_back(make_pair(b, c));
	}
	Dijkstra();
	return 0;
}	 
```