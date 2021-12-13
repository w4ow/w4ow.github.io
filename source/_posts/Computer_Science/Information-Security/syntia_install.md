---
title: Install Syntia with Ubuntu 18.04 
date: 2019-09-09
updated: 2019-09-09
mathjax: true
meta:
    date: false
categories: 
    - Computer Science
    - Information Security
tags:
    - MBA
---

[Syntia](https://github.com/RUB-SysSec/syntia)是基于程序合成的反混淆框架，它使用程序跟踪作为一个blackbox来产生随机的输入输出对，利用这些输入输出对，程序合成器可以学习代码的底层语义。本文介绍了Ubuntu下安装syntia

---

<!-- more -->

## 1. 下载源码

```shell
git clone https://github.com/RUB-SysSec/syntia
```

## 2. 安装第三方Python库Syntia

syntia是一个第三方Python库，使用的是**Python2**。为了避免Python版本紊乱，可以使用conda创建一个Python2.7的虚拟环境，在该环境中安装syntia。在下载的源码中有setup.py文件，使用下面命令可以安装该Python库：

```shell
python setup.py install
```

## 3. 安装依赖软件

依赖软件包含在*install_deps.sh*脚本文件中，原则上可以通过执行该脚本文件来安装4个依赖，但是实际操作却总是报错。因此我使用手动安装

### 3.1 安装Unicorn

```python
# 使用git下载源码
git clone https://github.com/unicorn-engine/unicorn.git

cd unicorn

# compile
./make.sh

# install
sudo ./make.sh install

# 验证，命令执行后出现success，可认为是安装成功了
./samples/sample_all.sh
```

### 3.2 安装Capstone

```python
# 使用git下载源码
git clone https://github.com/aquynh/capstone.git

cd capstone

# compile
./make.sh

# install
sudo ./make.sh install

# 验证，我也不清楚验证结果是什么样代表正确安装
./tests/test*
```

### 3.3 安装Miasm

```python
# 1. 安装额外Python依赖库

# 使用git下载源码
git clone https://github.com/serpilliere/elfesteem.git elfesteem

cd elfesteem

python setup.py build
python setup.py install

# 2. 安装miasm

cd ..
# 使用git下载源码
git clone https://github.com/cea-sec/miasm.git

cd miasm

python setup.py build
python setup.py install

# 验证
# 可以通过在Python编程环境中import miasm来验证，若不报错则说明成功安装
```

### 3.4 安装Z3

```python
# 使用git下载源码
git clone https://github.com/Z3Prover/z3.git

cd z3

python scripts/mk_make.py --python

cd build

make
make install    # 该语句执行后会打印 Z3 was successfully installed.
```
