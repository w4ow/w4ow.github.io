---
title: Softmax回归：原理与实践
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

不同于线性回归输出连续值，有的任务需要输出离散值，例如对图像的分类。对于离散值预测问题，我们可以使用softmax回归模型进行建模。softmax回归实际上是逻辑回归的一般形式，逻辑回归用于二分类，而softmax回归用于多分类。

---

<!-- more -->

## 1. 理论模型

考虑一个花朵的分类问题，每个花朵样本$\bm{x}=[x_1,...,x_d]$有$d$个特征，例如花萼长度、宽度等等。花朵可以被分为$k$个类别。在现实生活中，我们通常使用离散值来表示类别，例如3种花我们可以将其分配标签1、2、3，但考虑到模型的输入，即**样本的特征通常都是连续值，如果输出是离散的，那么这种从连续值到离散值的转化通常都会影响到分类结果的质量**，因此我们引入了softmax回归。softmax的任务就是计算输入个例$\bm{x}$属于每一类的概率，其输出是一个大小为$1\times k$的向量$o\in\mathbb{R}^{1\times k}$，其中每个分量的值都介于0到1之间，且各分量之和为1。我们可以认为该个例属于概率最高的那一类，例如当某样本经模型模型预测的输出为$[0.25,0.5,0.1,0.15]$，我们可以判断该样本属于第二类。

因为softmax对于一个样本$\bm{x}$要计算其属于每一个类别的概率，因为样本包含$d$个特征、输出有$k$中类别，因此softmax回归模型包含$d\times k$个权重以及$k$个偏差，对于每个类别，其输出结果计算为：

$$
\begin{aligned}
& o_1 = x_1w_{11} + x_2w{21} + ... + x_dw_{d1} + b_1 \\\\
& o_2 = x_1w_{12} + x_2w{22} + ... + x_dw_{d2} + b_2 \\\\
& \cdots \\\\
& o_k = x_1w_{1k} + x_2w{2k} + ... + x_dw_{dk} + b_k
\end{aligned}
$$

一个简单的办法是直接用$o_i$中最大值对应的类作为输出结果，但这样做主要有两方面的问题：
1. 输出值$o_i$的范围不确定，直观上很难判断这些值的意义。
2. 真实标签是离散值，这些离散值与不确定范围的输出值之间的误差难以衡量。

因此我们对这些输出$o_i$进行softmax运算，使得其输出变为分布在0和1之间且和为1的概率分布：

$$
y_1, y_2, ..., y_k = \mathrm{softmax}(o_1, o_2, ..., o_k)
$$
其中
$$
y_i = \frac{e^{o_i}}{\sum_{j=1}^k e^{o_j}}
$$

从该式可以看出$\mathrm{sum}(\hat y_i=1)$且$0 \le \hat y_i \le 1$，因此输出是一个合法的概率分布。

综上，我们可以总结基于softmax回归的多分类模型的计算表达式：


### 1.1 单样本分类的矢量计算表达式

给定一个样本$\bm{x}=[x_1, x_2, ..., x_d]$，样本被分为$k$类中的某一类

模型参数为
$$
\bm{W}=\begin{bmatrix}
w_{11} & w_{21} & & w_{d1}\\\\
w_{12} & w_{22} & & w_{d2}\\\\
& & \ddots & \\\\
w_{1k} & w_{2k} & & w_{dk}
\end{bmatrix},
\bm{b}=\begin{bmatrix}b_1 & b_2 & \cdots & b_d\end{bmatrix}
$$

模型输出层的输出为

$$
\bm{o} = \begin{bmatrix}o_1, o_2, \cdots, o_k\end{bmatrix}
$$

随后我们队输出层的输出做softmax运算，得到分类的概率分布

$$
\bm{y} = \begin{bmatrix}y_1, y_2, \cdots, y_k\end{bmatrix}
$$

整个单样本过程计算为：
$$\begin{aligned}
\bm{o} &= \bm{xW}+\bm{b} \\\\
\bm{y} &= \mathrm{softmax}(\bm{o})
\end{aligned}$$

### 1.2 小批量多样本分类的矢量计算表达式

给定一个小批量输入$\bm{X} = [\bm{x}_1, \bm{x}_2, \cdots, \bm{x}_n]^\top \in \mathbb{R}^{n\times d}$，其批量大小为$n$，每个样本的特征数为$d$，输出类别数为$k$，$\bm{W}\in\mathbb{R}^{d\times k}, \bm{b}\in\mathbb{R}^{1\times k}$，整个计算过程为

$$\begin{aligned}
\bm{O} &= \bm{XW}+\bm{b} \\\\
\bm{Y} &= \mathrm{softmax}(\bm{O})
\end{aligned}$$

特别要注意的是这里参数矩阵的尺寸为$d\times k$


---

## 2. 模型求解

### 2.1 损失函数定义

不同于线性回归经常使用均方误差作为损失函数，在分类预测中，想要预测的结果正确，我们并不需要预测概率完全等于标签概率，即我们只需要确保输入样本的某个分类的预测概率大于其他分类的预测概率即可。例如对于两种预测结果$\bm{\hat y}=[0.1, 0.7, 0.2]$和$\bm{\hat y}=[0.3, 0.4, 0.3]$对于分类结果来说都是一样的，而这对均方误差来说差别就很大。为了缓解这种现象，我们一般使用交叉熵损失。
对于单样本$\bm{x}$，**假设其只有一个标签**，那么我们可以为其构造真实标签$\bm{y}\in\mathbb{R}^k$，并使$\bm{y}$中对应该样本分类的分量为1，其余分量为0。那么，对于单样本其损失函数为
$$
l = -\sum_{i=1}^kI(y_i)\log y_i
$$
其中$I(y_i)$为指示性函数，我们假设该样本的分类为$y_i$，即$\hat y_j=1$。那么当且仅当$y_i$的下标$i=j$时，$I(y_i)=1$,且此时上式等价于$l = -\log y_j$；当$i\not=j$时，$I(y_i)=0$。也就是说，交叉熵只关心对正确类别的预测概率，因为只要其值足够大，就可以确保分类结果正确。

同样的，对于多样本，假设样本数为$n$，那么总体的损失函数定义为
$$
L = \frac{1}{n}\sum_{i=1}^nl^{(i)} = -\frac{1}{n}\sum_{i=1}^n \sum_{j=1}^k I(y_j^{(i)})\log y_j^{(i)}
$$
同样地，如果每个样本只有一个标签且分类为$k$，那么$L$可以简化为$L = -\frac{1}{n}\sum_{i=1}^n\log y_k^{(i)}$。从另一个角度来看，最小化$L$等价于最大化$\exp(-nL)=\prod_{i=1}^n\hat y_k^{(i)}$，即最小化交叉熵等价于最大化数据集所有标签类别的联合概率分布。



### 2.2 损失函数的反向传播

考虑到softmax函数为
$$
y_i = \frac{e^{o_i}}{\sum_{j=1}^k e^{o_j}}
$$

应用链式法则，可以得到：
$$
    \frac{\partial l}{\partial o_i} = \frac{\partial l}{\partial y_j}\cdot\frac{\partial y_j}{\partial o_i}
$$
注意上式中各符号的下标，$y$的下标是$j$而不是$i$的原因是在softmax中，其分母$\sum_{j=1}^k\exp(o_j)$包含了所有的输出。因此，我们对$w_i$求导时，对于下标$j\not=i$时也要进行计算。

1. 对于$\frac{\partial l}{\partial y_j}$，我们有 
   $$\frac{\partial l}{\partial y_j}=-\sum_{j=1}^k \frac{I(y_j)}{y_j}$$
2. 对于$\frac{\partial y_j}{\partial o_i}$，分两种情况讨论：
   1. 当$i=j$时： 
   $$\begin{aligned}
   \frac{\partial y_i}{\partial o_i} &= \frac{\partial(\frac{e^{o_i}}{\sum_{j=1}^k e^{o_j}})}{\partial o_i} \\\\
   &= \frac{\sum_{j=1}^k e^{o_j} e^{o_i} - (e^{o_i})^2}{\sum_{j=1}^k(e^{o_j})^2} \\\\
   &= \left( \frac{e^{o_i}}{\sum_{j=1}^k e^{o_j}} \right) \left( 1 - \frac{e^{o_i}}{\sum_{j=1}^k e^{o_j}} \right) \\\\
   &= y_i (1 - y_i)
   \end{aligned}$$
   2. 当$i\not= j$时： 
   $$\frac{\partial y_j}{\partial o_i} = \frac{\partial(\frac{e^{o_j}}{\sum_{t=1}^k e^{o_t}})}{\partial o_i} = \frac{-e^{o_i}e^{o_j}}{(\sum_{t=1}^k e^{o_t})^2} = -y_iy_j$$


将1.2.相乘，有：
$$\begin{aligned}
\frac{\partial l}{\partial o_i} &=(-\sum_{j=1}^k \frac{I(y_j)}{y_j})(y_i (1-y_i) - y_iy_j) \\\\
&= -\sum_{j=1, i=j}^k\frac{I(y_j)}{y_j} y_i(1-y_i) + \sum_{j=1, i\not=j}^k \frac{I(y_j)}{y_j}y_iy_j \\\\
&= -\frac{I(y_i)}{y_i}y_i(1-y_i) + \sum_{j=1, i\not=j}^k \frac{I(y_j)}{y_j}y_iy_j \\\\
&= -I(y_i)(1-y_i) + \sum_{j=1,j\not=i}^kI(y_j)y_i \\\\
&= -I(y_i) + I(y_i)y_i + \sum_{j=1,j\not=i}^kI(y_j)y_i \\\\
&= -I(y_i) + y_i\sum_{j=1}^k I(y_j)
\end{aligned}$$

当样本只有一个分类时，上式中$I(y_j)$只有一个值为1，因此上式又等价为$y_i-I(y_i)$

当我们对各参数$w_i$求偏导时，有
$$\frac{\partial L}{\partial w_i} = \frac{\partial L}{\partial o_i}\cdot\frac{\partial o_i}{\partial w_i} = \frac{1}{n}\sum_{i=1}^n(y_i-I(y_i))\cdot \bm{x}_i$$

## 3. PyTorch实战

图像分类数据集FashionMNIST是一个10分类图片数据集，每一张图片的大小为28*28像素。由于数据集是用0~9折十个数字给图片分类，不利于人类阅读，因此我们为其分配文字标签。


```python
import torch
import torchvision
import torchvision.transforms as transforms
import matplotlib.pyplot as plt

mnist_train = torchvision.datasets.FashionMNIST(root='./Datasets/FashionMNIST', train=True, download=True, transform=transforms.ToTensor())
mnist_test = torchvision.datasets.FashionMNIST(root='./Datasets/FashionMNIST', train=False, download=True, transform=transforms.ToTensor())

def get_labels(labels):
    text_labels = ['t-shirt', 'trouser', 'pullover', 'dress', 'coat', 'sandal', 'shirt', 'sneaker', 'bag', 'ankle boot']
    if isinstance(labels, list):
        return [text_labels[i] for i in labels]
    else:
        return text_labels[labels]

def get_images(images, labels):
    if isinstance(images, list):
        _, figs = plt.subplots(1, len(images), figsize=(12, 12))
        for f, img, lbl in zip(figs, images, labels):
            f.imshow(img.view((28, 28)).numpy())
            f.set_title(lbl)
            f.axes.get_xaxis().set_visible(False)
            f.axes.get_yaxis().set_visible(False)
    else:
        plt.imshow(images)
        plt.title(labels)
        # plt.axes.get_xaxis().set_visible(False)
        # plt.axes.get_yaxis().set_visible(False)
    plt.axis('off')
    plt.show()

feature, label = mnist_train[0]
print(f'Image size: {feature.shape} \nImage class: {get_labels(label)}')
get_images(feature[0], get_labels(label))

# more samples
x, y = [], []
for i in range(10):
    x.append(mnist_train[i][0])
    y.append(mnist_train[i][1])
get_images(x, get_labels(y))
```

```shell
Image size: torch.Size([1, 28, 28]) 
Image class: ankle boot
```

<img src="softmax_reg_1.png" width="60%" height="60%">
<img src="softmax_reg_2.png" width="60%" height="60%">


```python
'''封装数据'''

batch_size = 256

train_iter = torch.utils.data.DataLoader(mnist_train, batch_size=batch_size, shuffle=True)
test_iter = torch.utils.data.DataLoader(mnist_test, batch_size=batch_size, shuffle=False)

'''定义训练函数'''

def train(model, train_iter, test_iter, loss, num_epochs, batch_size, params=None, lr=None, optimizer=None):
    for epoch in range(num_epochs):
        train_l_sum, train_acc_sum, n = 0, 0, 0
        for x, y_hat in train_iter:
            y = model(x)
            l = loss(y, y_hat).sum()

            if optimizer is not None:
                optimizer.zero_grad()
            elif params is not None and params[0].grad is not None:
                for param in params:
                    param.grad.data.zero_()
            l.backward()
            if optimizer is None:
                sgd(params, lr, batch_size)
            else:
                optimizer.step()

            train_l_sum += l.item()
            train_acc_sum += (y.argmax(dim=1) == y_hat).sum().item()
            n += y.shape[0]
        test_acc = evaluate_accuracy(test_iter, model)
        print(f'epoch {epoch + 1}, \ttrain loss: {train_l_sum / n:.4f}, \t train accuracy: {train_acc_sum / n:.4f}, \t test accuracuy: {test_acc:.4f}')

'''计算准确率'''

def accuracy(y_hat, y):
    return (y_hat.argmax(dim=1) == y).float().mean().item()

def evaluate_accuracy(data_iter, net):
    acc_sum, n = 0.0, 0
    for x, y_hat in data_iter:
        acc_sum += (net(x).argmax(dim=1) == y_hat).float().sum().item()
        n += y_hat.shape[0]
    return acc_sum / n
```

### 3.1 从零实现


```python
import numpy as np

'''定义模型参数'''

num_inputs = 28 * 28
num_outputs = 10

w = torch.tensor(np.random.normal(0, 0.01, (num_inputs, num_outputs)), dtype=torch.float)
b = torch.zeros(num_outputs, dtype=torch.float)

w.requires_grad_(requires_grad=True)
b.requires_grad_(requires_grad=True)

'''实现softmax'''

def softmax(x):
    x_exp = x.exp()
    partition = x_exp.sum(dim=1, keepdim=True)
    return x_exp / partition

'''定义模型'''

def net(x):
    return softmax(torch.mm(x.view((-1, num_inputs)), w) + b)

'''定义交叉熵损失函数'''

def cross_entropy(y_hat, y):
    return - torch.log(y_hat.gather(1, y.view(-1, 1)))

'''定义优化函数'''

def sgd(params, lr, batch_size):
    for param in params:
        param.data -= lr * param.grad / batch_size


'''训练'''

num_epochs = 5
lr = 0.1
device = torch.device('cuda')
train(net, train_iter, test_iter, cross_entropy, num_epochs, batch_size, [w, b], lr)
```

```shell
epoch 1, 	train loss: 0.7856, 	 train accuracy: 0.7476, 	 test accuracuy: 0.7952
epoch 2, 	train loss: 0.5710, 	 train accuracy: 0.8127, 	 test accuracuy: 0.8086
epoch 3, 	train loss: 0.5266, 	 train accuracy: 0.8246, 	 test accuracuy: 0.8214
epoch 4, 	train loss: 0.5014, 	 train accuracy: 0.8318, 	 test accuracuy: 0.8247
epoch 5, 	train loss: 0.4850, 	 train accuracy: 0.8367, 	 test accuracuy: 0.8290
```


```python
'''预测'''

x, y = iter(test_iter).next()

true_labels = get_labels(list(y.numpy()))
pred_labels = get_labels(list(net(x).argmax(dim=1).numpy()))
titles = [true + '\n' + pred for true, pred in zip(true_labels, pred_labels)]

get_images(list(torch.squeeze(x[10:19], dim=1)), titles[10:19])
```
    
<img src="softmax_reg_3s.png" width="60%" height="60%">
    


### 3.2 简洁实现


```python
import torch.nn as nn

device = torch.device('cuda')

class Linear(torch.nn.Module):
    def __init__(self, input_dim, output_dim):
        super().__init__()
        self.linear = nn.Linear(input_dim, output_dim)

    def forward(self, x):
        return self.linear(x.view(x.shape[0], -1))


input_dim = 28*28
output_dim = 10
model = Linear(input_dim, output_dim)
loss = nn.CrossEntropyLoss()
optimizer = torch.optim.SGD(model.parameters(), lr=.1)
num_epochs = 5

train(model, train_iter, test_iter, loss, num_epochs, batch_size, None, None, optimizer)
```

```shell
epoch 1, 	train loss: 0.0031, 	 train accuracy: 0.7475, 	 test accuracuy: 0.7869
epoch 2, 	train loss: 0.0022, 	 train accuracy: 0.8133, 	 test accuracuy: 0.7666
epoch 3, 	train loss: 0.0021, 	 train accuracy: 0.8261, 	 test accuracuy: 0.8168
epoch 4, 	train loss: 0.0020, 	 train accuracy: 0.8328, 	 test accuracuy: 0.8224
epoch 5, 	train loss: 0.0019, 	 train accuracy: 0.8368, 	 test accuracuy: 0.8237
```

