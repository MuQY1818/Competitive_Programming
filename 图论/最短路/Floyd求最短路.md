## 基本思想

**1. 算法目的**
 解决 **所有顶点对之间 (All-Pairs)** 的最短路径问题。也就是说，在一个给定的加权图中，它能找出**任意两个**指定顶点之间的最短路径长度。

**2. 核心思想：动态规划 (Dynamic Programming)**

Floyd 算法的精髓在于**动态规划**和**迭代**。它思考问题的角度是：

> 从顶点 `i` 到顶点 `j` 的最短路径，要么是**不经过**某个中间顶点 `k` 的路径，要么是**必须经过**中间顶点 `k` 的路径。

它通过不断增加允许使用的“中间顶点”的集合，来逐步逼近最终的最短路径。

**3. 状态定义与转移**

*   我们定义 `dist[k][i][j]` 为：从顶点 `i` 到顶点 `j`，**只允许**使用编号在 `1` 到 `k` 范围内的顶点作为**中间顶点**时，所能得到的**最短路径长度**。

*   **状态转移方程:**
    `dist[k][i][j] = min( dist[k-1][i][j], dist[k-1][i][k] + dist[k-1][k][j] )`

    *   **解释:**
        *   `dist[k-1][i][j]`：表示从 `i` 到 `j`，不允许经过 `k` (只允许经过 `1` 到 `k-1`) 的最短路径长度。
        *   `dist[k-1][i][k] + dist[k-1][k][j]`：表示从 `i` 先到 `k` (只允许经过 `1` 到 `k-1`)，再从 `k` 到 `j` (只允许经过 `1` 到 `k-1`) 的路径长度之和。这条路径是强制经过了顶点 `k` 的。
        *   `min(...)`：取这两者中的较小值，即为考虑了顶点 `k` 作为中间顶点后，从 `i` 到 `j` 的新的最短路径长度。

*   **空间优化:** 我们可以发现，计算 `dist[k][...]` 时只依赖于 `dist[k-1][...]`。因此，可以省略掉第一个维度 `k`，直接用一个二维数组 `dist[i][j]` 来存储当前的、不断更新的最短路径长度。状态转移方程简化为：
    `dist[i][j] = min( dist[i][j], dist[i][k] + dist[k][j] )`
    这个更新需要在 `k` 的循环内部进行。

**4. 算法步骤**

假设图中有 `N` 个顶点，编号从 `1` 到 `N`（或 `0` 到 `N-1`）。

1.  **初始化 `dist` 矩阵:**
    *   创建一个 `N x N` 的二维数组 `dist`。
    *   对于所有的 `i`，`dist[i][i] = 0` (自己到自己的距离是 0)。
    *   对于所有直接相连的边 `(i, j)`，`dist[i][j] = weight(i, j)` (边的权重)。
    *   对于所有没有直接相连的顶点 `i` 和 `j`，`dist[i][j] = infinity` (一个非常大的数，表示初始时不可达)。

2.  **迭代更新:**
    *   使用**三重嵌套循环**：
        ```
        for k from 1 to N: // 枚举中间顶点 k
            for i from 1 to N: // 枚举起点 i
                for j from 1 to N: // 枚举终点 j
                    // 尝试通过中间点 k 更新 i 到 j 的距离
                    dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
        ```
    *   **关键:** `k` 的循环必须在**最外层**。这保证了在考虑使用 `k` 作为中间点时，`dist[i][k]` 和 `dist[k][j]` 都已经是只使用了 `1` 到 `k-1` 作为中间点的最优结果。

3.  **结果:**
    *   当所有循环结束后，`dist[i][j]` 就存储了从顶点 `i` 到顶点 `j` 的实际最短路径长度。
## Code
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 205;
int dist[N][N];
int n, m, k;

void floyd()
{
	for(int k = 1; k <= n; k++)
		for(int i = 1; i <= n; i++)
			for(int j = 1; j <= n; j++)
				dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
}


int main()
{
	ios::sync_with_stdio(false);
	cin.tie(nullptr);
	cin >> n >> m >> k;
    for(int i = 1; i <= n; i++)
        for(int j = 1; j <= n; j++)
        {
            if(i == j) dist[i][j] = 0;
            else dist[i][j] = 0x3f3f3f3f;
        }
	for(int i = 0; i < m; i++)
	{
		int a, b, c;
		cin >> a >> b >> c;
		dist[a][b] = min(dist[a][b], c); // 记得这里要取最短的
	}
	floyd();
	while(k--)
	{
		int a, b;
		cin >> a >> b;
		if(dist[a][b] > 0x3f3f3f3f / 2) cout << "impossible" << endl;
		else cout << dist[a][b] << endl;
	}
	return 0;
}
```

