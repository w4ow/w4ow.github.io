---
title: Ubuntu下GDB的使用
date: 2019-07-27 22:00
updated: 2019-07-27 22:00
meta:
    date: false
categories: 
    - Computer Science
    - Linux
tags:
    - GDB
---

最近项目中经常需要使用GDB，因此总结了一下在使用GDB时遇到的技巧

---

<!-- more -->

## 1. 使用GDB调试可执行文件

```bash
gdb a.out               # 不带参数
或
gdb --args a.out 参数   # 带参数
```

## 2. GDB常用命令

```bash
r [args]                # 带参数从头开始执行程序
b arg                   # 在arg处设置断点，arg可以为行数、地址
b xxx if (condition)    # 条件断点
b test.c:30 if n==100   # 当变量n等于100的时候在test.c的30行处加断点
n (next)                # 单步执行，如遇函数则直接返回函数的执行结果
s (step)                # 单步执行，如遇函数则进入函数体执行
stop                    # 停止执行
q (quit)                # 退出GDB
until                   # 当不想反复执行循环时，可以用until跳出循环
finish                  # 跳出当前函数
c (continue)            # 继续执行，直到下一个断点或程序结束
p arg                   # 打印出arg的值，arg可以为变量或地址
whatis variable         # 打印出变量的类型
ptype variable          # 打印出变量的类型，只不过比whatis更详细
i variables variable    # 打印出定义variable的文件
i source                # 查看当前程序
i b                     # 查看arg，arg可以为断点、变量值、
i args                  # 打印出当前函数的参数值
i locals                # 打印出当前函数中所有局部变量值
i r (r_name)            # 查看所有寄存器(寄存器r_name)的值
i threads               # 查看线程
d b                     # 删除断点，b为断点编号
bt                      # 查看函数的back trace
l (list) arg            # 显示arg附近的前后共10行代码，arg可以为行数或函数名
watch arg               # 设置监控，在arg改变时停止(需要先加断点)
```

## 3. 设置断点

```bash
# 当欲设断点在当前文件中时，可以b+行数或b+函数名
b 45/main
# 当欲设断点在其他文件中时，b 文件名:行数
b head.cpp:45
```
