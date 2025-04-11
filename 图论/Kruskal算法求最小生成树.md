![[Pasted image 20250411225527.png]]

## 基本思路
1. 对所有边从小到大排序
2. 遍历所有边，如果边对应的两点不在一个集合中（同一个生成子树中），则将两点并入一个集合，并加上边的权重
3. 如果最后最小生成树中的边数小于$n - 1$，则不是一个合法的
4. 最后得到`res`即为最小生成树的边权和

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 200010;
const int INF = 0x3f3f3f3f;
int p[N];
int n, m;
struct edge
{
	int a, b, w;
	bool operator < (const edge &W) const
	{
		return w < W.w;
	}
}edges[N];

int find(int x)
{
	if(p[x] != x) p[x] = find(p[x]);
	return p[x];
}

int Kruskal()
{
	int res = 0;
	int cnt = 0;
	sort(edges, edges + m);
	for(int i = 1; i <= m; i++) p[i] = i; // 初始化并查集
	
	for(int i = 0; i < m; i++)
	{
		int a = edges[i].a, b = edges[i].b, w = edges[i].w;
		a = find(a), b = find(b);
		if(a != b)
		{
			p[a] = b;
			res += w;
			cnt++;
		}
	}
	
	if(cnt < n - 1) res = INF;
	return res;
}

int main()
{
	cin >> n >> m;
	for(int i = 0; i < m; i++)
	{
		int a, b, w;
		cin >> a >> b >> w;
		edges[i] = {a, b, w};
	}	
	int res = Kruskal();
	if(res == INF) puts("impossible");
	else cout << res << endl;
	return 0;
}
```