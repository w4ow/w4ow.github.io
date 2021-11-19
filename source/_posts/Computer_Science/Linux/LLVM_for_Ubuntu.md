---
title: Ubuntu 18.10安装LLVM
date: 2018-12-30
updated: 2018-12-30
meta:
    date: false
categories: 
    - Computer Science
    - Linux
tags:
    - LLVM
    - Ubuntu
---

因为课题需要使用LLVM，因此在Ubuntu上安装了LLVM 8.0 Debug版本

---

<!-- more -->

## 1.安装cmake

安装LLVM需要使用cmake，可以使用如下命令安装:

```shell
sudo apt install cmake
```

## 2.修改swap分区(可选，不推荐)

从源码安装LLVM Debug版本时，链接过程会占用大量内存，因此可以尝试使用修改sawp分区加快安装进程。步骤如下：

1. 查看系统中已有的交换空间,如果没有条目或swap为0则说明没有可用交换空间:

```shell
sudo swapon --show
or
free -h
```

2. 检查磁盘使用情况,一般/dev下的设备是我们的磁盘，swap分区应小于此值

```shell
df -h
```

3. 在根目录(/)下创建名为swapfile的swap文件， 一般我们使用fallocate命令， 建议将swap分区设为20G以满足安装LLVM需求。

```shell
sudo swapoff -a     # 先关闭所有的swap分区，否则可能因为系统中存在swap分区而报错fallocate: fallocate failed: Text file busy，
sudo fallocate -l 20G /swapfile     # 创建swapfile
ls -lh /swapfile    # 验证是否成功创建swapfile，
```

　　如果显示*-rw------1 root root 20G 日期 /swapfile* 则表示创建成功

4. 启用交换文件

```shell
sudo chmod 600 /swapfile    # 锁定swapfile权限
sudo mkswap /swapfile       # 将文件标记为交换空间
```

　　若显示:*Setting up swapspace......* 则表示成功标记swapfile，标记之后启用该文件

```shell
sudo swapon /swapfile   # 启用
free -h                 # 验证
```

5. 永久保留swap文件：虽然我们更改了swap分区，但是重启后不会保留设置，因此可以将将swap文件添加到/etc/fstab来将其永久保留

```shell
sudo cp /etc/fstab /etc/fstab.bak   # 备份
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab  # 将swap文件信息添加到/etc/fstab文件
```

---

## 3.安装LLVM-8.0 Debug版本

1. 首先去[LLVM官网](http://releases.llvm.org/download.html#8.0.0)下载必要的软件包;
2. 获取源码:

```shell
# 在适当位置放置LLVM源码，此处放在home下
cd ~
mkdir tmp

# 解压LLVM源码
cd tmp
tar -Jxvg llvm-7.0.0.src.tar.xz
mv llvm-7.0.0.src llvm

# 解压clang源码(此时在tmp目录下)
cd llvm/tools
tar -Jxvg cfe-7.0.0.src.tar.xz
mv cfe-7.0.0.src clang

# 解压clang-tools-extra源码(此时在tmp/llvm/tools目录下)
cd clang/tools
tar -Jxvg clang-tools-extra-7.0.0.src.tar.xz
mv clang-tools-extra-7.0.0.src extra

# 解压compiler-rt源码(此时在tmp/llvm/tools/clang/tools目录下)
cd ~/tmp/llvm/projects
tar -Jxvg compiler-rt-7.0.0.src.tar.xz
mv compiler-rt-7.0.0.src compiler-rt
```

3. 开始安装

```shell
# 在tmp目录下新建build文件夹
cd build
cmake -G "Unix Makefiles" -DCMAKE_BUILD_TYPE=Debug ../llvm
make -j2 # 使用两个CPU核安装，该步骤既慢且卡，推荐设置为核心数一半
sudo make install

# 将路径添加到环境变量中
cd ~
vim .bashrc
export PATH=/usr/lib/llvm-7/bin
export LD_LIBRARY_PATH=/usr/lib/llvm-7/lib
source .bashrc
```

4. 重启终端并测试

```shell
clang -v
clang++ -v
clang test.c
```
