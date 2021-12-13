---
title: 线性MBA表达式存在定理证明
date: 2019-10-10 13:22
updated: 2019-10-10 16:41
mathjax: true
meta:
    date: false
categories: 
    - Computer Science
    - Information Security
tags:
    - MBA
---

定理来源参见论文*Information Hiding in Software with Mixed Boolean-Arithmetic Transforms*中定理1.

---

<!-- more -->

## 1. MBA表达式的定义

**多项式MBA表达式**形如：
$$\sum_{i \in I} a_i (\prod_{j \in J} e_{i,j}(x_1,...,x_t))$$
其中$a_i$是整数，$x_i$是在$B^n$上的变量，$e_{i,j}$是对变量$x_i$的按位表达式。例如：
$$f(x,y,z,t)=8458(x \lor y \land z)^3((xy)\land x \lor t) + x + 9(x \lor y)yz^3$$
是一个多项式MBA表达式，其中，表达式中变量总共有4个，即$t = 4$,即$x_1 = x, x_2 = y, x_3 = z$, $x_4 = t$，另外，$a$总共有3个值，分别是$a_0=8458, a_1=1, a_2 = 9$。\
对于$a_0$，总共有4个$e_{i,j}$作为其右部累积，即$e_{0,0}=e_{0,1}=e_{0,2}=x \lor y \land z,e_{0,3}=(xy) \land x \lor t$。\
同样的，$a_1$仅又结合了一个$e_{0,0}=x$。
至此算是比较清晰的解释了什么是MBA表达式

**线性MBA表达式**形如：
$$\sum_{i \in I}a_i e_i(x_1,...x_t)$$
线性MBA表达式是多项式MBA表达式的子集，例如：
$$f(x,y)=x+y-(x \oplus (\lnot y)) - 2(x \lor y) + 12564$$
是一个线性MBA表达式，其变量仅两个$x,y$，而$a=\lbrace 1,1,-1,-2,12564 \rbrace$，对于每个$a_i$，**仅右结合一个$e_i$**，这是线性MBA和多项式MBA的区别。

## 2. 定理1:线性MBA表达式存在定理

假设$n,s,t$为正整数，
1. 变量$x_i \in B^n, i=1,...,t$，即$x_i$形如$1011...010_{(n)}$；
2. $a_j$是一个正整数，$j=0,1,...,s-1$;
3. $e_j$是对$x_i$的**按位表达式**，$j=0,1,...,s-1$。由于该定理适用于线性MBA，因此对每个$a_j$仅右结合一个$e_j$，例如$e_j(x_1,x_2,x_3)=x_1 \lor x_2 \oplus x_3$,尤其需要注意的是，由于$e_j$是按位运算，因此有：$e_j(x_1,...,x_t)=\left( \begin{matrix} f_j(x_{1,0},...,x_{t,0}) \\\\ \vdots \\\\f_j(x_{1,n-1},...,x_{t,n-1}) \end{matrix} \right) = \left( \begin{matrix} f_j(y_0) \\\\ \vdots \\\\f_j(y_0) \end{matrix} \right)$
4. $e=\sum_{j=0}^{s-1}a_j e_j$是一个线性MBA表达式，则$e_j$是一个映射：$e_j:B^{n \times t} \to B$，由于$B$这个二进制数可以转换成模$2^n$的整数(在计算机中整数由补码表示，假设所有整数的二进制位数为$n$，则计算机中所有的数都需要模$2^n$)，因此$e_j:B^{n \times t} \to Z/2^n$，同样的,$e:B^{n \times t} \to Z/2^t$；
5. $F=\left( \begin{matrix} f_0(0) \cdots f_{s-1}(0) \\\\ \vdots \\\\f_0(2^t-1) \cdots f_{s-1}(2^t-1) \end{matrix} \right)$是一个$2^t \times s$的矩阵，表示$f_j$对每一bit位操作的所有可能的值，在Zhou的论文中说$F$是一个真值表；
6. $V=(a_0,...,a_{s-1})^T$；

$e=0$当且仅当$F \cdot V = 0$在环$Z/2^n$上有非平凡的解。

## 3. 证明

对于定理中的第5点，$F$为什么是$2^t \times s$，原因是，首先$f_j$也就是$e_j$总共有$s-1$个不同的表达式，这是列数为$s$的原因，其次，$y_i$是一个$t \times 1$的向量，$y_i$中的每个分量都是从$x_j$中第$i$个位置取出，例如$y_i=(1,0,1)$，那么对这个$t$位的$y_i$，其可能取值自然是$(0,1,...,2^t-1)$，也就有了$f_j$所有可能的取值是$(f(0),f(1),...,f(2^t-1))$，矩阵因此而来。

假设$F \cdot V = 0$，有：
$\forall x_1,...x_t \in B^n, y_i = (x_{1,i},...,x_{t,i}), i=0,...,n-1$,
$$
e(x_1,...,x_t)=\sum_{j=0}^{s-1}a_je_j(x_1,...,x_t)
$$

对于$e_j$有:
$$e_j(x_1,...,x_t) = [f_j(y_0),...,f_j(y_{n-1})] = \sum_{i=0}^{n-1}2^i f_j(y_i) \tag{1}$$
例如，$(x_1,x_2,x_3) = (101,110,001)$，且$e_j(x_1,x_2,x_3)=x_1 \land x_2 \oplus x_3$,则将$x_1,x_2,x_3$代入后可得$e_j(x_1,x_2,x_3)=101=5$,因为$e_j$是按位运算，因此可以把$e_j$拆分为$n$个$f_j$，每个$f_j$都是相同的，且每个$f_j$都是对一个不同变量的同一bit位操作的，这样就可以得到$e_j(x_1,x_2,x_3) = e_j(101,110,001)=[f_j(110), f_j(010), f_j(101)]=2^0f_j(110) + 2^1f_j(010) + 2^2f_j(101) = 5$;
将$(1)$带入$e$中可以得到：
$$e = \sum_{j=0}^{s-1}a_j e_j(x_1,...x_t) = \sum_{j=0}^{s-1}a_j \sum_{i=0}^{n-1}2^if_j(y_i)=\sum_{i=0}^{n-1}2^i \sum_{j=0}^{s-1}a_j f_j (y_i) \tag{2}$$
在$F \cdot V=0$中，有$\left(\begin{matrix} \vdots \\\\f_0(y_i) \cdots f_{s-1}(y_i) \\\\ \vdots \end{matrix} \right) \cdot \left( \begin{matrix} a_0 \\\\ a_1 \\\\ \vdots \\\\ a_{s-1}\end{matrix} \right) = \left( \begin{matrix} 0 \\\\ 0 \\\\ \vdots \\\\ 0\end{matrix} \right)$,即$\sum_{j=0}^{s-1}a_j f_j(y_i)=0$,代入$(2)$中可以得到：
$$
(2)=\sum_{i=0}^{n-1}2^i \sum_{j=0}^{s-1}a_j f_j (y_i)=\sum_{i=0}^{n-1}2^i \times 0 = 0
$$
以上证明了$F \cdot V = 0 \Rightarrow e = 0$,下面证明$e=0 \Rightarrow F \cdot V = 0$:

假设$e \equiv 0$，令$m_i = \sum_{j=0}^{s-1}a_j f_j (x_{1,i}, ... , x_{t,i})$，则有：
$$e=\sum_{i=0}^{n-1}2^i \cdot m_i = 2^0m_0 + 2^1m_1 +...+2^{n-1}m_{n-1} \equiv 0 \tag{3}$$
假定我们有任意输入$x_{1,k},...,x_{t,k}$，且其$m_k = \bar{m} \not = 0$,则将其代入$(3)$中有：
$$e=2^0 \bar{m} + 2^1 \bar{m} +...+2^{n-1} \bar{m} \equiv 0 \Rightarrow \bar{m} = 0$$
但是$\bar{m}=0$与假设的$\bar{m} \not = 0$矛盾，因此可以得出结论对任意的$m_i$都有$m_i=0$，即$\sum_{j=0}^{s-1}a_j f_j (x_{1,i}, ... , x_{t,i})=0$，进而有$F \cdot V = 0$.


