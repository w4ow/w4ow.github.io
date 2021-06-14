---
title: 常见排序算法总结与比较 
date: 2019-08-08
updated: 2019-08-08
mathjax: true
meta:
    date: false
categories: 
    - Computer Science
    - Algorithm
tags:
    - Sort
---

本文总结了目前常见的几种排序算法，包括冒泡、选择、插入、快排、堆排等。

---

<!-- more -->

## 1. 总体比较

**时间复杂度与稳定性**

类别|名称|平均复杂度|最好情况|最差情况|空间复杂度|稳定性
:--:|:--:|:--:|:--:|:--:|:--:|:--:
交换比较类|冒泡排序|$O(n^2)$|$O(n)$|$O(n^2)$|$O(1)$|稳定
交换比较类|快速排序|$O(nlogn)$|$O(n^2)$|$O(n^2)$|$O(logn)$|不稳定
选择类|选择排序|$O(n^2)$|$O(n^2)$|$O(n^2)$|$O(1)$|不稳定
选择类|堆排序|$O(nlogn)$|$O(nlogn)$|$O(nlogn)$|$O(1)$|不稳定
插入类|直接插入|$O(n^2)$|$O(n)$|$O(n^2)$|$O(1)$|稳定
插入类|二分排序|$O(n^2)$|$O(nlogn)$|$O(n^2)$|--|稳定
插入类|希尔排序|$O(nlogn)$|$O(n)$|$O(n^s)1$|$O(1)$|不稳定
其他|归并排序|$O(nlogn)$|$O(nlogn)$|$O(nlogn)$|$O(n)$|稳定
其他|基数排序|$O(log_R B)$|--|$O(log_R B)$|$O(n)$|稳定
其他|二叉排序|$O(nlogn)$|--|$O(nlogn)$|$O(n)$|稳定
其他|计数排序|$O(n+k)$|--|$O(n+k)$|$O(k)$|稳定
其他|拓扑排序|$O(n+e)$|--|--|$O(n)$|--
其他|枚举排序|$O(n^2)$|--|--|--|--

**算法特性**

名称|时间复杂度与初序|移动次数与初序|比较次数与初序|排序趟数与初序|每趟确定最终位置
:--:|:--:|:--:|:--:|:--:|:--:
冒泡排序|⭕️|⭕️|⭕️|⭕️|⭕️
快速排序|⭕️|⭕️|⭕️|⭕️|⭕️
选择排序|❌|❌|❌|❌|-
堆排序|❌|❌|❌|-|⭕️
直接插入|⭕️|⭕️|⭕️|❌|❌
二分排序|-|-|❌|-|-
希尔排序|⭕️|-|⭕️|-|❌
归并排序|❌|❌|⭕️|-|-
基数排序|❌|❌|❌|-|-
二叉排序|⭕️|-|-|-|-
计数排序|⭕️|-|-|-|-
拓扑排序|-|-|-|-|-
枚举排序|-|-|-|-|-

## 2. 冒泡排序

冒泡排序是一种典型的交换排序算法，相邻两数两两⽐比较并交换，直到所有数据排序完成，每一趟排序总能够将无序队列中的最⼤/小置于队尾。
**适用情况**: 待排序列列较小时性能较优。
**最坏情况**: 初始序列基本有序但顺序相反
**最好情况**: 初始序列列基本有序(没有插⼊快，因为要两两比较)
**应用**: 对基本有序序列查前n个元素

```c++
void BubbleSort(vector<int>& array) {
    if (array.size() <= 1)
        return;
    for (int i = 0; i < array.size(); i++)
        for (int j = 0; j < array.size() - i; j++)
            if (array[j] > array[j + 1])
                swap(array[j + 1], array[j]);
}
```

## 3. 选择排序

反复选择未排序序列中最大或最小元素，并将其放于已排序序列尾部，直到未排序序列长度为0

```c++
void SelectSort(vector<int>& array) {
    if (array.size() <= 1)
        return;
    for (int i = 0; i < array.size(); i++) {
        int minIndex = i;
        for (int j = i; j < array.size(); j++)
            if (array[j] < array[minIndex])
                minIndex = j;
        swap(array[i], array[minIndex]);
    }
}
```

## 4. 直接插入排序

将数据分为有序和无序两部分，首先有序部分为空，依次从无序部分中拿出元素插入到有序序列中
**可能出现情况**: 最后一趟开始前，所有元素都不在正确位置上
**最佳情况**: 基本有序下，插⼊排序是最快的

```c++
void InsertionSort(vector<int>& array) {
    if (array.size() <= 1)
        return;
    for (int i = 1; i < array.size(); i++) {
        int temp = array[i], j = i;
        while (j >= 0 && array[j - 1] > temp) {
            array[j] = array[j - 1];
            j--;
        }
        array[j] = temp;
    }
}
```

## 5. 归并排序

n越⼤大越好
**优点**: 当待排序列列较大，内存一次性放不不下时，需要外部排序，通常使用归并排序
**归并趟数**: m个元素k路归并趟数$s = log_k m$

```c++
void MergeSort(vector<int>& array) {
    int left = 0, right = (int)array.size() - 1;
    return MergeSort(array, left, right);
}

void Merge(vector<int>& array, int l1, int r1, int l2, int r2) {
    int i = l1, j = l2;
    vector<int> temp(array.size());
    int index = 0;
    while (i <= r1 && j <= r2) {
        if (array[i] <= array[j])
            temp[index++] = array[i++];
        else
            temp[index++] = array[j++];
    }
    while (i <= r1)
        temp[index++] = array[i++];
    while (j <= r2)
        temp[index++] = array[j++];
    for (int i = 0; i < index; i++)
        array[l1 + i] = temp[i];
}

void MergeSort(vector<int>& array, int left, int right) {
    if (left < right) {
        int mid = (left + right) / 2;
        MergeSort(array, left, mid);
        MergeSort(array, mid + 1, right);
        Merge(array, left, mid, mid + 1, right);
    }
}
```

## 6. 快速排序

分治法
**优点**: 平均最快，平均比较次数最少
**最好情况**: 每次能够均匀划分
**最坏情况**: 序列有序，或不均匀划分
**优化**: 快排需要使用递归，因此处理左右子段时，先处理短子段可以减少时间复杂度。先处理短⼦段的话，每次递归深度都是短⼦段的长度
**对排序对象的要求**: 待排序列的存储方式是顺序存储

```c++
void QuickSort(vector<int>& array) {
    int left = 0, right = (int)array.size() - 1;
    return QuickSort(array, left, right);
}

int Partition(vector<int>& array, int left, int right) {
    int temp = array[left];
    while (left < right) {
        while (left < right && array[right] > temp)
            right--;
        array[left] = array[right];
        while (left < right && array[left] < temp)
            left++;
        array[right] = array[left];
    }
    array[left] = temp;
    return left;
}

void QuickSort(vector<int>& array, int left, int right) {
    if (left < right) {
        int pos = Partition(array, left, right);
        QuickSort(array, left, pos - 1);
        QuickSort(array, pos + 1, right);
    }
}
```
