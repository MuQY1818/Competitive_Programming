## 算法概述
用于解决**负权边**的最短路问题
时间复杂度$O(n^2)$
Bellman-Ford算法的核心思想是通过**对图中所有边进行反复松弛操作**，逐步更新从源点到各顶点的最短路径估计值，直到找到最短路径（或检测出负权环）。

## 大致流程
```pseudocode
for k 次
	for 所有边
		update(边)
```

在Bellman-Ford算法的每一轮松弛操作（即遍历所有边进行更新）中，有一个重要的实现细节：当计算通过边 `(u, v)` 更新顶点 `v` 的距离 `dist[v]` 时，所依赖的 `dist[u]` 值必须是**本轮迭代开始前**的值（也就是上一轮迭代结束时的结果）。

如果在同一轮迭代中，允许一个顶点更新后的 `dist` 值立即被用于计算后续边的松弛，就会发生“串联更新”。这会破坏Bellman-Ford算法的核心逻辑，即第 `k` 轮迭代保证找到的是包含**最多** `k` 条边的最短路径。错误的即时更新可能导致算法提前“找到”看似更短的路径，或者影响负权环的正确检测。

为了确保每一轮松弛计算都基于**上一轮**的完整结果，一个标准的实现技巧是使用一个**备份（`backup`）数组**。在每轮迭代开始时，可以将当前的 `dist` 数组复制到 `backup` 数组中。然后，在本轮的所有松弛计算中，都**读取 `backup` 数组中的值**（例如 `backup[u] + w(u, v)`），并将计算出的可能更短的路径长度**写入到原始的 `dist` 数组**（例如 `dist[v] = ...`）。这样就保证了本轮所有更新计算的独立性，它们都统一基于迭代开始时的状态。

如何检测负环呢？当我们$n-1$次循环后，发现还可以更新dist，则说明出现了负环！

## 代码实现
下面代码实现了用Bellman-Ford算法求得从$1$号点到$n$号点的最多经过$k$条边的最短距离
```cpp
#include <bits/stdc++.h>  
using namespace std;  
const int N = 505, M = 10005;  
// 0x3f3f3f3f 是常用的表示无穷大的整数值，便于memset和防止加法溢出  
const int INF = 0x3f3f3f3f;  
int n, m, k; // n: 点数, m: 边数, k: Bellman-Ford迭代次数限制（通常为n-1，这里允许指定）  

// 边的结构体定义  
struct edge{  
	int a, b, w; // a: 起点, b: 终点, w: 权重  
}Edge[M]; // 存储所有边的数组  

int dist[N];    // dist[i] 存储从源点1到顶点i的最短距离估计值  
int backup[N];  // backup数组用于备份上一轮的dist值，防止串联更新  

// Bellman-Ford算法核心函数  
void bellman_ford()  
{  
	// 初始化源点距离为0  
	dist[1] = 0;  
	// 进行k轮迭代 (或者 n-1 轮)  
	for(int i = 0; i < k; i++)  
	{  
		// 备份本轮开始前的dist数组，防止在同一轮内发生串联更新  
		memcpy(backup, dist, sizeof dist);  
		// 遍历所有m条边进行松弛操作  
		for(int j = 1; j <= m; j++) // 注意：循环变量应为j  
		{  
			int a = Edge[j].a, b = Edge[j].b, w = Edge[j].w; // 从Edge[j]获取边信息  
			// 松弛操作：尝试用经过边(a, b)的路径更新到达b点的最短距离  
			// 使用backup[a]确保是基于上一轮的结果进行更新  
			dist[b] = min(dist[b], backup[a] + w);  
		}  
	}  
}  

int main()  
{  
	ios::sync_with_stdio(false); // 加速cin/cout  
	cin.tie(0);  

	// 初始化所有点的距离为无穷大  
	memset(dist, 0x3f, sizeof dist); // 使用0x3f进行字节填充得到INF  

	cin >> n >> m >> k; // 读入点数、边数、迭代次数  

	// 读入所有边的信息  
	for(int i = 1; i <= m; i++)  
	{  
		cin >> Edge[i].a >> Edge[i].b >> Edge[i].w;  
	}  

	// 执行Bellman-Ford算法  
	bellman_ford();  

	// 判断终点n是否可达  
	// 注意：这里判断>= INF / 2 而不是 == INF  
	// 是因为在存在负权边的情况下，即使一个点不可达，  
	// 它的距离也可能从INF被更新为一个略小于INF但仍很大的数。  
	// 使用 INF / 2 可以鲁棒地判断是否仍然为“不可达”（或路径长极大）。  
	if(dist[n] >= INF / 2) puts("impossible");  
	else cout << dist[n] << endl; // 输出到终点的最短距离  

	return 0;  
}
```