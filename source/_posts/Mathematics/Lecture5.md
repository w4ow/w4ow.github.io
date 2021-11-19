---
title: 形式语言与计算复杂性(四)：可判定性
date: 2021-07-04 16:00
# updated: 0000-00-00 00:00
mathjax: true
meta:
    date: true
categories: 
    - Mathematics
tags:
    - Lecture
---

{% post_link Mathematics/Lecture1 Lecture 0：绪论 %}<br>
{% post_link Mathematics/Lecture2 Lecture 1：正则语言 %}<br>
{% post_link Mathematics/Lecture3 Lecture 2：上下文无关语言 %}<br>
{% post_link Mathematics/Lecture4 Lecture Church-Turing %}
Lecture 4: 可判定性
{% post_link Mathematics/Lecture6 Lecture 5：可规约性 %}<br>
{% post_link Mathematics/Lecture7 Lecture 6：时间复杂度 %}

---

<!-- more -->

# 1. 可判定性

我们研究问题的可判定性，其目的是在拿到一个问题时，先看看它是否能被图灵机解决，对于无法解决的问题，我们也就没有必要花费精力去研究了。如何研究问题的可判定性呢？首先从语言的可判定性入手，如果一个语言是不可判定的，那么以该语言描述的问题肯定是不可判定的。首先，最基本的，正则语言RL和上下文无关语言CFL都是可判定的，然后我们再介绍几种可判定语言。

## 1.1 正则语言的可判定性

1. 语言$A_{DFA}=\lbrace\langle B,w\rangle|B\mathrm{\ is\ a\ DFA\ that\ accepts\ input\ string}\ w\rbrace$是可判定的。
   **证明：**
   首先弄清楚我们要证明的是什么，$B$是一个DFA，本质上就是一个五元组，$w$是任意的字符串。然后我们给定一个图灵机$M$，其输入是一个二元组$\langle B,w\rangle$，$M$要判定这个$w$能否被这个$B$所识别，如果能识别，则$M$返回$accpet$，否则$reject$。
   搞清楚了这个，我们就可以构造一个$M$来判定$A_{DFA}$
   > $M$ = "给定输入$\langle B,w\rangle$，其中$B$是DFA，$w$是字符串：
   > 1. 首先$M$检查$B$和$w$是否是合法的，不是合法的则直接返回$reject$
   > 2. 使用$M$来模拟$B$，且输入是$w$
   >    1. 开始时$B$的状态是$q_0$，且当前输入位置为$w$的最左边
   >    2. 根据$B$的状态转移函数$\delta$更新$M$的状态和输入位置
   > 3. 如果模拟完成后，$M$处于$B$的接受状态，则$M$返回$accept$；当$M$不处于$B$的接受状态，则$reject$"

2. 语言$A_{NFA}=\lbrace\langle B,w\rangle|B\mathrm{\ is\ a\ NFA\ that\ accepts\ input\ string}\ w\rbrace$是可判定的。
   **证明：**
   因为任意NFA都有一个等价的DFA，而我们在上面已经证明了图灵机$M$可以判定$A_{DFA}$，因此我们利用$M$进行证明：
   > $N$= "给定输入$\langle B,w\rangle$，其中$B$是NFA，$w$是字符串：
   > 1. 首先将NFA $B$转换为等价的DFA $C$
   > 2. 使用上面的图灵机$M$来模拟$C$，输入信号为$w$
   > 3. 如果模拟结束时，$N$处在接受状态，则$accpet$，否则$reject$"

3. 语言$A_{REX}=\lbrace\langle R,w\rangle|B\mathrm{\ is\ a\ regular\ expression\ that\ generate\ string}\ w\rbrace$是可判定的。
   **证明：**
   同样的，任意正则表达式都可转换为NFA：
   > $P$= "给定输入$\langle R,w\rangle$，其中$R$是正则表达式，$w$是字符串：
   > 1. 首先将$R$转换为等价的NFA $A$
   > 2. 使用上面的图灵机$N$来模拟$A$，输入信号为$w$
   > 3. 如果模拟结束时，$P$处在接受状态，则$accpet$，否则$reject$"

4. 语言$E_{DFA}=\lbrace\langle A\rangle|A\mathrm{\ is\ a\ DFA\ and}\ L(A)=\emptyset\rbrace$是可判定的(即一个不能识别任何字符串的DFA $A$)
   **证明**
   > $T$ = "给定输入$\langle A\rangle$，其中$A$是一个DFA：
   > 1. 首先标记$A$的开始状态
   > 2. 利用图灵机$T$反复标记那些**由已标记状态能够到达的状态**
   > 3. 如果没有任何一个$A$的接受状态被标记，则$T$返回$accept$，否则$reject$"

5. 语言$EQ_{DFA}=\lbrace\langle A,B\rangle|A\ \mathrm{and}\ B\mathrm{\ are\ DFAs\ and}\ L(A)=L(B)\rbrace$是可判定的
   **证明**
   直接证明有一定的难度，因此我们需要对其进行转换，因为上面已经证明了$E_{DFA}$是可以判定的，因此我们可以尝试将问题转换为$E_{DFA}$，即构造某种语言且该语言的自动机不能识别任何字符串。
   > $F$ = "给定输入$\langle A,B\rangle$，其中$A$和$B$是DFA
   > 1. 我们构造一个新的DFA $C$，使得$L(C)=(L(A)\cap\overline{L(B)})\cup(\overline{L(A)}\cap L(B))$，因为FA具有封闭性，因此$C$依然是一个DFA
   > 2. 利用图灵机$T$模拟$C$
   > 3. 如果$T$返回$accept$，则$F$返回$accept$，否则$reject$.

## 1.2 上下文无关语言的可判定性

1. 语言$A_{CFG}=\lbrace\langle G,w\rangle|G\mathrm{\ is\ a\ CFG\ that\ generates\ string}\ w\rbrace$是可判定的。
   **证明**
   我们可以用图灵机模拟CFG，从开始变量一直推导，看看能否产生$w$，但是因为推导过程中可能产生循环，因此推导过程可能是无限的。我们必须转换思路，一种可行的办法是将CFG转换为CNF，而从一个CNF推导出一个字符串$w$只要$2n-1$步，因此不会陷入循环。说明一下，**因为CNF只有两种形式的规则，假设目标字符串$w$的长度为$n$，从开始变量$S$开始，推导出一个长度为$n$且全为变量的字符串需要$n-1$步，然后将这$n$个变量变成终结符需要$n$步，因此共计$2n-1$步。**
   > $S$ = "给定输入$\langle G,w\rangle$，其中$G$是CFG，$w$是字符串
   > 1. 将$G$转换为CNF形式
   > 2. 列出所有在$2n-1$步内能够推导出的字符串，其中$n=|w|$。如果$n=0$，那么列出所有一步能推导出的字符串。
   > 3. 如果这些字符串中存在$w$，则$S$返回$accept$，否则$reject$.

   由$A_{CFG}$的可判定性我们也可以得到定理：
   > **定理**：所有的上下文无关语言都是可判定的
   因为所有的上下文无关语言都有等价的CFG，我们可以构造上下文无关语言的CFG，并利用图灵机$S$来进行判定。

2. 语言$E_{CFG}=\lbrace\langle G\rangle|G\mathrm{\ is\ a\ CFG\ and}\ L(G)=\emptyset\rbrace$是可判定的(即一个不能生成任何字符串的CFG $G$)
   **证明**
   我们可以利用上面的图灵机$S$，枚举所有可能的字符串，并查看图灵机能否生成该字符串，如果所有的字符串都不能生成，则$accept$。但是枚举所有的字符串是不可行的，因此我们需要换个思路。
   > $R$ = "给定输入$\langle G\rangle$，其中$G$是一个CFG：
   > 1. 首先标记$G$的所有终结符
   > 2. 利用图灵机$R$利用右边规则反复标记变量：**对于$G$中的规则$A\to U_1U_2\cdots U_k$，如果$U_1,...,U_k$都被标记了，那么标记变量$A$**
   > 3. 如果开始变量没有被标记，则$R$返回$accept$，否则$reject$"

3. 语言$EQ_{CFG}=\lbrace\langle G,H\rangle|G\ \mathrm{and}\ H\mathrm{\ are\ CFGs\ and}\ L(G)=L(H)\rbrace$是**不可判定的**
   DFA之所以能判定，是因为FA具有封闭性，因此可以构造等价的DFA $C$，但是上下文无关语言不具有封闭性。

最后我们给出RL、DFL和可判定语言之间的关系。
<img src="5_1.png" width="40%" height="40%">

---

# 2. 不可判定性 

在上图中我们了解到，有许多语言是图灵机可识别但不可判定的，例如语言$A_{TM}=\lbrace\langle M,w\rangle|M\mathrm{\ is\ a\ TM\ and}\ M\ \mathrm{accepts}\ w\rbrace$是图灵可识别但不可判定的：我们可以利用一台图灵机$N$来模拟图灵机$M$，如果$M$返回$accept$则$N$返回$accept$，但是，由于$M$中可能存在循环，所以是不可判定的。在这一节中，我们将学习一些数学理论，用于判断一个语言是否是可判定的。

## 2.1 映射与可数性

首先我们了解三种映射：单射、满射与双射。假设我们有两个集合$A$和$B$，以及一个函数$f$可以将$A$中的元素$x$映射到$B$中的元素$y$，我们称$f$为：
1. 单射injective, one-to-one：$f:A\to B$是单射的当且仅当对所有的$a,b\in A$，有$f(a)=f(b)\Rightarrow a=b$
2. 满射surjective, onto：$f:A\to B$是满射的当且仅当对所有的$b\in B$，存在$a\in A$满足$f(a)=b$
3. 双射bijective, correspondence：$f:A\to B$是双射的当且仅当对所有的$b\in B$，存在**唯一**的$a\in A$满足$f(a)=b$
<img src="5_2.png" width="60%" height="60%">

**如果存在满射函数$f$使得$f:A\to B$，则我们称$A$和$B$的尺寸是相同的**，例如，令$y=f(x)=2x$，自然数集$\mathcal{N}=\lbrace1,2,3,...\rbrace$和正偶数集合$\Lambda=\lbrace2,4,6,...\rbrace$的尺寸是相同的。

> **可数的**：如果一个集合的元素是有限的或者其尺寸和自然数集$\mathcal{N}$相等，则称其为可数的。

例如，集合$Q=\lbrace\frac{m}{n}|m,n\in\mathcal{N}\rbrace$是可数的，我们可以按下图方式把所有的$\frac{m}{n}$都列出来：
<img src="5_3.png" width="35%" height="35%">

即第一行是所有分子为$1$的数，第二行是所有分子为$2$的数，以此类推。可以看到，每一行每一列都有无穷个数，但是我们可以按照图中箭头方向将数进行排列，使其对应到$\mathcal{N}$，即$f(1)=\frac{1}{1}$，$f(2)=\frac{2}{1}$，$f(3)=\frac{1}{2}$，以此类推。注意，因为$\frac{1}{1}$和$\frac{2}{2}$相等，所以我们排除$\frac{2}{2}$，其他相等的数同样需要排除。按照此方法，我们可以构造一个映射函数$f$将$Q$和$\mathcal{N}$一一对应，所以$Q$是可数的。

可以看到，尽管$Q$和$\mathcal{N}$都有无穷个元素，我们照样将其称为可数的，但对有些无穷集，因其尺寸不等于$\mathcal{N}$，我们称其为不可数的，最典型的就是实数集$\mathcal{R}$。我们可以利用反证法来证明$\mathcal{R}$是不可数的。假设$\mathcal{R}$是可数的，那么必定存在一个映射$f$使得$\mathcal{R}$和$\mathcal{N}$的元素一一对应。假设$f$是确定的，那么对应的$f(\mathcal{N})$也是确定的，假设元素$1$对应的元素是$f(1)=3.14159$，同样$2$对应$f(2)=55.5555$，以此类推，如下表所示，第一列是$\mathcal{N}$，第二列是$\mathcal{R}$。现在我们构造一个$x\in\mathcal{R}$，并证明$x$不对应任何自然数$n\in\mathcal{N}$。
<img src="5_4.png" width="35%" height="35%">

我们想要构造一个$x$使得$x\not=f(n)$，我们假设$x$是一个$0$和$1$之间的小数。首先，为了保证$x\not=f(1)$，我们让$x$的十分位不等于$f(1)$的十分位，$f(1)$的十分位是$1$，所以我们可以随便取个不为$1$的数，假设取$4$。同样的，为了$x\not=f(2)$，我们取$x$的千分位为$6$，以此类推。**因为$x$的第$n$位小数与$f(n)$的第$n$位小数不同，所以$x\not=f(n)$，即不存在任意一个$n$，能够使得$f(n)=x$**，也就是$x$对应不到任意一个$n$，所以$\mathcal{R}$和$\mathcal{N}$不是双射，所以$\mathcal{R}$不可数。

## 2.2 非图灵可识别语言

接下来，我们证明一个重要的推论：
> **推论**：有些语言不是图灵可识别的。

**证明**：
假设所有的语言都是图灵可识别的，那么所有的图灵机的集合和所有的语言的集合应当有相同的尺寸，接下来，我们证明两者的尺寸并不相同，更具体的说，图灵机集是可数的，语言集是不可数的。
1. 因为没一个图灵机$M$都可以编码为一个字符串$\langle M\rangle$，而所有的字符串集合是可数的(可以与$\mathcal{N}$一一对应)，因此图灵机集是可数的。
2. 令语言集为$\mathcal{L}$，我们接下来证明$\mathcal{L}$不可数
   1. **无限二进制序列**是一个由$0$和$1$组成的、长度无限的二进制序列，令$\mathcal{B}$为无限二进制序列集合。$\mathcal{B}$是不可数的，其证明过程与$\mathcal{R}$相同(即取第$i$位与$f(i)$的第$i$位不同)。
   2. 首先，我们证明$\mathcal{L}$和$\mathcal{B}$之间存在双射$f$。
      1. 我们假设$\mathcal{L}$的字母表为$\Sigma$，我们令字母表幂集为$\Sigma^\ast=\lbrace s_1,s_2,...\rbrace$，显然$\Sigma^\ast$是一个无限集。
      2. 每个语言$A\in\mathcal{L}$在$\mathcal{B}$中都有唯一的被称为**特征序列**的序列$\mathcal{X}_A$，当$s_i\in A$时，我们令$\mathcal{X}_A$的第$i$位为$1$,否则为$0$。例如，假设字母表为$\Sigma=\lbrace0,1\rbrace$，且给定$A$如下，则$A$的特征序列$\mathcal{X}_A$如图所示，显然，**$\mathcal{X}_A$是一个无限二进制序列**。
      <img src="5_5.png" width="50%" height="50%">
      3. 因此，存在一个$f:\mathcal{L}\to\mathcal{B}$，其中$f(A)$等于$A$的特征序列，使得$\mathcal{L}$和$\mathcal{B}$是双射的。
   3. 因为$\mathcal{L}$和$\mathcal{B}$是一一对应的，而$\mathcal{B}$是不可数的，因此$\mathcal{L}$是不可数的
3. 因为图灵机集可数，而语言集不可数，所以有些语言不是图灵可识别的。

## 2.3 一个重要的不可判定语言

> **定理**：语言$A_{TM}=\lbrace\langle M,w\rangle|M\mathrm{\ is\ a\ TM\ and}\ M\ \mathrm{accepts}\ w\rbrace$是不可判定的。

### 2.3.1 反证法证明

**证明**
我们假设$A_{TM}$是可判定的，并且存在一个图灵机$H$可以判定$A_{TM}$。给定输入字符串$\langle M,w\rangle$，$M$是一个图灵机而$w$是一个字符串，当$M$返回$accept$时$H$也返回$accept$，否则$reject$，如下式：
$$H(\langle M,w\rangle) = \begin{cases}
    accept & \mathrm{if}\ M\ \mathrm{accepts}\ w \\\\
    reject & \mathrm{if}\ M\ \mathrm{does\ not\ accepts}\ w
\end{cases}$$

我们构建一个新的图灵机$D$，且$D$调用$H$作为自己的子程序，我们让$D$判断一件特殊的事情：我们令$w=\langle M\rangle$，并输入到$M$。随后**我们在$D$上运行$H$来判定$M$能否判定$\langle M\rangle$**，且$D$返回$M$的相反结果，即**当$M$返回$accept$时$D$返回$reject$，$M$返回$reject$时$D$返回$accept$**。下面是$D$的正式描述：

$D$ = "给定输入$\langle M\rangle$，其中$M$是一个图灵机：
　　１. 运行输入为$\langle M,\langle M\rangle\rangle$的图灵机$H$
　　２. 当$H$返回$accept$时，$D$返回$reject$，当$H$返回$reject$时，$D$返回$accept$"

即
$$D(\langle M\rangle) = \begin{cases}
    accept & \mathrm{if}\ M\ \mathrm{does\ not\ accepts}\ \langle M\rangle \\\\
    reject & \mathrm{if}\ M\ \mathrm{accepts}\ \langle M\rangle
\end{cases}$$

那么，如果我们将$D$的描述$\langle D\rangle$送到$D$中运行，会发生什么呢？
$$D(\langle D\rangle) = \begin{cases}
    accept & \mathrm{if}\ D\ \mathrm{does\ not\ accepts}\ \langle D\rangle \\\\
    reject & \mathrm{if}\ D\ \mathrm{accepts}\ \langle D\rangle
\end{cases}$$

不管$D$是什么样的，出现这种情况都是很荒谬的，因此上述定理得以证明。

### 2.3.2 对角化

我们可以利用对角化的思想更形象的阐述上面的证明过程。首先，$M$，$H$和$D$还是和上一小节一样定义，然后我们可以画一个表，该表的列是世界上所有的图灵机，行则是这图灵机所对应的描述。如果图灵机$M_i$能识别字符串$\langle M_j\rangle$，则将表的$(i,j)$置为$accept$，如果$M_i$不能识别(即$M_i$返回$reject$或陷入循环)，则将表的$(i,j)$置为$reject$。因为该表列出了所有的图灵机，而$D$也是图灵机，所以$D$肯定是这些列出的图灵机中的一个，然后，根据上面我们对$D$的构造，它返回相反的结果，即当$M_i$对$\langle M_i\rangle$返回$accept$时，$D$对$\langle M_i\rangle$返回$reject$，则我们可以得到如下的表：
<img src="5_6.png" width="50%" height="50%">
问题来的，我们无法标出?处。

## 2.4 图灵不可识别语言

除了图灵不可判定语言之外，还有一些语言甚至是图灵不可识别的。我们首先定义**co-Turing-recognizable**为一个图灵可识别语言的补集，注意，**一个语言是一个字符串集合，其co-Turing-recognizable，是这个集合的补集，而不是图灵不可识别语言**，而且

> **定理**:一个语言是可判定的当且仅当它是图灵可识别的且它的补集也是图灵可识别的(co-Turing-recognizable)。

1. 首先我们来证明，**如果一个语言$A$是可判定的，那么它和它的补集都是图灵可识别的**。首先，$A$都是可判定的了，那么肯定是图灵可识别的。其次，对$A$的补集$\overline{A}$，我们可以构造衣蛾可以判定$A$的图灵机，用它来判定$\overline{A}$，且返回相反的结果，所以可判定语言的补集也是可判定的，当然也是可识别的
2. 接下来证明，**如果一个语言和它的补集都是图灵可识别的，那么该语言是可判定的**。我们构造两个图灵机，$M_1$可以识别$A$，$M_2$可以识别$\overline{A}$，那么我们可以构造一个新的图灵机$M$:
   
   $M$ = "给定输入$w$
   　　1. 在$M_1$和$M_2$上并行运行输入$w$
   　　2. 如果$M_1$返回accept，则$M$返回$accept$；如果$M_2$返回reject，则$M$返回$reject$"
   
   显然，这样的机器能够$accept$语言$A$，并且$reject$语言$\overline{A}$，即$A$是可判定的

根据上面的定理，我们可以得到推论：**$\overline{A_{TM}}$不是图灵可识别的**。因为$A_{TM}$是图灵可识别的，若$\overline{A_{TM}}$也是图灵可识别的，那$A_{TM}$就是图灵可判定的了，这显然是矛盾的。

---

# 作业

1. Answer all parts for the following DFA $M$ and give reasons for your answers.

   <img src="e1.png" width="50%" height="50%">

   **Answer**
　　1. 是，因为$M$能识别$0100$
　　2. 否，$M$不能识别$011$
　　3. 否，$A_{DFA}$的输入应形如$\langle M,w\rangle$，所以输入格式不正确
　　4. 否，$A_{REX}$输入的第一元应当为正则表达式而不是图灵机，输入格式不正确
　　5. 否，$E_{DFA}$要求$L(M)=\emptyset$，显然这里的$L(M)\not=\emptyset$
　　6. 是，$EQ_{DFA}$要求$L(A)=L(B)$，显然这里$L(M)=L(M)$

2. Let $ALL_{DFA}=\lbrace\langle A\rangle|A\mathrm{\ is\ a\ DFA\ and\ }L(A)=\Sigma^\ast\rbrace$. Show that $ALL_{DFA}$ is decidable.
   
   **Answer**
   构造一个新的TM $P$
   $P$ = "给定输入$\langle A\rangle$，其中$A$是一个DFA：
　　１. 构造一个新的DFA $B$，且$B$所识别的语言$L(B)=\overline{L(A)}$
　　２. 使用自动机$T$来判定基于输入$\langle B\rangle$的$E_{DFA}$
　　３. 如果$T$返回accept，则$P$返回$accept$，否则$T$返回$reject$

3. Let $X$ be the set $\\{1, 2, 3, 4, 5\\}$ and $Y$ be the set $\\{6, 7, 8, 9, 10\\}$. We describe the functions $f:X\to Y$ and $g:X\to Y$ in the following tables. Answer each part and give a reason for each negative answer.

   <img src="e3.png" width="50%" height="50%">
   
   **Answer**
   $$
   \begin{aligned}
      & \mathbf{a}. 否 f(2)=f(4) & \mathbf{d}. 是 \\\\
      & \mathbf{b}. 否 Y中的8、9、10没有对应的x\in X & \mathbf{e}. 是 \\\\
      & \mathbf{c}. 否 & \mathbf{f}. 是
   \end{aligned}
   $$

4. Let $\mathcal{B}$ be the set of all infinite sequences over $\\{0,1\\}$. Show that $\mathcal{B}$ is uncountable
using a proof by diagonalization.
   
   **Answer**

   假设$\mathcal{B}$是可数的，那么必定存在一个映射$f$使得$\mathcal{B}$和$\mathcal{N}$的元素一一对应。假设元素$1\in\mathcal{N}$对应的元素是$f(1)=01000\cdots$，$2$对应$f(2)=00010\cdots$，以此类推，如下表所示，第一列是$\mathcal{N}$，第二列是$\mathcal{B}$。现在我们构造一个$x\in\mathcal{B}$，使其第一位不等于$f(1)$的第一位，即令$x$的第一位为$1$，同样的，$x$的第二位不等于$f(2)$的第二位，令$x$的第二位为$1$，以此类推。
   
   |$n$ | $f(n)$ |
   | ---| ---    |
   $1$ | $\underline{0}1000\cdots$ |
   $2$ | $0\underline{0}010\cdots$ |
   $3$ | $10\underline{0}00\cdots$ |
   $4$ | $000\underline{0}0\cdots$ |
   $\vdots$ | $\vdots$ |
   
   构造出的$x=1111\cdots$，因为$x$的第$n$位与$f(n)$的第$n$位不同，所以$x\not=f(n)$，即不存在$n$使得$f(n)$等于$x$，故而不存在双射$f:\mathcal{N}\to\mathcal{B}$，故而$\mathcal{B}$不可数。