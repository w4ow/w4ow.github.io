---
title: Git常见问题及解决方法
date: 2021-11-11 13:35
# updated: 0000-00-00 10:00
mathjax: true
meta:
    date: true
categories: 
    - Computer Science
    - Tools
tags:
    - Git
---

Git常见问题及解决方法

---

<!-- more -->

## 1 网络问题

### 1.1 连接GitHub端口超时

**问题**：上传代码到GitHub时报连接超时

```text
ssh: connect to host github.com port 22: Connection timed out
fatal: Could not read from remote repository.
```

**解决方案**

1. 首先测试能否连接到GitHub
    ```bash
    <!-- -T参数表示关闭pseudo-tty的分配，-p参数指定端口 -->
    ssh -T -p 443 git@ssh.github.com
    ```
    若无反应则表示连接不上GitHub。

2. 若连接不上，则向`~/.ssh/config`文件添加如下内容(无该文件则创建)：
    ```shell
    Host github.com
        HostName ssh.github.com
        Port 443
    ```

3. 再次测试，出现一下字样则表示能够连接到GitHub。
    ```shell
    Hi your_github_name! You've successfully authenticated, but GitHub does not provide shell access.
    ```

### 1.2 连接GitHub端口超时

**问题**：与上面不太一样的端口超时

```bash
fatal: unable to access 'https://github.com/***/***.git/': Failed to connect to github.com port 443: Connection timed out
```

**解决方法**

1. 首先尝试能不能ping通github.com，我的无法ping通
2. 前往<https://websites.ipaddress.com/www.github.com>查询当前github.com的实际ip
3. 拷贝实际IP并添加到/etc/hosts文件下，注意权限
    ```shell
    140.82.114.4 github.com
    ```
4. 再次ping发现可以ping通了，此时上传没有问题