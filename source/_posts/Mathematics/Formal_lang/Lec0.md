---
title: 形式语言与计算复杂性(零)：绪论
date: 2021-06-16 15:00
# updated: 0000-00-00 10:00
mathjax: true
meta:
    date: true
categories: 
    - Mathematics
tags:
    - Lecture
---

USTC2021春季学期研究生课程《形式语言与计算复杂性》

教材:[Introduction to the Theory of Computation 2018](http://fuuu.be/polytech/INFOF408/Introduction-To-The-Theory-Of-Computation-Michael-Sipser.pdf)

作业要求：布置一周内完成，BB上提交

考试要求：
- 闭卷，可带一张带有笔记的A4纸
- 送分：课后作业相关，80分
- 压轴：课件中所提到的证明，20分

Lecture 0：绪论
{% post_link Mathematics/Formal_lang/Lec1 Lecture 1：正则语言 %}<br>
{% post_link Mathematics/Formal_lang/Lec2 Lecture 2：上下文无关语言 %}<br>
{% post_link Mathematics/Formal_lang/Lec3 Lecture 3：Church-Turing %}<br>
{% post_link Mathematics/Formal_lang/Lec4 Lecture 4：可判定性 %}<br>
{% post_link Mathematics/Formal_lang/Lec5 Lecture 5：可规约性 %}<br>
{% post_link Mathematics/Formal_lang/Lec6 Lecture 6：时间复杂度 %}

---

<!-- more -->

# 1 符号与术语

## 1.1 集合

- 集合(set)：一组不包含重复对象的整体，无顺序关系
- $\in$和$\not\in$，表示元素和集合的从属关系
- $\subseteq$和$\not\subseteq$，表集合与集合之间的从属关系
- 多重集(multiset)：允许集合中出现重复元素
- 无穷集：包含无穷元素的集合，如自然数集$\mathcal{N}=\lbrace1,2,3,...\rbrace$，整数集$\mathcal{Z}$
- 空集$\emptyset$：没有元素的集合，注意$\emptyset\not=\lbrace\rbrace$
- $\lbrace n|\mathrm{rule\ about\ }n\rbrace$:集合的另一种写法
- $\cup,\cap, \overline{A}$:交、并、补

## 1.2 序列与元组

- 序列：有序的对象组合
- 元组(tuple)：有限序列，$k$个对象的序列称为$k$-元组，$2$-元组亦称为有序对
- 幂集(power set)：集合的所有子集(包括空集)
- 笛卡尔积$\times$：两个集合$A,B$中的元素组成的所有有序对的集合，其中第一个元素来自$A$，第二个元素来自$B$。例：
  
  $A = \lbrace 1, 2\rbrace,B = \lbrace x, y, z\rbrace$

  $A \times B = \lbrace (1, x),(1, y),(1, z),(2, x),(2, y),(2, z)\rbrace$

- $A^k=\underset{k}{\underbrace{A\times A\times \cdots\times A}}$

## 1.3 函数与关系

**函数与映射**
函数是输入输出之间的映射关系

**定义域(doamin)与值域(range)**
- 函数的输入所可能取到的值的集合称为定义域
- 函数的输出所可能取到的值的集合称为值域
- 符号: 定义域为$D$以及值域为$R$的函数$f$标记为
  $$f:D \to R$$

**$k$-ary函数**
当函数$f$的值域为$A_1\times \cdots\times A_k$，其输入为$k$-元组$(a_1,...,a_k)$
- 称$a_i$为$f$的参数
- 称$f$为$k$-ary函数
  - $k=1$时称为一元函数(unary function)
  - $k=2$时称为二元函数(binary fuction)
- 前缀表示与中序表示：
  - 前缀：$aRb$
  - 中缀：$R(a,b)$

**谓词与关系**

谓词(predicate)，是声明的数学形式化术语，表示一个断言，其值域为*true*或*false*。
给定一个谓词，若其定义域为$A\times\cdots\times A$，则称该谓词为一个关系(relation)，亦称之为($A$上的)$k$元关系。例如，2元关系谓词$aRb$意味着$aRb=True$

- 给定定义域$A$,**满足以下三个性质的2元关系$R$称为等价关系**
  - 自反性reflexive: 若$x=True$，则$xRx=True$
  - 对称性symmetric：若$xRy=True$， 则$yRx=True$
  - 传递性transitive: 若$xRy=True,yRz=True$，则$xRz=True$

## 1.4 字符串与语言

- 字母表Alphabet：任意非空有限集合
- 符号Symbols：字母表中的元素
- 本课程中表示字母表的符号包括:$\Sigma, \Gamma$
- 字符串：本课程中的字符串都是基于给定字母表的有限符号序列，例如$01001$是基于$\Sigma=\lbrace0,1\rbrace$的字符串。
- 字符串$w$的长度记为$|w|$
- 空字符串记为$\varepsilon$
- lexicographic order就是字典序,shortlex order等价于字典序，但短字符排在长字符之前
- 语言是字符串的集合
- 前缀：如果存在字符串$x$和$y$的拼接使得$xy=z$，则称$x$是$z$的前缀。若$x\not=z$则称为真前缀
- **前缀无关**：一个语言中(即字符串集合)，不存在一个元素是另一个元素的真前缀，则称该语言前缀无关。
  
## 1.5 布尔逻辑

- 符号：$\lnot, \land, \lor, \oplus$(左右不等则为真), $\leftrightarrow$(左右相等则为真)，$\to$(左假右真则为假)
- 分配律：
  - $P\land(Q\lor R)=(P\land Q)\lor(P\land R)$
  - $P\lor(Q\land R)=(P\lor Q)\land(P\lor R)$

---

# 2 定义、定理与证明

- 定义：对目标的描述，不可存在二义性
- 证明：说明一个声明为真的过程
- 定理：已经被证明为真的数学声明
  - 引理：在证明定理的过程中用到的“低层次的定理”
  - 推论：由定理推而广之得到的结论
- 证明的手段：构造法、反证法、归纳法...

---

# 作业

1. Write formal descriptions of the following sets
   1. The set containing the numbers $1$, $10$, and $100$
   2. The set containing all integers that are greater than $5$
   3. The set containing all natural numbers that are less than $5$
   4. The set containing the string $aba$
   5. The set containing the empty string
   6. The set containing nothing at all
   
   **Answer**:
   1. $\lbrace 1, 10, 100\rbrace$
   2. $\lbrace n|n > 5,n\in\mathcal{N}\rbrace$
   3. $\lbrace n|n < 5,n\in\mathcal{N}\rbrace$
   4. $\lbrace aba\rbrace$
   5. $\lbrace \varepsilon\rbrace$
   6. $\emptyset$


2. Let $A$ be the set $\lbrace x, y, z\rbrace$ and $B$ be the set $\lbrace x, y\rbrace$
   1. Is $A$ a subset of $B$?
   2. IS $B$ a subset of $A$?
   3. What is $A\cup B$?
   4. What is $A\cap B$?
   5. What is $A\times B$?
   6. What is the power set of $B$?
   
   **Answer**
   1. No
   2. Yes
   3. $\lbrace x, y, z\rbrace$
   4. $\lbrace x, y\rbrace$
   5. $\lbrace (x,x), (x,y), (y,x), (y,y), (z,x), (z, y)\rbrace$
   6. $\lbrace \emptyset, \lbrace x\rbrace, \lbrace y\rbrace, \lbrace x, y\rbrace \rbrace$

  
3. For each part, give a relation that satisfies the condition.
   1. Reflexive and symmetric but not transitive
   2. Reflexive and transitive but not symmetric
   3. Symmetric and transitive but not reflexive
   
   **Answer**

   给定定义域$A=\lbrace a, b, c\rbrace$,分别给出三种关系满足以上要求：
   1. $\lbrace(a,a),(b,b),(c,c),(a,b),(b,a),(a,c),(c,a)\rbrace$
   2. $\lbrace(a,a),(b,b),(c,c),(a,b),(b,c),(a,c)\rbrace$
   3. $\lbrace(a,b),(b,a),(b,c),(c,b),(a,c),(c,a)\rbrace$

  
4. Consider the undirected graph $G= (V,E)$ where $V$ , the set of nodes, is $\lbrace 1,2,3,4\rbrace$ and $E$, the set of edges, is $\lbrace\lbrace 1,2\rbrace,\lbrace 2,3\rbrace,\lbrace 1,3\rbrace,\lbrace 2,4\rbrace,\lbrace 1,4\rbrace\rbrace$. Draw the
graph $G$. What are the degrees of each node? Indicate a path from node $3$ to
node $4$ on your drawing of $G$.

  **Answer**
   <img src="08.png" width="20%" height="20%">

