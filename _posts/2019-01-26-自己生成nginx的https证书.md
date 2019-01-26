---
layout:     post
title:      自己生成nginx的https证书
subtitle:   这里说下Linux 系统怎么通过openssl命令生成 证书。
date:       2019-01-26
author:     HuiPeng
header-img: img/post-bg-kuaidi.jpg
catalog: true
tags:
    - https
    - HuiPeng
---

#生成一个key

首先执行如下命令生成一个key
```
[work@a3fbc53d18c9 hui-jz.github.io]$ openssl genrsa -des3 -out /home/work/app/ssl.key 1024
Generating RSA private key, 1024 bit long modulus
............................++++++
.....................++++++
e is 65537 (0x10001)
Enter pass phrase for /home/work/app/ssl.key:  //设置ssl.key的密码
Verifying - Enter pass phrase for /home/work/app/ssl.key:   //再次确认密码
[work@a3fbc53d18c9 hui-jz.github.io]$
```
-- 注 -- 
	key文件的密码。不推荐输入。因为以后要给nginx使用。每次reload nginx配置时候都要你验证这个PAM密码的。--
## genrsa 参数介绍:
	genrsa		//genrsa 标准命令生成私钥
	-des3		//-des/-des3/-idea：不同的加密算法
	-out 		//指定生成ssl.key的路径.
	1024 		//指定生成私钥的大小，默认是2048.
