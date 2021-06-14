---
title: Introduction to Queue
date: 2019-03-04
updated: 2019-03-04
mathjax: true
meta:
    date: false
categories: 
    - Computer Science
    - Data Structure
tags:
    - Data Structure
    - Queue
---

A note to queue

---

<!-- more -->

## 1. 循环队列

1. 计算大小为n的循环队列中元素的个数

<center>$(rear - front + n) % n$</center> 
<center>$(rear - front + n + 1) % n$</center>

2. 循环队列判空

<center>$front = rear$</center>

3. 循环队列判满

<center>$(rear + 1) % n = front $</center>

4. 出队后头指针front的值

<center>$ front = (front + 1) % m $</center>
