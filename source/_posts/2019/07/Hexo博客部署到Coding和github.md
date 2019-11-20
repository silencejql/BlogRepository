---
title: Hexo博客部署到Coding和github
permalink: Hexo博客部署到Coding和github
comments: true
copyright: false
abstract: 交钱！
message: Enter password to read！
date: 2019-07-04 14:48:17
description: 将Hexo部署或迁移到Github或Coding
categories:
- Hexo
tags:
- Coding
- github
top:
password:
---

### 部署到Github和Coding
#### 新建仓库
> Github：
> 新建Repository：name.github.io

> Coding：
>新建Repository：name.coding.me

#### 添加SSH key
若无SSHkey
> ssh-keygen -t rsa -C "your e-mail"
Enter passphrase (empty for no passphrase):<输入加密串>
Enter same passphrase again:<再次输入加密串>

打开生成的id_rsa.pub文件并复制其中的内容添加到Github或Coding项目中然后在git中执行
> Github：
    ssh -T git@github.com

> Coding：
    ssh -T git@git.coding.net //coding
    ssh -T git@git.dev.tencent.com //腾讯云

添加到腾讯云后需要开启Pages服务才可通过name.coding.me登录

#### 更改博客配置文件
```html
deploy:
  type: git
  repository:
    github: git@github.com:silencejql/silencejql.github.io.git
    coding: git@git.dev.tencent.com:silencejql/silencejql.coding.me.git
  branch: master
```
不同格式相应作出调整即可
#### 可能出现的问题
http://name.coding.me 在google chrome中打开可能会默认https，修复方式为:
> 地址栏中输入 chrome://net-internals/#hsts
在 Delete domain security policies 中输入项目的域名，并 Delete 删除
可以在 Query domain 测试是否删除成功

这里如果还是不行， 请清除浏览器缓存！
