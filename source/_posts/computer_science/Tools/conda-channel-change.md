---
title: Conda更改镜像源
date: 2019-11-07 10:00
updated: 2019-11-07 10:00
meta:
    date: false
categories: 
    - Computer Science
    - Tools
tags:
    - Conda
---

修改Conda默认的镜像源为清华镜像

---

<!-- more -->

## 1. 修改为Tsinghua源

```shell
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free

conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main

conda config --set show_channel_urls yes
```

## 2. 换回默认源

```shell
conda config --remove-key channels
```