## 经典例题：八数码问题

### 问题简述
在一个 3x3 的网格中，有数字 1 到 8 和一个空格（用 'x' 表示）。你可以将空格与其上、下、左、右相邻的数字进行交换。**目标是：** 从一个给定的初始排列开始，通过一系列交换操作，用**最少的步数**将网格变换成一个预先定义好的目标排列（通常是 1 到 8 按顺序排列，空格在右下角）。

例如：
```
1 2 3
x 4 6
7 5 8
```
#### 输入格式
输入占一行，将 3×3 的初始网格描绘出来。
例如，如果初始网格如下所示：
```
1 2 3 
x 4 6 
7 5 8 
```

则输入为：`1 2 3 x 4 6 7 5 8`
#### 输出格式
输出占一行，包含一个整数，表示最少交换次数。
如果不存在解决方案，则输出 −1。

## Code
```cpp
#include <bits/stdc++.h>
using namespace std;
unordered_map<string, int> d;
int dir[4][2] = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};

int bfs(string start)
{
	string end = "12345678x";
	queue<string> q;
	q.push(start);
	d[start] = 0; // 初始化初始状态步数为0
	while(!q.empty())
	{
		auto t = q.front();
		q.pop();
		int dis = d[t];
		if(t == end) return dis; // 最终状态，返回步数
		int idx = t.find('x'); // 找到x的位置
		int x = idx / 3, y = idx % 3;
		int nx, ny;
		for(int i = 0; i < 4; i++)
		{
			nx = x + dir[i][0];
			ny = y + dir[i][1];
			if(nx >= 0 && nx < 3 && ny >= 0 && ny < 3) // 判断是否在区域内
			{
				swap(t[ny + nx * 3], t[y + x * 3]);
				if(!d.count(t)) // 如果第一次搜到这种情况，则记录步数并入队
				{
					q.push(t);
					d[t] = dis + 1;
				}
				swap(t[ny + nx * 3], t[y + x * 3]); // 还原状态
			}
		}
	}
	return -1;
}

int main()
{
	ios::sync_with_stdio(false);
	cin.tie(nullptr);
	string c, start = "";
	for(int i = 0; i < 9; i++)
	{
		cin >> c;
		start += c;
	}
	cout << bfs(start) << endl;
	return 0;
}
```