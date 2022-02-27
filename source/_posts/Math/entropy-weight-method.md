---
title: 熵权法 
date: 2019-09-05
updated: 2019-09-05
mathjax: true
meta:
    date: false
categories: 
    - Mathematics
tags:
    - Mathematics
---

熵权法计算权值，Python3实现

---

<!-- more -->

## 1. 输入矩阵

假设计算矩阵为：
$$x = 
\begin{bmatrix}
1 & 2 & 2 \\\\
2 & 1 & 1
\end{bmatrix}
$$

## 2. 极差标准化

评价指标分为正指标和逆指标，计算公式如下

$$x_{ij}'=\frac{x_{ij} - \min_{j=1}^n x_{ij}}{\max_{j=1}^n x_{ij} - \min_{j=1}^n x_{ij}}$$

此处仅展示正指标计算公式。
将所有指标都视为正指标，标准化后的$x=[[0,1,1],[1,0,0]]$，可以使用scikit learn库中的MinMaxScaler()方法对数据进行最大最小归一化。

## 3. 计算指标信息熵

信息熵是一个对象所能提供的信息量大小的量化指标，其范围在$(0,1)$之间。

$e$应当为$1*n$大小的矩阵，式中$e_j$代表第$j$项指标的信息熵，信息熵的计算公式为：
$$e_j = (-\frac{1}{\ln m})\sum_{i=1}^m (p_{ij} \cdot \ln p_{ij})$$
其中$p_{ij}$为第$j$个指标的状态为$i$时的概率，即
$$p_{ij}= \frac{x_{ij}}{\sum_{i=1}^m x_{ij}}$$
因此
$$e_j = (-\frac{1}{\ln m})\sum_{i=1}^m (\frac{x_{ij}}{\sum_{i=1}^m x_{ij}} \cdot \ln(\frac{x_{ij}}{\sum_{i=1}^m x_{ij}}))$$
应用到上述矩阵，可以得到$e=[0.9183, 0.9183, 0.9183]$即

|指标名称|信息熵|
|:-:|:-:|
|指标1|0.9183|
|指标2|0.9183|
|指标3|0.9183|

## 4.计算指标权重

$w$应当为$1*n$大小的矩阵，式中$w_j$代表第$j$项指标的权重
$$w_j = \frac{1 - e_j}{\sum_{j=1}^n (1-e_j)}$$
应用到上述$e$，可以得到$w=[0.33, 0.33, 0.33]$，即

|指标名称|权重|
|:-:|:-:|
|指标1|0.33|
|指标2|0.33|
|指标3|0.33|

## 5. Coding

```python
# language = python3

import math
import numpy as np
import pandas as pd
from sklearn.preprocessing import MinMaxScaler

file = "test.csv"
data = pd.read_csv(file, header = None)

x = data.values.astype(np.float64)
x_std = MinMaxScaler().fit_transform(x)

def Entropy_Weight(data):
    m, n = np.shape(data)
    p = data / data.sum(axis = 0)
    p[np.where(data == 0)] = 0.01
    e = (-1.0 / np.log(m)) * np.nan_to_num(np.sum(p * np.log(p), axis = 0))
    w = (1 - e) / np.sum(1 - e)
    return w

w = Entropy_Weight(x_std)
```
