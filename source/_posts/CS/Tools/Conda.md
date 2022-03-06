---
title: Conda的使用
date: 2019-11-07 10:00
# updated: 2019-11-07 10:00
meta:
    date: true
categories: 
    - Computer Science
    - Tools
tags:
    - Conda
---

Conda是一个开源跨平台语言无关的包管理与环境管理系统，我们常用的Anaconda、Bioconda(计算生物学)、miniconda等都是基于conda的发行版。

---

<!-- more -->

# 1. Conda相关

## 1.1 Anaconda的安装以及配置

1. 安装Anaconda

   可以在Anaconda官网下载相应Python版本的软件，或者使用如下命令：

   ```shell
   wget https://repo.continuum.io/archive/Anaconda3-2018.12-Linux-x86_64.sh

   bash Anaconda3-2018.12-Linux-x86_64.sh
   ```

2. 创建Python虚拟环境

   ```shell
   conda create -n virtual_env_name python=x.x
   ```

3. 激活/关闭虚拟环境

   ```shell
   # 激活
   source activate virtual_env_name
   # 关闭
   source deactivate
   ```

4. 为相应的虚拟环境安装包

   ```python
   conda install -n virtual_env_name [package]
   ```

5. 删除虚拟环境

   ```python
   conda env remove -n(--name) virtual_env_name
   ```

6. 常用conda命令

   ```python
   conda env list  # 列出所有已创建的虚拟环境
   conda upgrade conda # 升级conda本身
   ```


## 1.2 Conda镜像源的修改

由于国外镜像访问速度较慢，推荐使用国内镜像。国内经Anaconda Inc.授权的为清华镜像。

1. 修改为Tsinghua源

   ```shell
   conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free

   conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main

   conda config --set show_channel_urls yes
   ```

1. 换回默认源

   ```shell
   conda config --remove-key channels
   ```

# 2. Jupyter Notebook的配置

通过`pip install notebook`或者`conda install notebook`可以安装交互式计算环境notebook。

## 2.1 Notebook工作路径的修改

Jupyter Notebook的默认工作路径为用户根目录，可以修改为指定目录。

1. 生成配置文件

   ```shell
   # Mac & Ubuntu & Windows在Terminal中运行下面命令
   jupyter notebook --generate-config
   ```
   如果在Windows中出现*'jupyter'不是内部或外部命令，也不是可运行的批处理文件*，则说明没有安装notebook，可以通过下面命令安装notebook

   ```shell
   pip install notebook
   ```

2. 修改配置文件

   找到如下所示的两行，第二行去注释并改成工作路径即可。

   ```shell
   macOS:
   ## The directory to use for notebooks and kernels.
   #c.NotebookApp.notebook_dir = ''

   Ubuntu & Windows:
   ## The default URL to redirect to from `/`
   #c.NotebookApp.default_url = '/tree'
   ```

## 2.2 Jupyter的Python内核管理

Conda可以创建多个Python虚拟环境，有时启动Jupyter Notebook后，在*新建*notebook选项中却找不到我们所创建的Python环境，此时可以通过向notebook中添加Python kernel解决该问题。

### 1. 添加Kernel

1. 切换环境，安装内核

   首先切换到想要使用的Python环境中，若是虚拟环境则激活该虚拟环境。随后可以使用如下命令查看该环境下搜索并安装Python内核：

   ```shell
   conda search ipykernel      # 搜索内核
   conda install ipykernel     # 安装内核
   ```

2. 在notebook中安装内核

   使用以下命令安装notebook内核，

   ```shell
   python -m ipykernel install --name name_you_want (--user)
   ```

   其中name_you_want是将在notebook的*新建*中所显示的Python环境名字，安装时若权限不够，则添加--user

3. 重新启动Jupyter Notebook

   重启Notebook，可以在*新建*下拉菜单中看到name_you_want选项，大功告成。

### 2. 查看kernel

   使用该命令可以查看notebook所有已安装kernel
   ```shell
   jupyter kernelspec list
   ```

### 3. 删除kernel

   使用以下命令可以删除指定kernel
   ```shell
   jupyter kernelspec remove kernel_name
   ```
