
C++ STL (Standard Template Library) 是算法竞赛中的得力助手，它提供了高效、可靠的数据结构和算法，能极大简化代码编写和调试过程。熟练掌握常用 STL 是提升解题效率的关键。

### 一、 常用容器 (Containers)

容器用于存储数据。选择合适的容器取决于数据的访问、插入、删除需求以及是否有序。

1.  **`vector`**
    *   **头文件:** `<vector>`
    *   **本质:** 动态数组（连续内存）
    *   **特点:**
        *   支持快速随机访问 (O(1))：`v[i]`
        *   尾部插入/删除 (均摊 O(1))：`push_back()`, `pop_back()`
        *   中间插入/删除 (O(N))：`insert()`, `erase()`
    *   **常用操作:**
        *   `v.push_back(x)`: 尾部添加元素 x。
        *   `v.pop_back()`: 删除尾部元素。
        *   `v.size()`: 获取元素个数。
        *   `v.clear()`: 清空容器。
        *   `v.empty()`: 判断是否为空。
        *   `v[i]`: 访问第 i 个元素（不进行边界检查）。
        *   `v.at(i)`: 访问第 i 个元素（进行边界检查，较慢）。
        *   `v.begin()`, `v.end()`: 获取指向首元素和尾元素之后位置的迭代器。
        *   `v.resize(n, val)`: 改变大小为 n，新元素用 val 初始化。
    *   **应用场景:** 替代普通数组，实现邻接表存图，需要随机访问和尾部快速操作的场景。

2.  **`string`**
    *   **头文件:** `<string>`
    *   **本质:** `vector<char>` 的特化版，用于字符串处理。
    *   **特点:** 支持 `vector` 的大部分操作，并有专用字符串操作。
    *   **常用操作:**
        *   `s += t`: 字符串拼接。
        *   `s.substr(pos, len)`: 获取子串。
        *   `s.find(t)`: 查找子串 t 的首次出现位置。
        *   `s.length()`, `s.size()`: 获取长度。
        *   可以直接使用 `cin >> s`, `cout << s`。
        *   `getline(cin, s)`: 读取一整行（包括空格）。
    *   **应用场景:** 字符串输入输出、处理、查找、匹配等。

3.  **`queue`**
    *   **头文件:** `<queue>`
    *   **本质:** 队列 (FIFO - First In First Out)。
    *   **特点:** 只能在队尾插入，队头删除。
    *   **常用操作:**
        *   `q.push(x)`: 元素 x 入队（队尾）。
        *   `q.pop()`: 队头元素出队（无返回值）。
        *   `q.front()`: 获取队头元素（不出队）。
        *   `q.back()`: 获取队尾元素（C++11）。
        *   `q.size()`: 获取元素个数。
        *   `q.empty()`: 判断是否为空。
    *   **应用场景:** BFS (广度优先搜索)。

4.  **`priority_queue`** (优先队列)
    *   **头文件:** `<queue>`
    *   **本质:** 堆 (Heap)。默认是大根堆。
    *   **特点:** 可以快速获取并弹出优先级最高（默认是最大值）的元素。
    *   **常用操作:**
        *   `pq.push(x)`: 元素 x 入队（并维护堆性质）。(O(log N))
        *   `pq.pop()`: 弹出优先级最高的元素。(O(log N))
        *   `pq.top()`: 获取优先级最高的元素。(O(1))
        *   `pq.size()`, `pq.empty()`
    *   **定义小根堆:** `priority_queue<int, vector<int>, greater<int>> pq;`
    *   **定义结构体/pair 优先队列:** 需要重载 `<` 运算符或提供自定义比较函数。
    *   **应用场景:** Dijkstra 算法、Prim 算法、堆排序、模拟、需要动态维护最值的场景。

5.  **`stack`**
    *   **头文件:** `<stack>`
    *   **本质:** 栈 (LIFO - Last In First Out)。
    *   **常用操作:**
        *   `st.push(x)`: 元素 x 入栈。
        *   `st.pop()`: 栈顶元素出栈（无返回值）。
        *   `st.top()`: 获取栈顶元素（不出栈）。
        *   `st.size()`, `st.empty()`
    *   **应用场景:** DFS (深度优先搜索)、表达式求值、括号匹配、单调栈。

6.  **`deque`** (双端队列)
    *   **头文件:** `<deque>`
    *   **特点:** 支持在队头和队尾快速插入/删除 (O(1))，支持随机访问 (但比 `vector` 慢)。
    *   **常用操作:** 包含 `vector` 和 `queue` 的大部分操作，加上 `push_front()`, `pop_front()`。
    *   **应用场景:** 需要两端操作的场景，如某些 BFS 变种，维护滑动窗口（但通常有更优方法）。不常用。

7.  **`set` / `multiset`**
    *   **头文件:** `<set>`
    *   **本质:** 基于红黑树的有序集合。
    *   **特点:**
        *   元素自动排序且唯一 (`set`) 或允许重复 (`multiset`)。
        *   插入、删除、查找均为 O(log N)。
    *   **常用操作:**
        *   `s.insert(x)`: 插入元素 x。
        *   `s.erase(x)`: 删除值为 x 的所有元素 (`multiset`) 或单个元素 (`set`)。也可传入迭代器删除。
        *   `s.count(x)`: 计算值为 x 的元素个数。
        *   `s.find(x)`: 查找值为 x 的元素，返回迭代器或 `s.end()`。
        *   `s.lower_bound(x)`: 查找 >= x 的第一个元素。
        *   `s.upper_bound(x)`: 查找 > x 的第一个元素。
        *   `s.size()`, `s.clear()`, `s.empty()`
    *   **应用场景:** 去重、判断元素是否存在、维护有序集合、自动排序。

8.  **`map` / `multimap`**
    *   **头文件:** `<map>`
    *   **本质:** 基于红黑树的有序键值对映射。
    *   **特点:**
        *   根据键 (key) 自动排序。键唯一 (`map`) 或允许重复 (`multimap`)。
        *   插入、删除、查找均为 O(log N)。
    *   **常用操作:**
        *   `m[key] = value`: 插入或更新键值对（最常用）。
        *   `m.insert({key, value})`: 插入（返回 `pair<iterator, bool>`）。
        *   `m.erase(key)`: 删除键为 key 的元素。也可传入迭代器删除。
        *   `m.count(key)`: 计算键为 key 的元素个数。
        *   `m.find(key)`: 查找键为 key 的元素，返回迭代器或 `m.end()`。
        *   `m.lower_bound(key)`, `m.upper_bound(key)`
        *   `m.size()`, `m.clear()`, `m.empty()`
        *   遍历：`for (auto const& [key, val] : m)` (C++17) 或 `for (auto it = m.begin(); it != m.end(); ++it)` 访问 `it->first`, `it->second`。
    *   **应用场景:** 按键存储和查找信息、离散化、频率统计（需要有序时）。

9.  **`unordered_set` / `unordered_multiset`**
    *   **头文件:** `<unordered_set>`
    *   **本质:** 基于哈希表的集合。
    *   **特点:**
        *   元素无序。唯一 (`unordered_set`) 或允许重复 (`unordered_multiset`)。
        *   插入、删除、查找平均时间复杂度 O(1)，最坏 O(N)。
    *   **常用操作:** 类似 `set`/`multiset`，但没有 `lower_bound`/`upper_bound`。
    *   **应用场景:** 只需要快速判断元素是否存在、去重，且不关心顺序时。比 `set` 更快。

10. **`unordered_map` / `unordered_multimap`**
    *   **头文件:** `<unordered_map>`
    *   **本质:** 基于哈希表的键值对映射。
    *   **特点:**
        *   键无序。唯一 (`unordered_map`) 或允许重复 (`unordered_multimap`)。
        *   插入、删除、查找平均时间复杂度 O(1)，最坏 O(N)。
    *   **常用操作:** 类似 `map`/`multimap`，但没有 `lower_bound`/`upper_bound`。
    *   **应用场景:** 快速按键存储和查找信息、频率统计（最常用）、离散化。比 `map` 更快。**注意：** C++11 之后才能用，自定义类型做 key 需要提供哈希函数和等于函数。

11. **`bitset`**
    *   **头文件:** `<bitset>`
    *   **本质:** 固定大小的位集合。
    *   **特点:** 空间效率高，支持位运算。
    *   **常用操作:**
        *   `bitset<N> b;`: 定义长度为 N 的 bitset。
        *   `b[i]`: 访问第 i 位。
        *   `b.set()`: 全部置 1。
        *   `b.reset()`: 全部置 0。
        *   `b.flip()`: 全部取反。
        *   `b.count()`: 计算 1 的个数。
        *   支持 `&`, `|`, `^`, `~`, `<<`, `>>` 位运算。
    *   **应用场景:** 状态压缩、位运算相关问题、优化空间（代替 `bool` 数组）。

### 二、 常用算法 (Algorithms)

算法通常作用于迭代器指定的范围（如 `[begin(), end())`）。

*   **头文件:** `<algorithm>` (大部分在此) `<numeric>` (如 `accumulate`, `iota`)

1.  **`sort(begin, end, [cmp])`**:
    *   对范围 `[begin, end)` 内的元素进行排序。
    *   默认升序。可提供自定义比较函数 `cmp`。
    *   时间复杂度：平均 O(Nlog N)。
    *   **应用:** 最常用的算法之一。

2.  **`reverse(begin, end)`**:
    *   翻转范围 `[begin, end)` 内的元素。
    *   时间复杂度：O(N)。

3.  **`unique(begin, end)`**:
    *   **去除相邻**的重复元素，返回去重后范围的末尾迭代器。
    *   **注意:** 通常需要先排序 `sort`。
    *   **用法:** `int new_len = unique(v.begin(), v.end()) - v.begin; v.resize(new_len);`
    *   时间复杂度：O(N)。
    *   **应用:** 数组/vector 去重。

4.  **`lower_bound(begin, end, val, [cmp])`**:
    *   在 **已排序** 范围 `[begin, end)` 内查找第一个 **大于等于** `val` 的元素，返回其迭代器。
    *   若找不到，返回 `end`。
    *   时间复杂度：O(log N)。

5.  **`upper_bound(begin, end, val, [cmp])`**:
    *   在 **已排序** 范围 `[begin, end)` 内查找第一个 **严格大于** `val` 的元素，返回其迭代器。
    *   若找不到，返回 `end`。
    *   时间复杂度：O(log N)。
    *   **应用:** 二分查找特定值或满足条件的边界。`upper_bound - lower_bound` 可得等于 `val` 的元素个数。

6.  **`binary_search(begin, end, val, [cmp])`**:
    *   在 **已排序** 范围 `[begin, end)` 内查找是否存在 `val`。
    *   返回 `bool` 值。
    *   时间复杂度：O(log N)。

7.  **`max_element(begin, end, [cmp])` / `min_element(begin, end, [cmp])`**:
    *   返回范围 `[begin, end)` 内最大/最小元素的迭代器。
    *   时间复杂度：O(N)。

8.  **`accumulate(begin, end, init, [op])`**:
    *   计算范围 `[begin, end)` 内元素的累加和（或其他二元操作），初始值为 `init`。
    *   **头文件:** `<numeric>`
    *   时间复杂度：O(N)。
    *   **应用:** 求和、求积等。

9.  **`fill(begin, end, val)` / `fill_n(begin, n, val)`**:
    *   将范围 `[begin, end)` 或从 `begin` 开始的 `n` 个元素赋值为 `val`。
    *   比 `memset` 更通用（可以赋非 0/-1 的值），但对非字节类型可能稍慢。
    *   时间复杂度：O(N)。

10. **`find(begin, end, val)`**:
    *   在范围 `[begin, end)` 内查找第一个等于 `val` 的元素，返回其迭代器。
    *   若找不到，返回 `end`。
    *   时间复杂度：O(N)。
    *   **注意:** 在 `set`/`map` 等容器上，使用其自带的 `find` 成员函数效率更高 (O(log N) 或 O(1))。

11. **`next_permutation(begin, end, [cmp])`**:
    *   将范围 `[begin, end)` 内的元素重新排列为字典序的下一个排列。
    *   如果当前已是最大排列，返回 `false` 并变为最小排列（通常是排序后的状态）。
    *   时间复杂度：平均 O(1)？（一次调用是 O(N)，但遍历所有排列的总复杂度符合预期）
    *   **应用:** 生成全排列。需要先排序以从最小排列开始。

12. **`iota(begin, end, val)`**:
    *   用从 `val` 开始递增的值填充范围 `[begin, end)`。`v[0]=val, v[1]=val+1, ...`
    *   **头文件:** `<numeric>`
    *   时间复杂度：O(N)。
    *   **应用:** 初始化索引数组等。

### 三、 实用工具 (Utilities)

1.  **`pair`**
    *   **头文件:** `<utility>` (通常被其他头文件包含)
    *   **定义:** `pair<T1, T2> p;`
    *   **访问:** `p.first`, `p.second`
    *   **创建:** `make_pair(a, b)` 或 `{a, b}` (C++11)
    *   **特点:** 自带比较运算符（先比较 `first`，相同再比较 `second`），可以直接用于 `sort`, `set`, `map` 等。
    *   **应用:** 存储坐标、键值对（临时）、迪杰斯特拉存 `(distance, node)` 等。

2.  **`tuple`** (C++11)
    *   **头文件:** `<tuple>`
    *   **定义:** `tuple<T1, T2, ..., Tn> t;`
    *   **访问:** `get<i>(t)` (i 是编译期常量)
    *   **创建:** `make_tuple(a, b, c)` 或 `{a, b, c}`
    *   **特点:** `pair` 的扩展，可存多个元素，自带比较运算符（逐个比较）。
    *   **应用:** 需要绑定三个或更多值时。

3.  **输入输出 (`cin`, `cout`)**
    *   **头文件:** `<iostream>`
    *   **优化 (竞赛常用):** 在 `main` 函数开头加上
        ```cpp
        ios_base::sync_with_stdio(false);
        cin.tie(NULL);
        cout.tie(NULL); // 有些情况也加速 cout
        ```
        可以大幅提高 `cin`/`cout` 速度，接近 `scanf`/`printf`。但之后**不能**混用 C 风格 I/O (`scanf`, `printf`, `getchar`, `puts` 等)。

4.  **`max`, `min`**
    *   **头文件:** `<algorithm>`
    *   **用法:** `max(a, b)`, `min(a, b)`。C++11 支持 `max({a, b, c, ...})`。
    *   **应用:** 随处可见的取最值。

5.  **`swap(a, b)`**
    *   **头文件:** `<utility>` 或 `<algorithm>`
    *   **用法:** 交换 a 和 b 的值。通常比手动交换更高效（尤其对容器）。

### 四、 通用技巧与注意事项

1.  **头文件:**
    *   竞赛中为图方便，常使用 `#include <bits/stdc++.h>`，它包含了几乎所有常用 STL 头文件。但缺点是编译慢，且非标准。正规场合应包含具体头文件。
2.  **命名空间:**
    *   `using namespace std;` 在竞赛中常用以简化代码，避免频繁写 `std::`。但可能引起命名冲突，大型项目中不推荐。
3.  **迭代器:**
    *   理解 `begin()` 指向第一个元素，`end()` 指向最后一个元素**之后**的位置。
    *   `auto` 关键字 (C++11) 能极大简化迭代器的书写：`for (auto it = v.begin(); it != v.end(); ++it)` 或范围 `for` 循环 `for (auto& x : v)`。
4.  **复杂度意识:**
    *   选择 STL 组件时，务必清楚其关键操作的时间复杂度（平均和最坏情况）。这是决定算法能否 TLE 的关键。
    *   例如，对 `vector` 大量中间插入是 O(N^2) 的，此时应考虑 `list` (虽然 `list` 竞赛不常用) 或其他结构。需要快速查找时，优先 `unordered_set/map` (O(1))，其次 `set/map` (O(log N))，最后才是 `vector/array` 的 `find` (O(N))。
5.  **自定义比较函数:**
    *   `sort`, `priority_queue`, `set`, `map` 等都支持传入自定义比较函数（或重载运算符）来改变排序/比较逻辑。

