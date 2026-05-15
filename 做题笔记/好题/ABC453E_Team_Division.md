---
tags:
  - 组合数学
  - 差分
  - 枚举
  - AtCoder
---

# ABC 453 E - Team Division

- **题目链接**：[AtCoder ABC 453 E](https://atcoder.jp/contests/abc453/tasks/abc453_e)
- **关键技巧**：枚举队伍大小 → 三类归类 → 差分 + 组合数

---

## 题意

将 $N$ 个玩家分入 A、B 两个**可区分**的队伍，每人必须恰好属于一队，每队至少一人。

玩家 $i$ 有约束 $[L_i, R_i]$：其所在队伍的人数必须在 $[L_i, R_i]$ 内。

求合法分配方案数，模 $998244353$。

---

## 思路

### 枚举 A 队大小

枚举 A 队人数 $k \in [1, n-1]$，B 队即为 $n-k$。

对于玩家 $i$，在当前 $k$ 下：
- **能去 A**：$k \in [L_i, R_i]$
- **能去 B**：$n-k \in [L_i, R_i]$

据此将玩家分为三类：

| 类别 | 条件 | 含义 |
|------|------|------|
| 只能去 A | 仅满足 A 约束 | 强制分入 A |
| 只能去 B | 仅满足 B 约束 | 强制分入 B |
| 都能去 | A、B 约束都满足 | 可自由选择 |

---

### 差分统计三类人数

#### `pre[x]`：能加入大小为 $x$ 的队伍的玩家数

对每个玩家 $[l, r]$：

```cpp
pre[l]++;  pre[r+1]--;
```

前缀和后 `pre[x]` 即为能加入大小为 $x$ 的队伍的玩家总数。

#### `b[x]`（$x \le n/2$）：能同时加入大小为 $x$ 和 $n-x$ 的队伍的玩家数

当 $x \le n/2$ 时，$\max(L_i, n-R_i) \le n/2$，$\min(R_i, n-L_i) \ge n/2$，只需检查下界：

```cpp
int st = max(l, n - r);
int ed = n / 2;
if (st <= ed) b[st]++, b[ed+1]--;
```

前缀和后，$b[\min(k, n-k)]$ 即为「都能去」的玩家数。

#### 对每个 $k$：

```
C = b[min(k, n-k)]         // 都能去
A = pre[k] - C             // 只能去 A
B = pre[n-k] - C           // 只能去 B
```

---

### 合法性检查

1. $A + B + C = n$：所有玩家都能被分配
2. $A + C \ge k$：A 队有足够候选人
3. $B + C \ge n - k$：B 队有足够候选人
4. $A \le k$：强制去 A 的不超过 A 队容量
5. $B \le n - k$

---

### 组合计数

满足条件时，从 $C$ 个「都能去」中选 $(k - A)$ 个去 A，其余去 B：

$$\text{ans} \mathrel{+}= \binom{C}{k-A} \pmod{998244353}$$

---

## 复杂度

- 差分填充：$O(n)$
- 枚举 $k$ 计算：$O(n)$
- 组合数预处理：$O(n)$
- **总复杂度**：$O(n)$

---

## 代码

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N = 2e5 + 10;
const ll M = 998244353;

ll qp(ll a, ll x) {
    ll res = 1;
    for (; x; x >>= 1, a = a * a % M)
        if (x & 1) res = a * res % M;
    return res;
}

ll fac[500005], inv[500005];

void init() {
    fac[0] = inv[0] = 1;
    for (int i = 1; i <= 300005; i++) {
        fac[i] = fac[i - 1] * i % M;
        inv[i] = qp(fac[i], M - 2);
    }
}

ll C(ll n, ll k) {
    if (n < k) return 0;
    if (n == k) return 1;
    return fac[n] * inv[k] % M * inv[n - k] % M;
}

int n, pre[N], b[N];

void solve() {
    cin >> n;
    for (int i = 1; i <= n; i++) pre[i] = b[i] = 0;
    for (int i = 1; i <= n; i++) {
        int l, r;
        cin >> l >> r;
        pre[l]++; pre[r + 1]--;

        int st = max(l, n - r);
        int ed = n / 2;
        if (st <= ed) b[st]++, b[ed + 1]--;
    }
    for (int i = 1; i <= n; i++)
        pre[i] += pre[i - 1], b[i] += b[i - 1];

    ll ans = 0;
    for (int k = 1; k <= n - 1; k++) {
        int C_ = b[min(k, n - k)];
        int A = pre[k] - C_;
        int B = pre[n - k] - C_;

        if (A + B + C_ < n || A + C_ < k || B + C_ < n - k || A > k || B > n - k)
            continue;

        ans = (ans + C(C_, k - A)) % M;
    }
    cout << ans << '\n';
}

int main() {
    ios::sync_with_stdio(0); cin.tie(0);
    init();
    solve();
}
```
