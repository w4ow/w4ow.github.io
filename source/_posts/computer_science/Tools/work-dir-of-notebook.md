---
title: 修改jupyter notebook默认工作路径
date: 2019-10-15 10:00
updated: 2019-10-15 22:00
meta:
    date: false
categories: 
    - Computer Science
    - Tools
tags:
    - Conda
---

修改jupyter notebook默认工作路径

---

<!-- more -->

**1. 生成配置文件**

```shell
# Mac & Ubuntu & Windows在Terminal中运行下面命令
jupyter notebook --generate-config
```
如果在Windows中出现*'jupyter'不是内部或外部命令，也不是可运行的批处理文件*，可以通过下面命令安装notebook

```shell
pip install notebook
```

**2. 修改配置文件**

找到如下所示的两行，第二行去注释并改成工作路径即可。

```shell
macOS:
## The directory to use for notebooks and kernels.
#c.NotebookApp.notebook_dir = ''

Ubuntu & Windows:
## The default URL to redirect to from `/`
#c.NotebookApp.default_url = '/tree'
```