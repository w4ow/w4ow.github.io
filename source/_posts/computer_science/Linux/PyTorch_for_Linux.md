---
title: MacOS/Ubuntu 18.10 安装PyTorch
date: 2019-03-30
updated: 2018-03-30
meta:
    date: false
categories: 
    - Computer Science
    - Linux
tags:
    - PyTorch
    - MacOS
    - Ubuntu
---

MacOS/Ubuntu 18.10 安装PyTorch

<!-- more -->

---

## 1. MacOS/Ubuntu 18.10 安装Conda管理环境

MacOS下前往Anaconda官网下载安装即可
Linux下，从官网下载的是一个.sh文件，下载后终端执行zsh xxxx.sh并根据指示安装即可.
如果安装后使用zsh启动conda显示找不到conda，可以在.zshrc文件中添加*export PATH="/home/user_name/anaconda3/bin:$PATH"*并source一下即可

---

## 2. MacOS/Ubuntu 18中安装PyTorch

1. (可选) 为了避免各种软件的版本冲突问题，推荐使用python虚拟环境，在虚拟环境中安装pytorch

```shell
conda create -n environment_name python=X.X (2.7/3.6)   # 创建虚拟环境
conda info --env    # 显示所有的conda虚拟环境
conda activate environment_name # 激活虚拟环境
```

2. 修改安装镜像源并安装PyTorch，由于国外的conda源安装速度很慢，因此建议改为清华源

```shell
# for Anaconda
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/

# for PyTroch
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/

conda config --set show_channel_urls yes
```

3. 安装PyTorch

```shell
    conda install pytorch torchvision
    conda deactivate    # 使用结束后退出虚拟环境
```

---

## 3. 虚拟环境中安装jupyter

如果是新建的虚拟环境，即使你之前已经安装了anaconda，也有可能需要重新在虚拟环境中重新安装jupyter

```shell
python3 -m pip install jupyter
```
