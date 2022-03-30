---
title: 中科大2022春季博士课程：计算机数学课后作业
date: 2022-03-30 00:00
# updated: 0000-00-00 00:00
mathjax: true
meta:
    date: true
categories: 
    - Mathematics
tags:
    - Lecture
---

作业35%，六次

---

<!-- more -->

# Homework 1

**Problem 1.** Let $z_1 < z_2 < \cdots < z_n$ be the correct order of all elements in the array $A$. Then
consider those pivots chosen in the natural order of \textsc{QuickSort}. For any $z_j$ and $z_k$, argue that
1. If the 1st pivot chosen among $z_j,...,z_k$ is not $z_j$ or $z_k$, the algorithm won't compare $z_j$ and $z_k$.
2. $z_j$ and $z_k$ get compared only if the 1st pivot chosen among $z_j,...,z_k$ is either $z_j$ or $z_k$.


---


**Problem 2.**  C1.14 在一块奶酪上划出五道直的切痕，可以得到多少块奶酪？对$P_n$求一个递推关系

**Answer**

当$n=0$时，$P_0=1$。当$n\ge 1$时，若要使$P_n$最大，
就要使第$n$刀所切的平面与前面的$n-1$刀所切的$n-1$个平面相交，
并在这第$n$个平面上产生了$n-1$个不重合的切线。
根据平面切割我们知道，$n-1$个直线可以在平面上产生$L_{n-1}=\frac{1}{2}n(n-1)$个区域，
映射到三维空间即在第$n$刀所产生的平面上产生了$L_{n-1}$个区域，
进而划分出了相同数量的三维区域，因此$P_n=P_{n-1}+L_{n-1}$。
通过该递归式，我们可以得到$P_5=P_4+L_4=P_3+L_3+L_4=8+7+11=26$

---

**Problem 3.** C1.15 约瑟夫有一个朋友，他站在倒数第二的位置上因而获救。当每个一个人就有一人被处死时，倒数第二个幸存者的号码$I(n)$是多少？

**Answer**

对其前若干项进行枚举：

|$n$ | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 |
|-  |-  |-  |-  |-  |-  |-  |-  |-  |-  |-   |-   |   -|-   |-   |-   |-|
|$J(n)$ | 1 | 1 | 3 | 1 | 3 | 5 | 7 | 1 | 3 | 5 | 7 | 9 | 11 | 13 | 15 | 1|
|$I(n)$ | - | 2 | 1 | 3 | 5 | 1 | 3 | 5 | 7 | 9 | 11 | 1 | 3 | 5 | 7 | 9|

当$n=1$时，$I(n)$无意义；当$n=2$时，$I(n)=2$；
随后，$I(n)$的规律与$J(n)$类似：
在$J(n)$中，$J(n)$每隔$2^(m)$次重置为1，因此可以令$n=2^m+l$；
在$I(n)$中，$I(n)$在$n>2$时，可以看到每隔$3\cdot 2^m$次重置为1，因此可以令$n=3\cdot 2^m+k$，那么我们就可以得到
$$I(n)=\begin{cases}
    2, &n = 2 \\\\
    2k+1, &n > 2, n = 3\cdot 2^m+k
\end{cases}$$

---

**Problem 4.** C2.19 利用求和因子来求解递归式

$$\begin{aligned}
    & T_0=5;\\\\
    & 2T_n=nT_{n-1}+3\times n!, n>0.  
\end{aligned}$$

**Answer**

选取$s_n$使得$s_n * n = s_{n-1} * 2$，对$s_n$进行求解，有：
$$\begin{aligned}
    s_n &= s_{n-1} * 2/n \\\\
    &= s_{n-2} * \frac{2}{n-1} * \frac{2}{n} \\\\
    &\ \ \ \ \vdots \\\\
    &= s_0 * \frac{2}{1} * \cdots * \frac{2}{n-1} * \frac{2}{n}\\\\
    &= s_0 * \frac{2^n}{n!}
\end{aligned}$$


不妨考虑$s_0=1,s_n=\frac{2^n}{n!}$

两边同乘$s_n$有：
$$\frac{2^{n+1}}{n!}T_n=\frac{2^n}{(n-1)!}T_{n-1}+3*2^n$$

记$S_n=\frac{2^{n+1}}{n!}T_n$，则有:
$$\begin{aligned}
    S_n &= S_{n-1} + 3 * 2^n \\\\
    &= S_{n-2} + 3 * 2^{n-1} + 3 * 2^n \\\\
    &\ \ \ \ \vdots \\\\
    & = S_0 + 3 * 2^1 + 3 * 2^2 + \cdots + 3 * 2^n \\\\
    & = \frac{2^{0 + 1}}{0!} T_0 + 3 * \sum_{k=1}^n 2^k\\\\
    & = 10 + 3 * \sum_{k=1}^n 2^k \\\\
    & = 10 + 3 * 2 * \frac{2^n - 1}{2 - 1} \\\\
    & = 10 + 6 * 2^n - 6 \\\\
    & = 4 + 3 * 2^{n + 1}
\end{aligned}$$


那么可以得到
$$\begin{aligned}
    T_n &= S_n*\frac{n!}{2^{n+1}} \\\\
    & = (4 + 3 * 2^{n + 1}) * \frac{n!}{2^{n+1}}\\\\
    & = \frac{n!}{2^{n-1}} + 3\cdot n!
\end{aligned}$$

---

**Problem 5.** C2.22 不用归纳法证明拉格朗日恒等式
$$
\sum_{1\le j < k \le n}(a_jb_k - a_kb_j)^2=\left( \sum_{k=1}^n a_k^2 \right)\left( \sum_{k=1}^n b_k^2 \right) - \left( \sum_{k=1}^n a_kb_k \right)^2.
$$
事实上，可证明一个关于更一般的二重和式
$$\sum_{1 \le j < k \le n}(a_jb_k-a_kb_j)(A_jB_k-A_kB_j)$$
的恒等式

**Answer**

考虑证明一般的二重和式，令
$$\begin{aligned}
    S=\sum_{1 \le j < k \le n}(a_jb_k-a_kb_j)(A_jB_k-A_kB_j)
\end{aligned}$$

利用交换律，可以将$k, j$互换从而得到
$$\begin{aligned}
    S &= \sum_{1 \le k < j \le n}(a_kb_j-a_jb_k)(A_kB_j-A_jB_k)\\\\
    &= \sum_{1 \le k < j \le n}(a_kb_jA_kB_j-a_kb_jA_jB_k-a_jb_kA_kB_j+a_jb_kA_jB_k) \\\\
    &= \sum_{1 \le k < j \le n}((a_jb_k-a_kb_j)A_jB_k-(a_jb_k-a_kb_j)A_kB_j) \\\\
    &= \sum_{1 \le k < j \le n}(a_jb_k-a_kb_j)(A_jB_k-A_kB_j)
\end{aligned}$$

考虑到
$$\sum_{1\le j < k \le n}a + \sum_{1\le k < j \le n}a  = \sum_{1\le j, k \le n}a  - \sum_{1\le j = k \le n}a$$

则有
$$\begin{aligned}
    2S &= \sum_{1 \le j < k \le n}(a_jb_k-a_kb_j)(A_jB_k-A_kB_j) + \sum_{1 \le k < j \le n}(a_jb_k-a_kb_j)(A_jB_k-A_kB_j) \\\\
    &= \sum_{1 \le j, k \le n} (a_jb_k-a_kb_j)(A_jB_k-A_kB_j) - \sum_{1 \le j = k \le n}(a_jb_k-a_kb_j)(A_jB_k-A_kB_j) \\\\
    &= \sum_{1 \le j, k \le n} (a_jb_kA_jB_k-a_jb_kA_kB_j-a_kb_jA_jB_k+a_kb_jA_kB_j) - 0 \\\\
    &= \sum_{1 \le j, k \le n}(a_jb_kA_jB_k + a_kb_jA_kB_j) - \sum_{1 \le j, k \le n}(a_jb_kA_kB_j+a_kb_jA_jB_k) \\\\
    &= 2\sum_{1 \le k \le n}a_kA_k\sum_{1 \le k \le n}b_kB_k-2\sum_{1 \le k \le n}a_kB_k\sum_{1 \le k \le n}b_kA_k \\\\
\end{aligned}$$

故而
$$S=\sum_{1 \le k \le n}a_kA_k\sum_{1 \le k \le n}b_kB_k - \sum_{1 \le k \le n}a_kB_k\sum_{1 \le k \le n}b_kA_k$$

将$A=a, B=b$带入可得
$$
\begin{aligned}
    \sum_{1\le j < k \le n}(a_jb_k - a_kb_j)^2 &= \sum_{1 \le k \le n}a_ka_k\sum_{1 \le k \le n}b_kb_k - \sum_{1 \le k \le n}a_kb_k\sum_{1 \le k \le n}b_ka_k \\\\
    &=\left( \sum_{k=1}^n a_k^2 \right)\left( \sum_{k=1}^n b_k^2 \right) - \left( \sum_{k=1}^n a_kb_k \right)^2
\end{aligned}
$$
