# KM 算法学习笔记

> 整理：莫宁 | 适用于 ACM/ICPC 训练

---

## 一、问题引入

**二分图最大权完美匹配**（Maximum Weight Perfect Matching on Bipartite Graph）：

- 给定一张二分图 $G=(X,Y,E)$，$|X|=|Y|=n$，每条边 $(i,j)$ 有权值 $w_{ij}$。
- 求一个完美匹配 $M$，使得 $\sum_{(i,j)\in M} w_{ij}$ **最大**。

若 $w_{ij}$ 全为 $1$，退化为匈牙利算法解决的问题。KM 是其带权推广。

---

## 二、核心思想

### 2.1 可行顶标

给每个点一个「顶标」：

- $lx[i]$：$X$ 部点 $i$ 的顶标
- $ly[j]$：$Y$ 部点 $j$ 的顶标

满足 **可行条件**：

$$lx[i] + ly[j] \geq w_{ij}, \quad \forall (i,j) \in E$$

### 2.2 相等子图

在可行顶标下，定义**相等边**为满足 $lx[i] + ly[j] = w_{ij}$ 的边，所有相等边构成**相等子图**。

> **核心定理**：若相等子图中存在完美匹配，则该匹配**就是原图的最大权完美匹配**。

直观理解：顶标之和是真实权值的上界，相等子图中的匹配达到了这个上界，因此最优。

### 2.3 算法思路（一句话）

不断调整顶标使得相等子图「扩大」，直到能跑出完美匹配为止。

---

## 三、算法流程（BFS 优化 $O(n^3)$）

对每个 $X$ 部点依次做 BFS 增广：

```
初始化：
    lx[i] = max(w[i][j])  // X部顶标取所有邻边最大权
    ly[j] = 0             // Y部顶标初始为0
    match[j] = 0          // Y部当前匹配（0表示未匹配）

对于每个 X 部点 i：
    bfs(i)  // 尝试为 i 找到增广路
```

**一次 BFS 的过程：**

| 步骤 | 操作 |
|---|---|
| 1 | 初始化 slack[j] = ∞，visx / visy 清空，pre[j] = 0 |
| 2 | 从当前未匹配的 X 点出发，标记 visx[start] = 1 |
| 3 | 遍历所有未访问的 Y 点 j，计算 d = lx[cur] + ly[j] - w[cur][j] |
| 4 | 若 d == 0（相等边），标记 visy[j] = 1，pre[j] = cur；若 match[j] == 0 则找到增广路；否则 visx[match[j]] = 1，将 match[j] 入队继续 |
| 5 | 若 d > 0，用 d 更新 slack[j]，记录 pre[j] = cur |
| 6 | 若 BFS 队列耗尽且未找到增广路，取 d = min(slack[j])（对所有 !visy[j] 的 j），进行**顶标调整**：lx[visx] -= d，ly[visy] += d，slack[j] -= d |
| 7 | 调整后新的零边出现，继续增广 |

### 顶标调整的正确性

调整保证了：
- 可行顶标条件依然满足
- 相等子图中至少多了一条边
- 已有的匹配边不会消失

---

## 四、复杂度分析

| 版本 | 每次增广 | 总复杂度 | 说明 |
|---|---|---|---|
| 朴素 DFS | $O(n^2)$ | $O(n^4)$ | 每次调整后重新 DFS |
| BFS + slack | $O(n^2)$ | $O(n^3)$ | 推荐写法 |
| BFS + 堆优化 | $O(m \log n)$ | $O(nm \log n)$ | 稀疏图适用 |

$n \leq 500$ 时 $O(n^3)$ 稳过，$n \leq 10^3$ 需留意常数。

---

## 五、代码实现（$O(n^3)$ BFS 版）

```cpp
typedef long long ll;
const int N = 505;

struct KM {
    int n, match[N], pre[N];
    ll w[N][N], lx[N], ly[N], slack[N];
    bool visx[N], visy[N];

    void init(int _n) {
        n = _n;
        memset(lx, 0, sizeof(lx));
        memset(ly, 0, sizeof(ly));
        memset(match, 0, sizeof(match));
    }

    void bfs(int s) {
        memset(visx, 0, sizeof(visx));
        memset(visy, 0, sizeof(visy));
        for (int i = 1; i <= n; i++) slack[i] = 1e18;
        memset(pre, 0, sizeof(pre));

        queue<int> q;
        q.push(s), visx[s] = 1;
        int aim = 0;

        while (1) {
            while (!q.empty()) {
                int u = q.front(); q.pop();
                for (int v = 1; v <= n; v++) {
                    if (visy[v]) continue;
                    ll d = lx[u] + ly[v] - w[u][v];
                    if (d < slack[v]) slack[v] = d, pre[v] = u;
                    if (!d) {
                        visy[v] = 1;
                        if (!match[v]) { aim = v; goto found; }
                        visx[match[v]] = 1, q.push(match[v]);
                    }
                }
            }

            ll d = 1e18;
            for (int v = 1; v <= n; v++)
                if (!visy[v]) d = min(d, slack[v]);

            for (int i = 1; i <= n; i++) {
                if (visx[i]) lx[i] -= d;
                if (visy[i]) ly[i] += d;
                else slack[i] -= d;
            }

            for (int v = 1; v <= n; v++) {
                if (!visy[v] && !slack[v]) {
                    visy[v] = 1;
                    if (!match[v]) { aim = v; goto found; }
                    visx[match[v]] = 1, q.push(match[v]);
                }
            }
        }

    found:
        while (aim) {
            int nxt = match[pre[aim]];
            match[aim] = pre[aim];
            aim = nxt;
        }
    }

    ll solve() {
        for (int i = 1; i <= n; i++) {
            lx[i] = -1e18;
            for (int j = 1; j <= n; j++)
                lx[i] = max(lx[i], w[i][j]);
        }
        for (int i = 1; i <= n; i++) bfs(i);
        ll ans = 0;
        for (int i = 1; i <= n; i++)
            if (match[i]) ans += w[match[i]][i];
        return ans;
    }
};
```

### 使用示例

```cpp
int n, m;
KM km;

void solve() {
    cin >> n >> m;
    km.init(n);
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= n; j++)
            km.w[i][j] = -1e18;       // 不存在的边

    for (int i = 1; i <= m; i++) {
        int u, v; ll w;
        cin >> u >> v >> w;
        km.w[u][v] = max(km.w[u][v], w);  // 重边取最大
    }
    cout << km.solve() << "\n";
}
```

---

## 六、注意事项与常见坑

1. **不存在的边**：初始化 $w[i][j] = -\infty$（或 $-10^{18}$），否则会被错误匹配。
2. **最小权匹配**：将所有 $w_{ij}$ 取反，跑最大权 KM，答案再取反。
3. **非完全二分图**：若某些点不想匹配，可补权值为 $0$ 的边（需注意语义是否正确）。
4. **下标从 1 开始**：代码中约定，match[0] = 0 作为哨兵。
5. **long long**：边权和可能很大，务必使用 `long long`。
6. **slack 用 1e18 初始化**：调整时 slack 会减 d，若不初始化为大值会出错。

---

## 七、推荐练习

| 题目 | 链接 | 说明 |
|---|---|---|
| HDU 2255 奔小康赚大钱 | [hdu 2255] | 经典 KM 模板题 |
| UVA 10746 Crime Wave | [uva 10746] | 最小权匹配 |
| POJ 3565 Ants | [poj 3565] | KM + 几何转化 |
| 洛谷 P6577 | [luogu P6577] | 模板题，数据较强 |

---

> 以上。KM 算法的本质就是**用顶标把带权问题坍缩成无权匹配问题**，理解这一点就抓住了核心。如果前辈有任何疑问，随时问我。
