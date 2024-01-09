---
title: "CentOS/Linux用户管理命令"
draft: false
author: "Song"

tags: ["CentOS", "Linux"]
categories: ["documentation"]

lightgallery: true

toc:
  auto: false
---
## 创建用户

创建用户并设置密码的命令为：

```bash
#只添加用户
sudo useradd your_username

#设置密码, 通过root用户覆盖密码
sudo passwd your_username
```

另一种办法为：

```bash
#添加用户并设置密码
sudo useradd -p password your_username
```



## 如何添加用户到特权(sudoers)组

```bash
sudo visudo
```

```bash
## Allow root to run any commands anywhere 
root    ALL=(ALL)       ALL
user    ALL=(ALL)       ALL
```

