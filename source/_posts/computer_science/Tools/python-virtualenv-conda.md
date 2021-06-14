---
title: 使用conda管理Python虚拟环境 
date: 2019-09-09
updated: 2019-09-09
mathjax: true
meta:
    date: false
categories: 
    - Computer Science
    - Tools
tags:
    - Conda
---

使用conda管理Python虚拟环境

---

<!-- more -->

## 1. 安装Anaconda

可以在Anaconda官网下载相应Python版本的软件，或者使用如下命令：

```shell
wget https://repo.continuum.io/archive/Anaconda3-2018.12-Linux-x86_64.sh

bash Anaconda3-2018.12-Linux-x86_64.sh
```

## 2. 创建Python虚拟环境

```shell
conda create -n virtual_env_name python=x.x
```

## 3. 激活/关闭虚拟环境

```python
# 激活
source activate virtual_env_name
# 关闭
source deactivate
```

## 4. 为相应的虚拟环境安装包

```python
conda install -n virtual_env_name [package]
```

## 5. 删除虚拟环境

```python
conda env remove -n(--name) virtual_env_name
```

## 6. 常用conda命令

```python
conda env list  # 列出所有已创建的虚拟环境
conda upgrade conda # 升级
```
