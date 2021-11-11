---
title: GitHub常见问题及解决方法
date: 2021-11-11 13:35
# updated: 0000-00-00 10:00
mathjax: true
meta:
    date: true
categories: 
    - Computer Science
    - Tools
tags:
    - GitHub
---

GitHub常见问题及解决方法

---

<!-- more -->

## 1 网络问题

### 1.1 无法连接到GitHub端口

**问题**：上传代码到GitHub时报如下连接端口错误

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

