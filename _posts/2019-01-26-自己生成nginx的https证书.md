---
layout:     post
title:      自己生成nginx的https证书
subtitle:   局域网环境 自己生成nginx的https证书
date:       2019-01-26
author:     HuiPeng
header-img: img/post-bg-kuaidi.jpg
catalog: true
tags:
    - https
    - HuiPeng
---
#自己生成ssl证书
这里说下Linux 系统怎么通过openssl命令生成 证书。

首先执行如下命令生成一个key
openssl genrsa -des3 -out ssl.key 1024