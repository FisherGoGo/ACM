---
tags:
  - 转化
  - 贪心
  - 差分
  - 模运算
  - AtCoder
---

# ABC 454 F - Make it Palindrome 2

- **题目链接**：[AtCoder ABC 454 F](https://atcoder.jp/contests/abc454/tasks/abc454_f)
- **难度**：🔴 (ABC-F 级别)
- **关键技巧**：对称约化 → 差分转化 → 贪心选最小 $k$ 个

---

## 第一步：对称约化，消去跨越中点的操作

首先，我们可以**禁止任何跨越序列中点**的操作。因为任何一个跨越中点的操作，都可以通过抵消掉以中点为对称中心的前缀或后缀，转化为一个**不跨越中点**的操作。

> 这意味着所有操作可以独立地在左半部分和右半部分分别考虑。

---

## 第二步：构造差值序列 $B$

定义 $N' = \left\lfloor \frac{N}{2} \right\rfloor$，构造 $B = (B_1, B_2, \ldots, B_{N'})$：

$$B_i = (A_i - A_{N+1-i}) \bmod M$$

$B_i$ 表示对称位置之间的「差距」。若 $B_i = 0$，则这对位置已经满足回文条件。

问题转化为：

> 对 $B$ 施加以下操作，最少多少次可使 $B$ 全部变为 $0$？
>
> - 选择 $1 \le l \le r \le N'$ 和 $c \in \{-1, 1\}$，将 $B_l, \ldots, B_r$ 全部替换为 $(B_i + c) \bmod M$。

其中 $c=1$ 对应在 $A$ 的左半部分操作，$c=-1$ 对应在 $A$ 的右半部分操作。

---

## 第三步：差分，转化为点操作

定义 $N'' = N' + 1$，构造差分序列 $C = (C_1, C_2, \ldots, C_{N''})$：

$$C_i = (B_i - B_{i-1}) \bmod M$$

其中约定 $B_0 = B_{N'+1} = 0$。

此时，对 $B[l, r]$ 做区间 $+c$（模 $M$）等价于：

- $C_l \gets (C_l + c) \bmod M$
- $C_{r+1} \gets (C_{r+1} - c) \bmod M$

问题进一步转化为：

> 对 $C$ 执行以下操作，最少多少次可使 $C$ 全部变为 $0$？
>
> - 选择 $i \neq j$，令 $C_i \gets (C_i + 1) \bmod M$，$C_j \gets (C_j - 1) \bmod M$。

> **注**：忽略 $i \neq j$ 的条件不影响答案（若 $i = j$，两次操作互相抵消，本质无意义），以下均忽略此条件。

---

## 第四步：独立决策——谁加谁减

将「$C_i \gets (C_i+1) \bmod M$」称为一次**增量**，「$C_i \gets (C_i-1) \bmod M$」称为一次**减量**。

由于每次操作同时产生一次增量和一次减量，**总增量次数 = 总减量次数**。

我们选择子集 $\varnothing \neq X \subsetneq \{1, 2, \ldots, N''\}$，规定：
- $i \in X$：只对 $C_i$ 施加增量
- $i \notin X$：只对 $C_i$ 施加减量

> 对 $C_i = 0$ 的元素，是否放入 $X$ 不影响答案，故决定不放。

由 $C$ 的定义可知，总和 $S = \sum C_i$ 总是 $M$ 的倍数。因此**无论选择哪个 $X$，总存在合法的操作序列。**

---

## 第五步：计算操作次数

- 需要的增量总次数：$\displaystyle \sum_{i \in X} C_i$
- 需要的减量总次数：$\displaystyle \sum_{i \notin X} (M - C_i)$

操作次数为：

$$
\max\left( \sum_{i \notin X} (M - C_i),\ \sum_{i \in X} C_i \right)
$$

令 $S_X = \sum_{i \in X} C_i$，则该式可重写为：

$$
S_X + M \cdot \max\left( N'' - |X| - \frac{S}{M},\ 0 \right)
$$

---

## 第六步：贪心——选择最小的 $k$ 个

当 $|X| = k$ 固定时，为使操作次数最小，显然应选 $C$ 中最小的 $k$ 个元素构成 $X$。

分析 $k$ 变化时的行为：

- $S_X$ 随 $k$ 增加，每增加一个元素至少增 $0$，至多增 $M-1$
- $\displaystyle M \cdot \max\left(N'' - k - \frac{S}{M},\ 0\right)$
  - 当 $k < N'' - \frac{S}{M}$ 时，每增 $k$ 减 $M$
  - 当 $k \ge N'' - \frac{S}{M}$ 时，恒为 $0$

因此 $\max$ 的两个参数在 $k = N'' - \dfrac{S}{M}$ 处相等，此时操作次数最小。

**结论**：最少操作次数 = $C$ 中**最小的 $\displaystyle \left( N'' - \frac{S}{M} \right)$ 个元素之和**。

---

## 复杂度分析

- 构造 $B, C$：$O(N)$
- 排序取最小的 $k$ 个：$O(N \log N)$（瓶颈）
- **总复杂度**：$O(N \log N)$

---

## 核心转化链总结

$$
\begin{aligned}
A &\xrightarrow{\text{对称约化}} B \xrightarrow{\text{差分}} C \\[4pt]
&\xrightarrow{\text{贪心}} \min_k \left\{\sum \text{最小的 } k \text{ 个 } C_i \right\},\quad k = N'' - \frac{\sum C_i}{M}
\end{aligned}
$$

三层转化，每一步都让问题的结构更加清晰，最终化为一个简单的「排序取前 $k$ 小」问题。
