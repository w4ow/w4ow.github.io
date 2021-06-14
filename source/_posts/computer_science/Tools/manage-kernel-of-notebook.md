---
title: Jupyter Notebook中添加、删除Python kernel的方法
date: 2019-10-20 19:00
updated: 2019-10-20 19:00
meta:
    date: false
categories: 
    - Computer Science
    - Tools
tags:
    - Conda
---

在通过Anaconda使用Jupyter时，有时我们创建了多个Python环境(包括虚拟环境)，但是启动Jupyter Notebook后，在*新建*notebook选项中却找不到我们所创建的Python环境，此时我们可以通过向notebook中添加Python kernel解决该问题。

---

<!-- more -->
## 1. 添加Kernel

### 1.1 切换环境，安装内核

首先切换到想要使用的Python环境中，若是虚拟环境则激活该虚拟环境。随后可以使用如下命令查看该环境下搜索并安装Python内核：

```shell
conda search ipykernel      # 搜索内核

conda install ipykernel     # 安装内核
```

### 1.2 在notebook中安装内核

使用以下命令安装notebook内核，

```shell
python -m ipykernel install --name name_you_want (--user)
```

其中name_you_want是将在notebook的*新建*中所显示的Python环境名字，安装时若权限不够，则添加--user

### 1.3 重新启动Jupyter Notebook

重启Notebook，可以在*新建*下拉菜单中看到name_you_want选项，大功告成。

## 2. 查看kernel

使用该命令可以查看notebook所有已安装kernel
```shell
jupyter kernelspec list
```

## 3. 删除kernel

使用以下命令可以删除指定kernel
```shell
jupyter kernelspec remove kernel_name
```
