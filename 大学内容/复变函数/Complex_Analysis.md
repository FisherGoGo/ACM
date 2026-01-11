---
title: Complex Analysis
date: 2025-04-18
tags:
  - Analysis
  - Note
---
##### Derivate, Integral and Series

设 $f(x+iy)=u(x,y)+iv(x,y)$

*Defination:* Cauchy-Riemann 方程
$$
\frac{\partial u}{\partial x}=\frac{\partial v}{\partial y}, \frac{\partial v}{\partial x}=-i \frac{\partial u}{\partial y}
$$
等价写法
$$
\frac{\partial f}{\partial \bar{z}}=0
$$


*Theorem:* $f:\Omega\to C$ 可微的充要条件是，$u,v$ 可微且满足 Cauchy-Riemann 方程。

*Loomann-Menchoff Theorem:*  $f\in C(\Omega)$ ，$u_{x},v_{x},u_{y},v_{y}$ 存在，则 $f$ 可微的充要条件是，$u,v$ 满足 Cauchy-Riemann 方程。

注：$\Omega$ 上的点都要满足条件。不讨论点可微。反例：
$$
f(x+iy)=\sqrt{ |xy| }
$$

*Theorem:* 设 $f\in H(\Omega)$，$z_{0}\in \Omega$，若 $f'(z_{0})\ne 0$，则 $f$ 在 $z_{0}$ 局部可逆。

*Theorem:* 设 $\{ f_{n} \}$ ，且一致收敛 $f$，则
$$
\begin{aligned}
f_{n}\in C(\Omega)\implies f\in C(\Omega) \\
f_{n}\in H(\Omega)\implies f\in H(\Omega)
\end{aligned}
$$
若 $f_{n}\in C(\Omega)$，$\gamma$ 可求长，则
$$
\int_{\gamma}f(x)\mathrm{d}x=\lim_{ n \to \infty } \int_{\gamma} f_{n}(x)\mathrm{d}x
$$


##### Cauchy-Gousat Theorem

*Theorem I:* 设 $\Omega$ 区域，$\Delta$ 内部闭三角形，$\gamma=\partial\Delta$，$f\in H(\Omega)$
*Theorem II:* 设 $\Omega$ 凸区域，$\gamma$ 内部闭曲线，$f\in H(\Omega)$
*Theorem III:* 设 $\Omega$ 区域，$\Delta$ 内部闭多边形，$\gamma=\partial\Delta$，$f\in H(\Omega)$
*Theorem IV:* 设 $\Omega$ 单连通区域，$\gamma$ 分段线性闭曲线，$f\in H(\Omega)$
*Theorem V:* 设 $\Omega$ 单连通区域，$\gamma$ 可求长闭曲线，$f\in H(\Omega)$
*Theorem VI:* 设 $\Omega$ 区域，$\gamma$ 零伦可求长闭曲线，$f\in H(\Omega)$
*Theorem VII:* 设 $\Omega$ 有界区域，$\gamma=\partial \Omega$ 由有限多条简单曲线组成， $f\in C(\overline{\Omega})$ 且 $f\in H(\Omega)$
则有
$$
\int_{\gamma}f(z)\mathrm{d}z=0
$$


*Theorem I:* $\gamma$ 零伦可求长闭曲线，$f\in H(\Omega)$，$z\in \Omega\setminus \gamma$
$$
f(z)= n(\gamma,z)\int_{\gamma} \frac{f(\zeta)}{\zeta-z}\mathrm{d}\zeta
$$
*Theorem II:* 设 $\Omega$ 有界区域，$\gamma=\partial \Omega$ 由有限多条简单曲线组成， $f\in C(\overline{\Omega})$ 且 $f\in H(\Omega)$ 
则有
$$
f(z)= n(\gamma,z)\int_{\gamma} \frac{f(\zeta)}{\zeta-z}\mathrm{d}\zeta
$$


##### Applications

*Theorem:* $f\in H(\Omega)$，$z_{0}\in \Omega$，则 $f$ 在 $z_{0}$ 有任意阶导数，且
$$
f^{(n)}(z_{0})= \frac{n!}{2\pi i}\int_{\gamma} \frac{f(z)}{(z-z_{0})^{n}}\mathrm{d}z
$$

*Collary:* $f\in H(\Omega)$，$z\in \Omega$，设 $\rho>0$ 满足 $\overline{D}(z,\rho)\subset \Omega$，则在 $z\in\overline{D}(z_{0},\rho)$ 上有
$$
f(z)=\sum_{n=0}^{\infty} \frac{f^{(n)}(z)}{n!}(z-z_{0})^{n}
$$

*Cauchy Estinimation:* 设 $M=\max \{|f(z)|:z\in \overline{D}(z_{0},\rho)\}$，有
$$
|f^{(n)}(z_{0})|\le \frac{n!M}{\rho^{n}}
$$

*Liouville Theorem:* 若 $f\in H(\mathbb{C})$ 有界，则 $f$ 是常函数。

*Fundamental Algebraic Theorem:* 设 $p\in \mathbb{C}[z],\deg p>0$，则存在 $z_{0}\in \mathbb{C}$，使得 $p(z_{0})=0$。

*Morera Theorem:* $f\in C(\Omega)$，若 $f$ 在 $\Omega$ 内任意可求长闭曲线积分为 $0$，则 $f\in H(\Omega)$。

*Riemann Theorem:* $z_{0}\in \Omega$，若 $f$ 在 $\Omega\setminus \{ z_{0} \}$ 上全纯，则存在 $F\in H(\Omega)$，使得 $F\Big\vert_{\Omega\setminus \{ z_{0} \}}=f$。

*Inverse Function:* 

*Lagrange Reverse Formula:* 


##### Zero Points

$f\in H(\Omega)$，$z_{0}\in \Omega$ 满足 $f(z_{0})=0$。考虑 $f$ 在 $z_{0}$ 的 Taylor 级数
$$
f(z)=a_{1}(z-z_{0})+a_{2}(z-z_{0})^{2}+\dots
$$
有两种可能
*Case 1:* 对于 $\forall n$，$a_{n}=0$ 
*Case II:* 存在 $a_{m}\ne 0$，即
$$
f(z)=(z-z_{0})^{m}(a_{m}+a_{m+1}z+\dots)
$$

*Theorem:* 对于 *Case II*，零点一定是孤立的。

*Theorem:* 对于 *Case I*，$f$ 在 $\Omega$ 恒为 $0$。

*Collary:* 若 $f(z)=0$ 的集合有聚点，则 $f\equiv 0$

*Collary:* 若 $f(z)$ 在 $\Omega$ 开子集上为常数，则 $f$ 是常函数

*Theorem:* $f\in H(\Omega)$，$\gamma$ 零伦可求长闭曲线，内部区域 $D$，则 $f$ 在 $D$ 中的零点数量 $N$ （记重数）为
$$
N=\frac{1}{2\pi i}\int_{\gamma} \frac{f'(z)}{f(z)}\mathrm{d}z
$$

*Hurwitz Theorem:* $\{ f_{n} \}\subset H(\Omega)$，$f_{n}$ 紧一致收敛到 $f$，若 $f$ 在 $\Omega$ 处处非零，则 $f$ 在 $\Omega$ 上恒为 $0$ 或不为 $0$。

*Rouche Theorem:* $f,g\in H(\Omega)$，$\gamma$ 是零伦可求长闭曲线，若在 $\gamma$ 上满足
$$
|f(z)-g(z)|<|f(z)|
$$
则 $f$ 与 $g$ 零点个数相同。

*Collary:* $f,g\in H(\Omega)$，$w_{0}=f(z_{0})$，设 $z_{0}$ 是 $f(z)-w_{0}$ 的 $m$ 重零点，则对于充分小的 $\rho$，存在 $\delta>0$，使得 $D(w_{0},\delta)$ 的每一点 $A$，$f(z)-A$ 在 $D(z_{0},\rho)$ 内恰有 $m$ 个零点。

*Collary:* 若 $f\in H(\Omega)$ 不是常数，则 $f$ 是开映射。

##### Maximum Module Principle

*Maximum Module Principle:* $f\in H(\Omega)$，若 $z_{0}\in \Omega$ 是 $|f(z)|$ 的局部极大值，则 $f$ 是常函数。

*Schwartz Lemma:* 设 $D=D(0,1)$，$f\in H(D):D\to D$，$f(0)=0$，则 $|f(z)|\le |z|$，$|f'(0)|\le 1$，当且仅当 $f(z)=e^{i\tau}z$ 取等。


##### Laurent Series
*Weierstrass Theorem:* 设 $\{ f_{n} \}\subset H(\Omega)$内闭一致收敛到 $f$，则 $f\in H(\Omega)$，且 $\{ f^{(k)}_{n} \}$一致收敛到 $f^{(k)}$。

*Laurent Series:* 
$$
\sum_{n=-\infty}^{\infty}c_{n}(z-z_{0})^{n}
$$
称为 $a$ 处的 Laurent 级数，按正负分为 $f_{+}$ 全纯部分，$f_{-}$ 主要部分/奇异部分。称 $r<|z-z_{0}|<R$ 收敛圆环。

*Theorem:* 设 $0\le r<R\le \infty$，$\Omega=D(z_{0},r,R)$，$f\in H(\Omega)$，则对于 $z\in \Omega$ 有
$$
f(z)=\sum_{n=-\infty}^{\infty} c_{n}(z-z_{0})^{n}
$$
设 $\gamma$ 是 $\Omega$ 中绕 $z_{0}$ 环绕数为 $1$ 的曲线，有
$$
c_{n}=\frac{1}{2\pi i}\int_{\gamma} \frac{f(\zeta)}{(\zeta-z)^{n+1}}\mathrm{d}\zeta
$$
且表达方式唯一。

*Defination:* 若 $f\in H(\check D(z_{0},\rho))$，则称 $z_{0}$ 是孤立奇点。孤立奇点有三种情况
- $\lim f(z)$ 存在，则 $f_{-}(z)=0$，称为可去奇点
- $\lim f(z)$ 是无限，则 $f_{-}(z)$ 有限 $m$ 项，称为 $m$ 阶极点
- $\lim f(z)$ 不存在，则 $f_{-}(z)$ 无限项，称为本性奇点

*Weierstrass Theorem:* $f$ 在本性奇点 $z_{0}$ 附近的取值是稠密的。

定义 $\hat{C}=C\cup \{ \infty \}$。同理定义 $\infty$ 处奇点。


*Defination:* 若 $f\in H(\mathbb{C})$，则称 $f$ 是整函数。若 $f$ 在 $\mathbb{C}$ 上的奇点均为极点，则称 $f$ 是亚纯函数。

*Lemma:* $H(\Omega)$ 是整环。

*Theorem:* 设 $f$ 亚纯函数，$f$ 是有理函数的充要条件是，$\infty$ 是可去奇点或极点。

*Weierstrass Theorem:* 

*Mittag-Leffler Theorem:* 

##### Residue Theorem

*Defination:* 若 $z_{0}$ 是孤立奇点，$\gamma=\partial D(z_{0},\rho)$，定义其留数
$$
\operatorname{Res}(f,z_{0})=\frac{1}{2\pi i}\int_{\gamma} f(z)\mathrm{d}z
$$

*Residue Theorem:* 设 $f$ 在 $\mathbb{C}$ 除 $z_{1},\dots,z_{n}$ 以外全纯，则
$$
\sum_{k=1}^{n}\operatorname{Res}(f,z_{k})+\operatorname{Res}(f,\infty)=0
$$

设 $f\in H(\Omega)\cap C(\overline\Omega)$，$\gamma=\partial \Omega$ 可求长闭曲线，则
$$
\int_{\gamma} f(z)\mathrm{d}z=2\pi i \sum_{k=1}^{n}\operatorname{Res}(f,z_{k})
$$

