---
title: 形式语言与计算复杂性(三)：Church-Turing
date: 2021-06-29 14:00
# updated: 0000-00-00 00:00
mathjax: true
meta:
    date: true
categories: 
    - Mathematics
tags:
    - Lecture
---

{% post_link Mathematics/Formal_lang/Lec0 Lecture 0：绪论 %}<br>
{% post_link Mathematics/Formal_lang/Lec1 Lecture 1：正则语言 %}<br>
{% post_link Mathematics/Formal_lang/Lec2 Lecture 2：上下文无关语言 %}
Lecture 3: Church-Turing
{% post_link Mathematics/Formal_lang/Lec4 Lecture 4：可判定性 %}<br>
{% post_link Mathematics/Formal_lang/Lec5 Lecture 5：可规约性 %}<br>
{% post_link Mathematics/Formal_lang/Lec6 Lecture 6：时间复杂度 %}
---

<!-- more -->




---

# 作业

1. This exercise concerns TM $M_2$ , whose description and state diagram appear in Example 3.7. In each of the parts, give the sequence of configurations that $M_2$ enters when started on the indicated input string.
    1. $0.$
    2. $00.$
    3. $000.$
    4. $000000.$

    **Answer**
    $M_2\ decides\ A=\lbrace0^{2^n}|n\ge0\rbrace$，此题需要对照状态转移图作答，我们以$\dot{\Gamma}$表示图灵机针头所要读取的磁带上下一个字符。

    1. $q_1\dot{0}, \sqcup q_2\dot{\sqcup}, \sqcup\sqcup q_{accept}$
    2. $q_1\dot{0}0, \sqcup q_2\dot{0}\sqcup, \sqcup xq_3\dot{\sqcup}, \sqcup q_5\dot{x}\sqcup, q_5\dot{\sqcup} x\sqcup, \sqcup q_2\dot{x}\sqcup, \sqcup xq_2\dot{\sqcup}, \sqcup x\sqcup q_{accept}$
    3. $q_1\dot{0}00, \sqcup q_2\dot{0}0, \sqcup x q_3\dot{0}\sqcup, \sqcup x0q_4\dot{\sqcup}, \sqcup x0\sqcup q_{reject}$
    4. 
    $$\begin{aligned}
        & q_1\dot{0}00000               & \sqcup x0x0q_4\dot{0}\sqcup   \qquad   & \sqcup xq_5\dot{0}x0x\sqcup   & \sqcup xxq_3\dot{x}0x\sqcup   \\\\
        & \sqcup q_2\dot{0}0000         & \sqcup x0x0xq_3\dot{\sqcup}   \qquad   & \sqcup q_5\dot{x}0x0x\sqcup   & \sqcup xxxq_3\dot{0}x\sqcup   \\\\
        & \sqcup x q_3 \dot{0}000       & \sqcup x0x0q_5\dot{x}\sqcup   \qquad   & q_5\dot{\sqcup}x0x0x\sqcup    & \sqcup xxxxq_4\dot{x}\sqcup   \\\\
        & \sqcup x0q_4\dot{0}00         & \sqcup x0xq_5\dot{0}x\sqcup   \qquad   & \sqcup q_2\dot{x}0x0x\sqcup   & \sqcup xxxxxq_4\dot{\sqcup}   \\\\
        & \sqcup x0xq_3\dot{0}0         & \sqcup x0q_5\dot{x}0x\sqcup   \qquad   & \sqcup xq_2\dot{0}x0x\sqcup   & \sqcup xxxxx\sqcup q_{reject} \\\\
    \end{aligned}$$

2. This exercise concerns TM $M_1$ , whose description and state diagram appear in Example 3.9. In each of the parts, give the sequence of configurations that $M_1$ enters when started on the indicated input string.
    1. $11.$
    2. $1\\#1.$
    3. $1\\#\\#1.$
    4. $10\\#11.$
    5. $10\\#10.$

    **Answer**
    此题同样需要对照状态转移图作答。

    1. $$\begin{aligned}
       & q_1\dot{1}1 \\\\
       & xq_3\dot{1}\sqcup \\\\
       & x1q_3\dot{\sqcup} \\\\
       & x1\sqcup q_{reject} \\\\
      \end{aligned}$$
    ---
    2. $$\begin{aligned}
        & q_1\dot{1}\\# 1 & xq_1\dot{\\# }x \\\\
        & xq_3\dot{\\# }1 & x\\# q_8\dot{x} \\\\
        & x\\# q_5\dot{1} & x\\# xq_8\dot{\sqcup} \\\\
        & xq_6\dot{\\# }x & x\\# x\sqcup q_{accept} \\\\
        & q_7\dot{x}\\# x \\\\
      \end{aligned}$$
    ---
    3. $$\begin{aligned}
        & q_1\dot{1}\\#\\# 1 \\\\
        & xq_3\dot{\\# }\\# 1 \\\\
        & x\\# q_5\dot{\\# }1 \\\\
        & x\\#\\# q_{reject}1 \\\\
      \end{aligned}$$
    ---
    4. $$\begin{aligned}
        & q_1\dot{1}0\\# 11 & q_7\dot{x}0\\# x1 \\\\
        & xq_3\dot{0}\\# 11 & xq_1\dot{0}\\# x1 \\\\
        & x0q_3\dot{\\# }11 & xxq_2\dot{\\# }x1 \\\\
        & x0\\# q_5\dot{1}1 & xx\\# q_4\dot{x}1 \\\\
        & x0q_6\dot{\\# }x1 & xx\\# xq_4\dot{1} \\\\
        & xq_7\dot{0}\\# x1 & xx\\# x1q_{reject}\sqcup \\\\
      \end{aligned}$$
    ---
    5. $$\begin{aligned}
        & q_1\dot{1}0\\# 10    & xq_7\dot{0}\\# x0    \qquad  & xx\\# xq_4\dot{0}    & xx\\# q_8\dot{x}x    \\\\
        & xq_3\dot{0}\\# 10    & q_7\dot{x}0\\# x0    \qquad  & xx\\# q_6\dot{x}x    & xx\\# xq_8\dot{x}\sqcup  \\\\
        & x0q_3\dot{\\# }10    & xq_1\dot{0}\\# x0    \qquad  & xxq_6\dot{\\# }xx    & xx\\# xxq_8\dot{\sqcup}  \\\\
        & x0\\# q_5\dot{1}0    & xxq_2\dot{\\# }x0    \qquad  & xq_7\dot{x}\\# xx    & xx\\# xx\sqcup q_{accept}    \\\\
        & x0q_6\dot{\\# }x0    & xx\\# q_4\dot{x}0    \qquad  & xxq_7\dot{\\# }xx \\\\
      \end{aligned}$$

3. Explain why the following is not a description of a legitimate Turing machine.
    $M_{bad}$="On input $\left\langle p\right\rangle$, a polynomial over variables $x_1,...,x_k$:
    1. Try all possible settings of $x_1,...,x_k$ to integer values.
    2. Evaluate p on all of these settings.
    3. If any of these settings evaluates to 0, accept ; otherwise, reject ."
   
    **Answer**
    根据Matijasevic̆定理，我们无法为多变量的多项式表达式的每个变量计算bound，因此我们需要枚举每个变量的所有可能的值，这在时间上是不可接受的，甚至会陷入loop。一种似乎可行的图灵机是$k$-磁带图灵机，每条磁带$i$枚举变量$x_i$的可能值。不过这个图灵机的根本问题在于枚举所有可能解的组合所需要的时间是无穷的。

4. Give implementation-level descriptions of Turing machines that decide the following languages over the alphabet $\lbrace0,1\rbrace$.
    1. $\lbrace w|w\ \mathrm{contains\ an\ equal\ number\ of}\ 0s\ \mathrm{and}\ 1s\rbrace$
    2. $\lbrace w|w\ \mathrm{contains\ twice\ as\ many}\ 0s\ \mathrm{as}\ 1s\rbrace$
    3. $\lbrace w|w\ \mathrm{does\ not\ contain\ twice\ as\ many}\ 0s\ \mathrm{as}\ 1s\rbrace$
   
    **Answer**
    1. On input string $w$:
       1. Repeat the following two steps:
          a. Scan the tape and mark the first unmarked $1$. If has no unmarked $1$, jump to step 2, else, move the head back to headmost.
          b. Scan the tape and mark the first unmarked $0$. If has no unmarked $0$, $reject$, else, move the head back to headmost.
       2. Move the head back to headmost and scan the tape. If has unmarked $0$s, $reject$, else, $accept$.
    2. On input string $w$:
       1. Repeat the following two steps:
          a. Scan the tape and mark first two unmarked $0$s. If has no unmarked $0$, jump to step 2, else if has noly one unmarked $0$, $reject$, else, move the head back to headmost.
          b. Scan the tape and mark the first unmarked $1$. If has no unmarked $1$, $reject$, else, move the head back to headmost.
       2. Move the head back to headmost and scan the tape. If has unmarked $1$s, $reject$, else, $accept$.
    3. On input string $w$:
       1. Repeat the following two steps:
          a. Scan the tape and mark first two unmarked $0$. If has no unmarked $0$, jump to step 2, else if has noly one unmarked $0$, $accept$, else, move the head back to headmost.
          b. Scan the tape and mark the first unmarked $1$. If has no unmarked $1$, $accept$, else, move the head back to headmost.
       2. Move the head back to headmost and scan the tape. If has unmarked $1$s, $accept$, else, $reject$.

