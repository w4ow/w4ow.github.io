---
title: Ubuntu 18.10安装与配置
date: 2017-06-04
updated: 2017-06-04
meta:
    date: false
categories: 
    - Computer Science
    - Linux
tags:
    - Ubuntu
---

Ubuntu 18.10安装与配置

<!-- more -->

---

## 1. 安装Ubuntu 18.10

安装过程不多说，安装镜像可以去[中科大镜像](http://mirrors.ustc.edu.cn/)下载。

---

## 2. 安装搜狗输入法

1. 前往[搜狗输入法官网](https://pinyin.sogou.com/linux/?r=pinyin)下载deb安装文件，双击安装；
2. 前往Settings ➜ Region & Language ➜ Nanage Installed Languages， 将键盘输入系统改为fcixt；
3. 当前帐号注销并重新登入，打开应用菜单找到Fcitx Config Tool， 点击右下角的+，取消选定“只展示当前语言”，搜索"sogou"找到并添加搜狗输入法；
4. 重启系统。

---

## 3. 修改软件源

应用菜单 ➜ Software & Updates ➜ Download from ➜ Other ➜ China ➜ mirrors.ustc.edu.cn

---

## 4. 安装Chrome

前往[Chrome官网](https://www.google.cn/intl/zh-CN_ALL/chrome/)下载.deb安装包，双击安装即可。

---

## 5. 开启夜览模式

夜览可以在Setting ➜ Device ➜ Night Light开启，但是默认的暖度过高，需要调整：

1. 打开Terminal安装dconf-editor

```shell
sudo apt install dconf-editor
```

2. 在终端打开dconf-editor

```shell
dconf-editor
```

3. dconf-editor ➜ org ➜ gnome ➜ setting-deamon ➜ plugins ➜ color ➜ night-light-temperature
4. 关闭"Use default value"，并将温度值调到合适的值，一般5500比较合适

---

## 6. 系统美化

安装Tweaks工具用以配置桌面

```shell
sudo apt update
sudo apt install gnome-tweak-tool
```

在应用菜单中打开Tweaks，在Window Titlebars中可以将窗口按钮调至左侧，在Tweaks ➜ Desktop中可以将桌面上令人捉急的Trash图标抹去。默认情况下无法通过Tweaks修改Shell外观，因此需要安装扩展，打开应用中心，找到Add-ons ➜ Shell Extension ➜ User Themes ➜ 安装。推荐的扩展还有：
- Weather In The Clock，点击屏幕上方的时间可以展示天气;
- Dash to Dock，将Ubuntu原生应用栏变得和MacOS一样，不过默认的会自动隐藏，因此需要到应用中心中该扩展的设置里关闭autohide Hide Top Bar，自动隐藏顶部栏；

---

## 7. 配置终端

1. 安装zsh与git

```shell
sudo apt install zsh 
sudo apt install git
```

2. 安装oh-my-zsh

```shell
sudo wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh
```

3. 将Shell切换到zsh，登出并重新登入

```shell
chsh -s /bin/zsh
```

4. 在Home下的.zshrc文件中可以更换主题，个人比较喜欢ys主题

```shell
ZSH_THEME="ys"
```

5. 安装一些强大的zsh插件
首当其冲的插件就是incr，一个超级强大的自动补全插件，下载该插件到Home目录下的/.oh-my-zsh/plugins/incr下，在.zshrc文件中添加命令：

```shell
source ~/.oh-my-zsh/plugins/incr/incr*.zsh
```

6. 同时还推荐一些其他的插件，例如extract，在plugins=()语句括号中添加extract即可。
