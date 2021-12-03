---
title: 线性回归：原理与实践
# date: 2021-06-15 10:00
# updated: 0000-00-00 10:00
mathjax: true
meta:
    date: true
categories: 
    - Computer Science
    - Deep Learning
tags:
    - ML
---

线性回归的输出是一个连续值，因此适用于回归问题，例如房价预测、气温预测、股票预测等等，但是作为一个单层神经网络，其预测能力十分有限。本文简要的介绍其原理以及PyTorch实现。

---

<!-- more -->


## 1. 理论模型

给定一个随机样本$(\hat y_i, x_{i1}, x_{i2},...,x_{ip})$，一个线性回归模型假设模型的输入(又称回归量)为$(x_{i1}, x_{i2},...,x_{ip})$，模型的输出(又称回归子)为$\hat y_i$。同时我们加入一个误差项$\varepsilon_i$来捕获除了输入自变量之外的任何对输出的影响。

$$
y_i = b+w_1x_{i1}+w_2x_{i2}+...+w_px_{ip}+\varepsilon_i
$$

当模型只有一个输入自变量，即$p=1$时，模型被称为一元线性回归，反之则为多元线性回归。形式上地，我们可以定义线性回归模型为：

$$
\bm{y}=\bm{Xw}+b
$$

---

## 2. 模型求解

### 2.1 最小二乘法

我们以一元线性回归为例，简单介绍最小二乘法求解模型。
给定了若干个实际样本$(\hat y_i, x_i),i=1,2,...,n$，我们需要找出一元线性回归模型$y=b+wx$中的$w$和$b$，使得该模型能够很好地拟合这一组数据。
最小二乘法的标准是，所有样本到模型方程的**平方误差**最小。在最小二乘法中，单个样本的平方误差为：

$$
l=\frac{1}{2}(\hat y_i-y_i)^2
$$

其中，**添加系数**$\frac{1}{2}$**是为了后面对其求导时能够消除系数**。总样本平均误差为：

$$
L = \frac{1}{2n}\sum_{i=1}^n(\hat y_i-y_i)^2 = \frac{1}{2n}\sum_{i=1}^n(\hat y_i - (b+wx_i))^2
$$

实际上，当我们将$w$和$b$当做求解目标时，那么该方程是关于$w$和$b$的凸函数。凸函数的重要特点就是当其导数为0时取得最小值。因此我们可以对$w$和$b$分别求偏导并令其为0：

$$
\begin{aligned}
    \frac{\partial L}{\partial b} &= -\sum_{i=1}^n(\hat y_i-b-wx_i)=\sum_{i=1}^n(\hat y_i-b-wx_i)=0 \\\\
    \frac{\partial L}{\partial w} &= -\sum_{i=1}^n(\hat y_i-b-wx_i)x_i=\sum_{i=1}^n(\hat y_i-b-wx_i)x_i=0
\end{aligned}
$$

因为$x_i, \hat y_i$都是已知的，带入上式即可求得$w,b$。该方法被称为**最小二乘法，二乘即平方的意思**。

同样的，多元线性模型的与上面一致，即求得一组参数$w_i$使得以下方程最小化：

$$
L=\frac{1}{2n}\sum_{i=1}^n(\hat y_i-y_i)^2=\frac{1}{2n}\sum_{i=1}^n(\hat y_i - (b+w_1x_{i1}+w_2x_{i2}+...+w_px_{ip}))^2
$$

对各参数$w_i$的求解同样使用最小二乘法，先求其偏导，再带入实际值$x_i,y_i$进行求解，不再赘述。

### 2.2 梯度下降

在使用偏导求解$w_i$的过程中，小批量随机梯度下降法(Mini-Batch Stochastic Gradient Descent, MBSGD)目前被广泛应用，其算法原理是，首先对$w_i$取随机初始值，然后对$w_i$进行多次迭代，每次迭代都可能降低损失函数$L$的值，由于$L$是凸函数，因此该方法肯定会在$w_i$取到某些值时达到最小值(或极小值)。在每次迭代中，MBSGD会从所有的样本中随机抽取若干个样本构成一个小批量$\mathcal{B}$，并基于这些小批量样本更新$w_i$:

$$
w_i \gets w_i - \frac{\eta}{|\mathcal{B}|}\frac{\partial L}{\partial w_i}
$$

其中$\eta$为每次更新$w_i$的**步长**，也称为**学习率**。

---

## 3. PyTorch实战

### 3.1 数据集生成以及封装              

假设房价是关于房屋面积$x_1$和房龄$x_2$的二元回归，我们可以令其为$y=2.3x_1+3.4x_2+1.1$，即$b=1.1, \bm{w}=[2.3, 3.4]$。现在我们依照该函数构造一个数据集


```python
import torch
from matplotlib import pyplot as plt
import numpy as np
import random
import utils

from torch.utils.data import Dataset, TensorDataset, DataLoader

# 特征数(即自变量输入)个数为2：房屋面积以及房龄
num_features = 2
num_samples = 1000
data = torch.randn(num_samples, num_features, dtype=torch.float)
true_w = [2.3, 3.4]
true_b = 1.1

labels = true_w[0] * data[:, 0] + true_w[1] * data[:, 1] + true_b
# 添加噪声(误差)
labels += torch.tensor(np.random.normal(0, 0.01, size=labels.shape), dtype=torch.float)

plt.scatter(data[:, 0].numpy(), labels.numpy(), s=1)
plt.scatter(data[:, 1].numpy(), labels.numpy(), s=1)
plt.ylabel('House price')
plt.legend(labels=['$x_1$', '$x_2$'])
plt.show()
```
<img src="linear_regression_1.png" width="60%" height="60%">
    


封装数据集，封装后的数据变为Torch.Tensor数据类型，可以被Torch识别处理。


```python
dataset = TensorDataset(data, labels)
data_iter = DataLoader(dataset, batch_size=32, shuffle=True)
```

### 3.2 初始化参数模型

将$w$初始化成均值为$0$、标准差为$0.01$的正态随机数矩阵$\mathbb{R}^{2\times 1}$，将$b$初始化为$1$。在之后的模型训练中，因为要对这些参数求梯度，因此要将它们的`requires_grad=True`


```python
w = torch.tensor(np.random.normal(0, 0.01, (num_features, 1)), dtype=torch.float)
b = torch.zeros(1, dtype=torch.float)
w.requires_grad_(requires_grad=True)
b.requires_grad_(requires_grad=True)
print(w.shape, b.shape)
```

```shell
Output:
torch.Size([2, 1]) torch.Size([1])
```


### 3.3 定义模型

首先定义我们的回归模型：$$y=\bm{wX}+b$$
其平方损失函数为： $$L = \frac{1}{2n}\sum_{i=1}^{n}(\hat y_i-y_i)^2$$
模型可学习参数$\bm{w}$和$b$的更新过程为：
$$\begin{aligned}
w_i &\gets w_i - \frac{\eta}{|\mathcal{B}|}\frac{\partial L}{\partial w_i}\\\\
b &\gets b - \frac{\eta}{|\mathcal{B}|}\frac{\partial L}{\partial b}
\end{aligned}$$


```python
def linear(X, w, b):
    return torch.mm(X, w) + b

def squared_loss(y_hat, y):
    return (y_hat - y.view(y_hat.size())) ** 2 / 2

def sgd(params, lr, batch_size):
    for param in params:
        param.data -= lr * param.grad / batch_size
```

### 3.4 模型训练

在训练中，我们多提从数据集中读取小批次样本，通过调用PyTorch中的反向函数`backward`计算小批量样本的随机梯度，并调用优化算法SGD来迭代可学习参数$\bm{w}$和$b$


```python
lr = 0.03
num_epochs = 5
model = linear
loss = squared_loss

for epoch in range(num_epochs):
    for x, y in data_iter:
        l = loss(model(x, w, b), y).sum()
        l.backward()
        utils.sgd([w, b], lr, batch_size=32)

        w.grad.data.zero_()
        b.grad.data.zero_()
    train_l = loss(model(data, w, b), labels)
    print(f'epoch {epoch + 1}, loss: {train_l.mean().item():.4f}')

print()
print(f'True paras: \t w:{true_w} \t\t\t b:{true_b}')
print(f'Train result: \t w:{w.detach().view(1, 2).numpy()[0]}, \t b: {b.detach().numpy()}')
```

```shell
Output:
epoch 1, loss: 0.0000
epoch 2, loss: 0.0000
epoch 3, loss: 0.0000
epoch 4, loss: 0.0000
epoch 5, loss: 0.0000

True paras: 	 w:[2.3, 3.4] 			     b:1.1
Train result: 	 w:[2.2997692 3.4001975], 	 b: [1.0994117]
```


可以看到，模型通过5次迭代后的结果就已经十分接近真实值了。

### 3.5 模型的简洁实现

PyTorch是一个完成度十分高的深度学习框架，其中已经包含了许多定义好的模型，因此我们可以通过调用直接构造第3.3节中的模型


```python
import torch.nn as nn

# 定义模型
class Linear(nn.Module):
    def __init__(self, input_dim, output_dim=1):
        super().__init__()
        self.linear = nn.Linear(input_dim, output_dim)

    def forward(self, x):
        out = self.linear(x)
        return out

model = Linear(num_features)
```

PyTorch实例化的模型参数是随机的，个人认为是不需要再次随机化的。

同样的，PyTorch也提供了损失函数和优化算法，我们可以方便地直接调用。


```python
loss = nn.MSELoss()
optimizer = torch.optim.SGD(model.parameters(), lr=0.03)

num_epochs = 5
for epoch in range(num_epochs):
    for x, y in data_iter:
        out = model(x)
        l = loss(out, y.view(-1, 1))
        optimizer.zero_grad()
        l.backward()
        optimizer.step()
    print(f'epoch {epoch + 1}, loss: {l.item():.4f}')

print()
print(f'True paras: \t w:{true_w} \t\t\t b:{true_b}')
print(f'Train result: \t w:{w.detach().view(1, 2).numpy()[0]}, \t b: {b.detach().numpy()}')
```

```shell
Ouput:
epoch 1, loss: 0.0001
epoch 2, loss: 0.0002
epoch 3, loss: 0.0002
epoch 4, loss: 0.0001
epoch 5, loss: 0.0001

True paras: 	 w:[2.3, 3.4] 			     b:1.1
Train result: 	 w:[2.2997692 3.4001975], 	 b: [1.0994117]
```
