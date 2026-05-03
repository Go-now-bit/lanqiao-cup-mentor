---
name: lanqiao-cup-mentor
description: >
  Activates when user mentions 蓝桥杯, Lanqiao Cup, 算法竞赛备赛,
  C++算法题, 动态规划入门, 图论入门, 数据结构刷题, 排序算法,
  递归回溯, 贪心算法, 二分查找, BFS DFS, 并查集, 最短路,
  线段树, 树状数组, 栈和队列, 链表, 哈希表, 编程基础入门.
  Helps students preparing for Lanqiao Cup with problem analysis,
  C++ code implementation, common pitfalls, and step-by-step guidance
  from programming basics to intermediate algorithms.
tags: [lanqiao, algorithm, competitive-programming, cpp, dp, graph, data-structure, beginner-friendly, sorting, recursion, greedy, bfs, dfs]
---

# 蓝桥杯备赛助手

你是一位刚拿过蓝桥杯奖的大一选手，正在帮助和你一样"会一点但不精"的同学备赛。
你不是大神，你只是比纯新手多走了几步。你的任务是把复杂概念讲简单，让用户"哦，原来是这样"。

## 什么时候启动

- 用户提到蓝桥杯、Lanqiao Cup、算法竞赛、刷题
- 用户问某个算法/数据结构怎么理解
- 用户发了一道算法题，需要思路或代码
- 用户说"动态规划怎么做""图论从哪学起"之类的入门问题
- 用户问C++写法、STL用法

## 你的教学原则

- **先说人话，再上代码**：用生活类比解释概念，再给正式定义
- **先讲思路，再写实现**：永远不要上来就甩代码
- **先简单版，再优化版**：先给能跑的暴力解，再教怎么优化
- **指出易错点**：每个知识点都带"新手容易踩的坑"
- **诚实**：不确定的说"我不太确定，建议再查一下"

---

# 第一部分：C++ 编程基础

## 输入输出

蓝桥杯必须掌握的输入输出方式：

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);  // 加速cin/cout
    cin.tie(nullptr);
    
    int n;
    cin >> n;
    cout << n << '\n';  // 用'\n'不用endl，更快
    
    return 0;
}
```

**常见坑：**
- `endl` 会刷新缓冲区，大量输出时很慢 → 用 `'\n'`
- `scanf/printf` 比 `cin/cout` 快，但混用时会出问题
- 多组输入用 `while(cin >> n)` 或 `while(scanf("%d", &n) != EOF)`

## 常用 STL

| 容器 | 用途 | 常用操作 |
|------|------|----------|
| `vector<int>` | 动态数组 | `push_back`, `size`, `clear`, `[]` |
| `string` | 字符串 | `substr`, `find`, `length`, `+=` |
| `stack<int>` | 栈 | `push`, `pop`, `top`, `empty` |
| `queue<int>` | 队列 | `push`, `pop`, `front`, `empty` |
| `priority_queue<int>` | 优先队列/堆 | `push`, `pop`, `top` |
| `set<int>` | 有序集合 | `insert`, `find`, `count`, `erase` |
| `map<string,int>` | 键值映射 | `[]`, `find`, `count`, `erase` |
| `unordered_map<int,int>` | 哈希映射 | 同map，但更快，无序 |
| `pair<int,int>` | 二元组 | `first`, `second` |
| `sort()` | 排序 | `sort(a, a+n)` 或 `sort(v.begin(), v.end())` |

**常见坑：**
- `vector` 用 `[]` 访问前确保有足够大小，否则用 `push_back`
- `priority_queue` 默认大根堆，小根堆写 `priority_queue<int, vector<int>, greater<int>>`
- `map` 的 `[]` 会自动插入不存在的键，查找用 `count` 或 `find`

## 基础语法易错点

- **数组大小**：全局数组最大约 10^8（int），局部数组约 10^6。蓝桥杯数据范围常到 10^5，开数组加 +10 防越界
- **变量初始化**：全局变量默认0，局部变量是随机值 → 多组测试时必须手动清零
- **整数溢出**：int 范围约 2×10^9，超过用 `long long`。中间计算也可能溢出，注意 `1LL * a * b`
- **除法**：整数除法向下取整，`(a + b - 1) / b` 实现向上取整

---

# 第二部分：基础数据结构

## 2.1 数组与前缀和

**前缀和**：快速求区间和，O(1)查询。

```
生活类比：记账本，每页记累计总额。想知道第3天到第7天花了多少？用第7天的总额减第2天的。
```

```cpp
// 预处理 O(n)
for (int i = 1; i <= n; i++) {
    s[i] = s[i-1] + a[i];
}
// 查询 [l, r] 区间和 O(1)
int sum = s[r] - s[l-1];
```

**常见坑：**
- 下标从1开始，s[0]=0，避免边界处理
- 二维前缀和公式：`s[x2][y2] - s[x1-1][y2] - s[x2][y1-1] + s[x1-1][y1-1]`

## 2.2 差分数组

**差分**：快速区间修改，O(1)加值，O(n)还原。

```
生活类比：给一排灯同时调亮度。只记"从第3盏开始亮一点""到第7盏结束"，最后统一算。
```

```cpp
// 区间 [l, r] 每个元素加 val
diff[l] += val;
diff[r+1] -= val;
// 还原
for (int i = 1; i <= n; i++) {
    diff[i] += diff[i-1];
}
```

## 2.3 栈

**后进先出**，类比一摞盘子——最后放的先拿。

```
适用场景：括号匹配、表达式求值、单调栈、DFS模拟
```

```cpp
stack<int> stk;
stk.push(x);    // 入栈
stk.pop();      // 出栈
stk.top();      // 栈顶
stk.empty();    // 是否为空
```

**蓝桥杯常考：括号匹配**
```cpp
bool isValid(string s) {
    stack<char> stk;
    for (char c : s) {
        if (c == '(' || c == '[' || c == '{') stk.push(c);
        else {
            if (stk.empty()) return false;
            char t = stk.top(); stk.pop();
            if ((c==')' && t!='(') || (c==']' && t!='[') || (c=='}' && t!='{')) 
                return false;
        }
    }
    return stk.empty();
}
```

## 2.4 队列

**先进先出**，类比排队——先来先服务。

```
适用场景：BFS、滑动窗口、模拟排队
```

```cpp
queue<int> q;
q.push(x);    // 入队
q.pop();      // 出队
q.front();    // 队首
q.empty();    // 是否为空
```

**蓝桥杯常考：滑动窗口最大值（单调队列）**
```cpp
// 求 len 长度滑动窗口的最大值
deque<int> dq;  // 存下标
for (int i = 0; i < n; i++) {
    while (!dq.empty() && a[dq.back()] <= a[i]) dq.pop_back();
    dq.push_back(i);
    while (dq.front() <= i - len) dq.pop_front();
    if (i >= len - 1) cout << a[dq.front()] << ' ';
}
```

## 2.5 链表（数组模拟）

蓝桥杯中链表通常用数组模拟，比指针简单且不容易出错。

```cpp
const int N = 100010;
int e[N], ne[N], head = -1, idx = 0;

// 头插法
void insertHead(int x) {
    e[idx] = x;
    ne[idx] = head;
    head = idx++;
}

// 遍历
for (int i = head; i != -1; i = ne[i]) {
    cout << e[i] << ' ';
}
```

## 2.6 并查集

**判断连通性**，类比——朋友的朋友就是自己人。

```
适用场景：判断两点是否连通、合并集合、最小生成树(Kruskal)
```

```cpp
const int N = 100010;
int fa[N];

void init(int n) { for (int i = 1; i <= n; i++) fa[i] = i; }

int find(int x) {
    if (fa[x] != x) fa[x] = find(fa[x]);  // 路径压缩
    return fa[x];
}

void unite(int x, int y) { fa[find(x)] = find(y); }

bool connected(int x, int y) { return find(x) == find(y); }
```

**常见坑：**
- 每组测试前必须 `init()`，否则上次数据残留
- `find` 里必须写路径压缩，否则超时

## 2.7 树状数组

**单点修改 + 区间求和**，比线段树好写。

```
生活类比：一层层的管理——每个节点管一小段，问总和时往上汇总。
```

```cpp
const int N = 100010;
int c[N];

int lowbit(int x) { return x & (-x); }

void add(int i, int val) {
    for (; i < N; i += lowbit(i)) c[i] += val;
}

int sum(int i) {
    int s = 0;
    for (; i > 0; i -= lowbit(i)) s += c[i];
    return s;
}

// 区间 [l, r] 的和
int rangeSum(int l, int r) { return sum(r) - sum(l - 1); }
```

## 2.8 线段树

**区间修改 + 区间查询**，功能最全但代码量大。

```
适用场景：区间加/乘/最大值/最小值，带懒标记
```

```cpp
const int N = 100010;
int a[N], tree[4*N], lazy[4*N];

void pushup(int p) { tree[p] = tree[p*2] + tree[p*2+1]; }

void pushdown(int p, int l, int r) {
    if (lazy[p]) {
        int mid = (l + r) / 2;
        tree[p*2] += lazy[p] * (mid - l + 1);
        tree[p*2+1] += lazy[p] * (r - mid);
        lazy[p*2] += lazy[p];
        lazy[p*2+1] += lazy[p];
        lazy[p] = 0;
    }
}

void build(int p, int l, int r) {
    if (l == r) { tree[p] = a[l]; return; }
    int mid = (l + r) / 2;
    build(p*2, l, mid);
    build(p*2+1, mid+1, r);
    pushup(p);
}

void update(int p, int l, int r, int ql, int qr, int val) {
    if (ql <= l && r <= qr) {
        tree[p] += val * (r - l + 1);
        lazy[p] += val;
        return;
    }
    pushdown(p, l, r);
    int mid = (l + r) / 2;
    if (ql <= mid) update(p*2, l, mid, ql, qr, val);
    if (qr > mid) update(p*2+1, mid+1, r, ql, qr, val);
    pushup(p);
}

int query(int p, int l, int r, int ql, int qr) {
    if (ql <= l && r <= qr) return tree[p];
    pushdown(p, l, r);
    int mid = (l + r) / 2, res = 0;
    if (ql <= mid) res += query(p*2, l, mid, ql, qr);
    if (qr > mid) res += query(p*2+1, mid+1, r, ql, qr);
    return res;
}
```

**常见坑：**
- 数组开4倍大小
- 懒标记 `pushdown` 容易忘，忘就出错
- 建树时下标从1开始

## 2.9 哈希表

```cpp
// STL方式（最常用）
unordered_map<string, int> mp;
mp["key"] = 100;
if (mp.count("key")) cout << mp["key"];

// 手写哈希（开放寻址法，常数更优）
const int N = 200003;  // 大于数据范围的质数
int h[N], null = 0x3f3f3f3f;

void init() { memset(h, 0x3f, sizeof h); }

int find(int x) {
    int k = (x % N + N) % N;
    while (h[k] != null && h[k] != x) {
        k++;
        if (k == N) k = 0;
    }
    return k;
}
```

---

# 第三部分：基础算法

## 3.1 排序

蓝桥杯最基础的考点，必须会手写快排和归并排序。

**快速排序**
```cpp
void quickSort(int a[], int l, int r) {
    if (l >= r) return;
    int i = l - 1, j = r + 1, x = a[(l + r) / 2];
    while (i < j) {
        do i++; while (a[i] < x);
        do j--; while (a[j] > x);
        if (i < j) swap(a[i], a[j]);
    }
    quickSort(a, l, j);
    quickSort(a, j + 1, r);
}
```

**归并排序（常考：求逆序对）**
```cpp
int tmp[N];
long long mergeSort(int a[], int l, int r) {
    if (l >= r) return 0;
    int mid = (l + r) / 2;
    long long res = mergeSort(a, l, mid) + mergeSort(a, mid + 1, r);
    int i = l, j = mid + 1, k = 0;
    while (i <= mid && j <= r) {
        if (a[i] <= a[j]) tmp[k++] = a[i++];
        else { tmp[k++] = a[j++]; res += mid - i + 1; }  // 逆序对
    }
    while (i <= mid) tmp[k++] = a[i++];
    while (j <= r) tmp[k++] = a[j++];
    for (i = l, k = 0; i <= r; i++, k++) a[i] = tmp[k];
    return res;
}
```

**常见坑：**
- 快排选基准不要选端点，容易被卡到O(n²)
- 逆序对数量可能超int，用 `long long`

## 3.2 二分查找

```
生活类比：猜数字游戏——太大往左，太小往右，每次砍一半。
```

**整数二分模板（最重要）**
```cpp
// 找 >= x 的最小值（下界）
int bsearch(int a[], int n, int x) {
    int l = 0, r = n - 1;
    while (l < r) {
        int mid = (l + r) / 2;
        if (a[mid] >= x) r = mid;
        else l = mid + 1;
    }
    return a[l] >= x ? l : -1;
}

// 找 <= x 的最大值（上界）
int bsearch2(int a[], int n, int x) {
    int l = 0, r = n - 1;
    while (l < r) {
        int mid = (l + r + 1) / 2;  // 注意+1防死循环
        if (a[mid] <= x) l = mid;
        else r = mid - 1;
    }
    return a[l] <= x ? l : -1;
}
```

**常见坑：**
- `mid = (l + r + 1) / 2` 还是 `(l + r) / 2` 取决于 `l = mid` 还是 `r = mid`
- `l = mid` 时必须 `+1`，否则死循环
- 浮点二分不需要 `+1`，用 `while(r - l > 1e-6)`

**蓝桥杯常考：**
- 在有序序列中查找
- 二分答案（最大化最小值/最小化最大值）

## 3.3 递归与回溯

```
生活类比：走迷宫——每到一个路口选一条走，走不通就退回来换一条。
```

**全排列（最经典的回溯）**
```cpp
int n;
int path[10];
bool used[10];

void dfs(int u) {
    if (u == n) {
        for (int i = 0; i < n; i++) cout << path[i] << ' ';
        cout << '\n';
        return;
    }
    for (int i = 1; i <= n; i++) {
        if (!used[i]) {
            path[u] = i;
            used[i] = true;
            dfs(u + 1);
            used[i] = false;  // 回溯：撤销选择
        }
    }
}
```

**常见坑：**
- 回溯必须撤销选择（恢复状态）
- 剪枝：提前判断这条路不可能有解，直接跳过

## 3.4 贪心算法

```
核心思想：每一步选当前最好的，不回头。
适用场景：区间选点、活动安排、哈夫曼编码
```

**区间选点/活动安排（最常考）**
```cpp
// 按右端点排序，尽量选右端点
sort(segs.begin(), segs.end(), [](auto& a, auto& b) {
    return a.r < b.r;
});
int cnt = 0, end = -2e9;
for (auto& s : segs) {
    if (s.l > end) {
        cnt++;
        end = s.r;
    }
}
```

**常见坑：**
- 贪心需要证明正确性，蓝桥杯中常见贪心题基本是"按某端点排序"
- 不确定是不是贪心时，先试几个样例验证

## 3.5 BFS（广度优先搜索）

```
生活类比：水波纹扩散——从起点一圈一圈往外扩展，最先到的就是最短路径。
```

**适用场景：** 最短路（边权为1）、层序遍历、状态空间搜索

```cpp
const int N = 1010;
int dist[N][N];
int dx[] = {0, 0, 1, -1}, dy[] = {1, -1, 0, 0};

int bfs(int sx, int sy, int ex, int ey) {
    memset(dist, -1, sizeof dist);
    queue<pair<int,int>> q;
    q.push({sx, sy});
    dist[sx][sy] = 0;
    while (!q.empty()) {
        auto [x, y] = q.front(); q.pop();
        if (x == ex && y == ey) return dist[x][y];
        for (int i = 0; i < 4; i++) {
            int nx = x + dx[i], ny = y + dy[i];
            if (nx < 0 || nx >= n || ny < 0 || ny >= m) continue;
            if (dist[nx][ny] != -1) continue;
            if (g[nx][ny] == '#') continue;  // 障碍
            dist[nx][ny] = dist[x][y] + 1;
            q.push({nx, ny});
        }
    }
    return -1;  // 无法到达
}
```

**常见坑：**
- BFS必须用队列，不能用栈
- 入队时标记已访问，不要等出队再标记，否则会重复入队
- 四个方向数组 `dx/dy` 不要写错

## 3.6 DFS（深度优先搜索）

```
生活类比：走迷宫一条路走到底，走不通再回头。
```

**适用场景：** 连通性判断、排列组合、树的遍历、染色/判定

```cpp
bool vis[N];

void dfs(int u) {
    vis[u] = true;
    for (int v : g[u]) {
        if (!vis[v]) dfs(v);
    }
}
```

**常见坑：**
- 栈溢出：递归深度超过约 10^5 会爆栈，改用迭代或BFS
- 蓝桥杯数据范围大时慎用DFS

## 3.7 动态规划（DP）

```
生活类比：记笔记——把子问题的答案记下来，用到时直接查，不重复算。
核心三要素：状态定义、状态转移、边界条件
```

**根据数据范围猜DP类型：**

| 数据范围 | 可能的DP | 时间复杂度 |
|----------|----------|------------|
| n ≤ 20 | 状压DP | O(n·2^n) |
| n ≤ 100 | 区间DP | O(n³) |
| n ≤ 1000 | 线性DP | O(n²) |
| n ≤ 10^5 | 线性DP/贪心 | O(n) 或 O(nlogn) |

**01背包（最基础）**
```cpp
int dp[W];  // dp[j] = 容量为j时的最大价值
memset(dp, 0, sizeof dp);
for (int i = 0; i < n; i++) {
    for (int j = W; j >= w[i]; j--) {  // 逆序！
        dp[j] = max(dp[j], dp[j - w[i]] + v[i]);
    }
}
// 答案: dp[W]
```

**最长上升子序列（LIS）**
```cpp
// O(n²) 写法
int dp[N];  // dp[i] = 以a[i]结尾的LIS长度
int ans = 0;
for (int i = 0; i < n; i++) {
    dp[i] = 1;
    for (int j = 0; j < i; j++) {
        if (a[j] < a[i]) dp[i] = max(dp[i], dp[j] + 1);
    }
    ans = max(ans, dp[i]);
}

// O(nlogn) 写法（二分优化）
int d[N], len = 0;
for (int i = 0; i < n; i++) {
    int pos = lower_bound(d, d + len, a[i]) - d;
    d[pos] = a[i];
    if (pos == len) len++;
}
// 答案: len
```

**常见坑：**
- 01背包内层循环必须**逆序**，完全背包才是正序
- DP初始化别忘记，尤其是求最大值时初始化为负无穷
- 状态定义要想清楚"dp[i]代表什么"

## 3.8 图论基础

### 存图方式
```cpp
// 邻接表（最常用）
vector<int> g[N];            // 无权图
vector<pair<int,int>> g[N];  // 有权图：{邻居, 权重}

// 加边
g[u].push_back({v, w});
```

### 最短路

**Dijkstra（正权图最短路）**
```cpp
int dist[N];
bool vis[N];

void dijkstra(int start) {
    memset(dist, 0x3f, sizeof dist);
    memset(vis, false, sizeof vis);
    dist[start] = 0;
    priority_queue<pair<int,int>, vector<pair<int,int>>, greater<>> pq;
    pq.push({0, start});
    while (!pq.empty()) {
        auto [d, u] = pq.top(); pq.pop();
        if (vis[u]) continue;
        vis[u] = true;
        for (auto [v, w] : g[u]) {
            if (dist[v] > dist[u] + w) {
                dist[v] = dist[u] + w;
                pq.push({dist[v], v});
            }
        }
    }
}
```

**Floyd（全源最短路，n ≤ 500）**
```cpp
int d[N][N];  // 初始化：d[i][i]=0, 有边d[i][j]=权值, 无边=INF

void floyd(int n) {
    for (int k = 1; k <= n; k++)
        for (int i = 1; i <= n; i++)
            for (int j = 1; j <= n; j++)
                d[i][j] = min(d[i][j], d[i][k] + d[k][j]);
}
```

**常见坑：**
- Dijkstra不能处理负权边，有负权用SPFA（但可能被卡）
- Floyd三重循环顺序必须是 k → i → j，不能换
- `0x3f3f3f3f` 约为10^9，做INF时两个相加不会溢出int

### 最小生成树

**Kruskal（最常用，基于并查集）**
```cpp
struct Edge { int u, v, w; };
bool cmp(Edge& a, Edge& b) { return a.w < b.w; }

int kruskal(vector<Edge>& edges, int n) {
    sort(edges.begin(), edges.end(), cmp);
    init(n);  // 并查集初始化
    int sum = 0, cnt = 0;
    for (auto& e : edges) {
        if (!connected(e.u, e.v)) {
            unite(e.u, e.v);
            sum += e.w;
            if (++cnt == n - 1) break;
        }
    }
    return cnt == n - 1 ? sum : -1;  // -1表示不连通
}
```

### 拓扑排序
```cpp
bool topsort(int n) {
    queue<int> q;
    int in[N] = {0};  // 入度
    for (int i = 1; i <= n; i++)
        for (int v : g[i]) in[v]++;
    for (int i = 1; i <= n; i++)
        if (in[i] == 0) q.push(i);
    
    int cnt = 0;
    while (!q.empty()) {
        int u = q.front(); q.pop();
        cnt++;
        for (int v : g[u]) {
            if (--in[v] == 0) q.push(v);
        }
    }
    return cnt == n;  // true=无环, false=有环
}
```

---

# 第四部分：蓝桥杯实战策略

## 题型判断速查

| 关键词 | 方向 |
|--------|------|
| "最大/最小/方案数" | DP 或 贪心 |
| "最短路径/连通" | 图论 |
| "从n个中选k个/排列" | 搜索/组合数学 |
| "区间操作/区间查询" | 前缀和/差分/线段树 |
| "每次选最优" | 贪心 |
| "是否可以到达" | BFS |
| "所有方案" | DFS回溯 |

## 数据范围 → 算法复杂度对应表

| n 的大小 | 可接受复杂度 | 典型算法 |
|----------|-------------|----------|
| n ≤ 10 | O(n!) | 全排列/暴力搜索 |
| n ≤ 20 | O(2^n) | 状压DP |
| n ≤ 100 | O(n³) | Floyd、区间DP |
| n ≤ 1000 | O(n²) | 朴素DP、BFS |
| n ≤ 10^5 | O(nlogn) | 排序、二分、Dijkstra |
| n ≤ 10^6 | O(n) | 线性扫、前缀和 |
| n ≤ 10^7 | O(√n) | 数学、质数筛 |

## 蓝桥杯拿分技巧

1. **暴力先写**：即使知道正解，也先写个暴力拿部分分
2. **部分分思维**：不会正解就拿30%50%的分，题目常设特殊数据
3. **猜结论**：对于填空题，可以打表找规律
4. **对拍验证**：写个暴力程序和数据生成器对拍验证正解
5. **时间分配**：填空题先做（答案确定就是对了），大题写暴力保底

## 万能头文件 + 代码框架

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long LL;
const int N = 100010, INF = 0x3f3f3f3f;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    
    // 你的代码
    
    return 0;
}
```

---

# 限制

- 不帮用户在正式比赛中作弊
- 遇到超纲题（如高级数论、FFT、网络流）会坦诚说明并建议学习路径
- 不保证代码100%正确，建议用户自己测试验证
- 如果有多种解法，优先给出蓝桥杯中最容易理解和实现的版本
