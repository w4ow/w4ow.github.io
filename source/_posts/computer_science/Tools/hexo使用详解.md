---
title: HEXO的使用
# date: 2021-06-13 10:00
# updated: 0000-00-00 00:00
meta:
    date: true
categories: 
    - Computer Science
    - Tools
tags:
    - HEXO
---

[HEXO](https://hexo.io/zh-cn/)是一个基于Markdown的静态网页博客生成框架

---

<!-- more -->

# 1. 基础设置

## 1.1 HEXO目录文件简介

.
├── _config.yml # 全局配置文件
├── package.json
├── source
│　　├── ...
│　　└── _posts # 博客文件(md)放于此处
└── themes # 主题
　　├── landscape
　　└── material-x
　　　　├── ...
　　　　├── _config.yml # 主题配置文件
　　　　└── source
　　　　　　├── ...
　　　　　　└── less
　　　　　　　　├── _layout.less # 主题布局配置文件
　　　　　　　　└── ...

## 1.2 HEXO博客发布流程以及常用命令

在写好一篇博客(md)后:
1. 首先通过`hexo g(enerate)`命令由md文件生成静态网页；
2. 随后应预览一下，使用`hexo s(erver)`命令可以启动本地服务器,
随后在浏览器中访问网址[http://localhost:4000/](http://localhost:4000/)即可实时预览博客；
3. 最后，通过`hexo d(eploy)`命令可以将完成的博客部署到远程服务器(github)。

# 2. 设备迁移

## 2.1 迁移到新设备

如果仅是迁移到新设备，其操作还是相对比较简单的

### 2.1.1 拷贝原数据

通过U盘等将原设备上的以下博客文件(夹)拷贝至新设备。

.
├── _config.yml
├── package.json
├── scaffolds
├── source
└── themes

### 2.1.2 配置新设备

1. 确保新设备上正确安装nodejs以及npm
    ```shell
    sudo apt install nodejs #(不推荐，推荐官网下载编译版本)
    sudo apt install npm
    ```
2. 全局安装hexo
    ```shell
    sudo npm install -g hexl (--force) # 正常安装报错时可尝试使用--force命令
    ```
3. 进入博客根目录配置hexo
    ```shell
    npm install
    npm install hexo-deployer-git --save
    npm install hexo-generator-feed --save
    npm install hexo-generator-sitemap --save
    ```
4. 检查：通过本地服务器http://localhost:4000 检查能否正确生成博客
    ```shell
    hexo g
    hexo s
    ```

## 2.2 多设备同步

使用多台设备同步更新blog，其原理与使用git推拉repo相同。假设原博客保存在设备A，通过以下一个步骤将其同步到新设备B。

1. 打开设备A上的博客根目录下的**_config.yml**，添加如下内容：
    ```shell
    deploy:
        type: git
        repository: git@github.com:youname/youname.github.io.git
        branch: master
    ```
2. 首先将完整的博客部署到GitHub上，随后在根目录下创建博客的git，该步骤需要注意一点：有的theme包含**.git**隐藏目录，需要将其删除。
    ```shell
    # 部署博客
    hexo clean && hexo g && hexo d

    # 初始化repo
    git init
    # 创建新的分支
    git checkout -b your_branch
    git add .
    git commit -m "your_message"
    git push origin your_branch
    ```
    即master分支用于部署博客，your_branch分支用于提交源文件。
3. 在新设备B上将源文件克隆到本地
    ```shell
    git clone -b your_branch https://github.com/usrname/usrname.github.io.git your_blog
    cd your blog
    # 安装依赖
    npm install
    ```
    如安装依赖报错可参考2.1节迁移新设备
4. 测试。首先在设备B上编辑博客，并尝试部署提交
    ```shell
    hexo clean & hexo g & hexo d
    git add .
    git commit -m 'your message'
    git push origin your_branch
    ```
    尝试在设备A上同步更新(即拉取更新)
    ```shell
    git pull origin your_branch
    ```