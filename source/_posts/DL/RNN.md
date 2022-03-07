---
title: 序列建模：循环神经网络
date: 2021-08-15 10:00
# updated: 0000-00-00 10:00
mathjax: true
meta:
    date: true
categories: 
    - Computer Science
    - Deep Learning
tags:
    - RNN
---

**循环神经网络**(recurrent neural network, RNN)是一类用于处理序列数据的神经网络。

---

<!-- more -->

# 1. 数学原理

## 1.1. 语言模型及$n$元语法

给定一个长度为$\tau$的词序列或字符序列$x^{(1)},x^{(2)},...,x^{(\tau)}$，其中每个字词$x^{(i)}$都是依次生成的，那么传统RNN的工作原理可以认为是：

$$
P(x^{(1)},...,x^{(\tau)}) = \prod_{t=1}^{\tau}P(x^{(\tau)}|x^{(1)},,...,x^{(\tau-1)})
$$

即生成该序列的条件概率。例如，包含4个字符的文本序列的概率

$$
P(x^{(1)},x^{(2)},x^{(3)},x^{(4)}) = P(x^{(1)})P(x^{(2)}|x^{(1)})P(x^{(3)}|x^{(1)},x^{(2)})P(x^{(4)}|x^{(1)},x^{(2)},x^{(3)})
$$

而$P(x^{(1)})$可以通过计算$x^{(1)}$在语料库中出现的频率得到，同理$P(x^{(2)}|x^{(1)})$可以通过计算语料库中$x^{(2)}$紧跟$x^{(1)}$之后的频率，以此类推。

当序列长度增加时，所需要计算的概率会以指数级增长，为了简化计算的复杂度，可以利用基于**马尔科夫假设**的$n$元语法。$n$阶马尔科夫假设(Markov chain of order $n$)是指一个字词的出现仅与其前面的$n$个词有关，例如，如果$n=1$，上面的语言模型则可以改成

$$
P(x^{(1)},x^{(2)},x^{(3)},x^{(4)}) = P(x^{(1)})P(x^{(2)}|x^{(1)})P(x^{(3)}|x^{(2)})P(x^{(4)}|x^{(3)})
$$

因此这种语言模型因此也被称为$n$元语法。

## 1.2. 循环神经网络

循环神经网络有三种主要的设计模式：

1. 每个时间步都有输出，并且隐藏单元之间有循环连接的循环网络，如下图所示：
   <img src="rnn1.png" width="90%" height="90%">

   假设$\bm{x}^{(t)}$为$t$时刻的输入向量，那么时间步$t$的隐藏状态则可以通过以下公式计算：

   $$
   \begin{aligned}
      \bm{h}^{(t)} &= \phi(\bm{Wh}^{(t-1)}+\bm{Ux}^{(t)}+\bm{b}) \\\\
      \bm{o}^{(t)} &= \bm{Vh}^{(t)}+\bm{c}
   \end{aligned} \tag{1}
   $$

   其中$\bm{U}$和$\bm{W}$分别对应输入-隐藏和隐藏-隐藏的连接，
   $\bm{V}$则为隐藏-输出连接，而$\bm{b,c}$则是对应的偏置向量。这个循环网络将输入序列映射到相同长度的输出序列，与$\bm{x}$配对的$\bm{y}$的总损失就是所有时间步的损失之和，即
   
   $$
   \begin{aligned}
      &L(\lbrace \bm{x}^{(1)},...,\bm{x}^{(\tau)}\rbrace, \lbrace \bm{y}^{(1)},...,\bm{y}^{(\tau)}\rbrace) \\\\
      &=\frac{1}{\tau}\sum_{t}l^{(t)} \\\\
      &=-\frac{1}{\tau}\sum_{t}\log p_{model}(y^{(t)}|\lbrace \bm{x}^{(1)},...,\bm{x}^{(\tau)}\rbrace)
   \end{aligned} \tag{2}
   $$

2. 每个时间步都有输出，当前时刻的输出与下时刻隐藏单元之间有连接。
   <img src="rnn2.png" width="90%" height="90%">

   这一类的RNN唯一的循环是输出到隐藏的反馈，其表示没有前一种结构强大，只能表示更小的函数集合。由于只有$\bm{o}$是唯一传播到未来的信息，这使得该种RNN**更容易训练**。因为基于比较时刻$t$的预测及其真实值的损失函数中的所有时间步都解耦了，每个时间步可以与其他时间步分离训练，允许并行化。

   由输出反馈到模型而产生循环连接的模型可以用**导师驱动过程**(teacher forcing)进行训练，与前一种不同，该过程不再使用最大似然准则，而是在$t+1$时刻接受真实值$y^{(t)}$作为输入。使用导师是驱动的最初动机是为了避免在缺乏隐藏-隐藏连接的模型中使用**通过时间反向传播**(BPTT)算法。

3. 隐藏单元之间有循环连接，但读取整个序列后产生单个输出的循环网络。
   <img src="rnn3.png" width="60%" height="60%">

## 1.3 通过时间的反向传播BPTT

简单起见，我们考虑无偏差项的循环神经网络，由前文公式(1)(2)有
$$L=\frac{1}{\tau}\sum_{t}l^{(t)} $$
首先考虑模型的输出层$\bm{o}^{(t)}$，其梯度容易计算：
$$\frac{\partial L}{\partial\bm{o}^{(t)}}=\frac{\partial l(\bm{o}^{(t)}, y^{(t)})}{\tau\cdot\partial\bm{o}^{(t)}}$$
更进一步的计算则需要根据$l(\bm{o}^{(t)}, y^{(t)})$的详细设计。

随后可以计算隐藏-输出的模型参数$\bm{V}$， $L$通过$\bm{o}^{(t)}$依赖$\bm{V}$，即$\bm{o}^{(t)} = \bm{Vh}^{(t)}$，根据链式法则，有：

$$\frac{\partial L}{\partial \bm{V}} = \sum_{t=1}^{\tau}(\frac{\partial L}{\partial\bm{o}^{(t)}}\cdot \frac{\partial \bm{o}^{(t)}}{\partial\bm{V}}) = \sum_{t=1}^{\tau}\frac{\partial L}{\partial\bm{o}^{(t)}}{\bm{h}^{(t)}}^{\top}
$$

在最终时间步$\tau$，$\bm{o}^{(\tau)}$只依赖于$\bm{h}^{(\tau)}$，因此最终时间步的隐藏状态$\bm{h}^{(\tau)}$的梯度为：

$$
\frac{\partial L}{\partial \bm{h}^{(\tau)}} = \frac{\partial L}{\partial\bm{o}^{(\tau)}}\cdot \frac{\partial \bm{o}^{(\tau)}}{\partial\bm{h}^{(\tau)}} = \bm{V}^{\top}\cdot\frac{\partial L}{\partial\bm{o}^{(\tau)}}
$$

随后我们从序列的末尾开始，从时刻$t=\tau$反向迭代到$t=1$，因为$\bm{h}^{(t)}$同时具有$\bm{h}^{(t+1)}$和$\bm{o}^{(t)}$两个后续结点，因此其梯度计算为：

$$
\frac{\partial L}{\partial \bm{h}^{(t)}} 
= \frac{\partial L}{\partial\bm{h}^{(t+1)}}\cdot \frac{\partial \bm{h}^{(t+1)}}{\partial\bm{h}^{(t)}} + \frac{\partial L}{\partial\bm{o}^{(t)}}\cdot \frac{\partial \bm{o}^{(t)}}{\partial\bm{h}^{(t)}} 
= \bm{W}^{\top}\cdot\frac{\partial L}{\partial\bm{h}^{(t+1)}} + \bm{V}^{\top}\cdot\frac{\partial L}{\partial\bm{o}^{(t)}}
$$

将上面的式子展开，对任意时间步$t$有：

$$
\frac{\partial L}{\partial \bm{h}^{(t)}} 
=\sum_{i=t}^{\tau}{(\bm{W}^{\top})}^{\tau -i}\bm{V}^{\top}\frac{\partial L}{\partial \bm{o}^{(\tau + t - i)}}
$$

由该式中的指数可以看出，当$\tau$较大或较小时，RNN十分容易出现梯度爆炸或者梯度消失的问题。
剩下的参数梯度如下：

$$
\begin{aligned}
    \frac{\partial L}{\partial \bm{W}} &= \sum_{t=1}^{\tau}\frac{\partial L}{\partial \bm{h}^{(t)}}{\bm{h}^{(t-1)}}^{\top} \\\\
    \frac{\partial L}{\partial \bm{U}} &= \sum_{t=1}^{\tau}\frac{\partial L}{\partial \bm{h}^{(t)}}{\bm{x}^{(t)}}^{\top}
\end{aligned}
$$


# 2. 双向RNN

到目前为止，我们所考虑的RNN是基于因果结构的，即时刻$t$的状态仅依赖于之前的序列$\bm{x}^{(1)},...,\bm{x}^{(t-1)}$以及当前输入$\bm{x}^{(t)}$。然而，在众多应用中，所要输出的$\bm{y}^{(t)}$可能依赖于整个输入序列，最典型的例子就是数学表达式求解(例如化简$x^2+y^2-2xy$)，其中的同一个变量可能出现在表达式中的不同位置。双向RNN为满足这种需求而生，该结构在手写识别、语音识别、生物信息学等领域十分成功。下图展示了典型的双向RNN结构。
<img src="birnn.png" width="40%" height="40%">

对于双向RNN，其隐藏状态包含两个，$\bm{h}$是包含过去信息的隐藏状态，而$\bm{g}$则是包含将来信息的隐藏状态，因此其输出以以下公式计算：

$$
\begin{aligned}
   \bm{h}^{(t)} &= \phi(\bm{\overrightarrow{W}h}^{(t-1)}+\bm{\overrightarrow{U}x}^{(t)}+\bm{\overrightarrow{b}}) \\\\
   \bm{g}^{(t)} &= \phi(\bm{\overleftarrow{W}g}^{(t-1)}+\bm{\overleftarrow{U}x}^{(t)}+\bm{\overleftarrow{b}}) \\\\
   \bm{o}^{(t)} &= \bm{V}[\bm{h}^{(t)}; \bm{g}^{(t)}]+\bm{c}
\end{aligned}
$$


# 3. 基于编解码的Seq2Seq架构

在以上展示的RNN都包含了一个隐藏约束，即输入序列的长度$n_x$和输出序列的长度$n_y$之间存在$n_x=n_y=\tau$，或者$\bm{y}^{(t)}$为一个固定大小的向量。然而在现实场景中，有许多任务涉及将不定长序列映射到另一个不定长序列，如语音识别、机器翻译以及问答系统。

通常，我们将RNN的输入称为**上下文**，我们希望产生此上下文的表示$C$，它是一个概括输入序列$\bm{X}=(\bm{x}^{(1)},...,\bm{x}^{(n_x)})$的向量或者向量序列。基于这种想法，[Cho等人](https://arxiv.org/pdf/1406.1078.pdf)提出了编解码架构，该架构由两组RNN组合而成，前者称为编码器，编码器读取输入并产生上下文$C$。后者称为解码器，解码器则以固定长度的向量为条件产生输出序列(即输出序列的长度是固定的)。这种架构最大的优势是$n_x$和$n_y$可以彼此不同，这就允许了不同长度序列之间的互相映射，尽管解码器的输出序列是固定的，但我们可以对其进行截取从而取得不同长度的输出序列。同样的，我们也可以对输入序列进行填充从而得到相同长度的输入序列，从而便于编码器的计算。

此类架构的一个明显不足是，编码器输出的上下文$C$的维度太小而难以适当地概括一个长序列，例如输入序列为一篇1000字的文章，很难用一个向量或向量组来概括。

# 4. 递归神经网络(Recursive NNs)

递归神经网络是RNN的另一个扩展，其构造为深的树状结构，而不是通常RNN的链状结构，如下图所示。
<img src="recursive_nn.png" width="40%" height="40%">

这类网络的潜在用途是**学习推论**，递归神经网络已成功的应用于输入是*数据结构*的神经网络，如[自然语言处理](https://www.aclweb.org/anthology/D13-1170.pdf)和计算机视觉。其一个明显优势是，对于具有相同长度$\tau$的序列，结构的深度可以从$\tau$减小为$\mathcal{O}(\log\tau)$，这可能有助于解决长期依赖。该网络的一个悬而未决的问题是**如何以最佳的方式构造树**，其中一个选择是使用平衡二叉树。处理自然语言句子时，用于递归网络的树结构可以被固定为句子语法分析树的结构。

