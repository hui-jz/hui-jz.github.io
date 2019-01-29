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
[work@a3fbc53d18c9 hui-jz.github.io]$ openssl genrsa -des3 -out ssl.key 1024
Generating RSA private key, 1024 bit long modulus
............................++++++
.....................++++++
e is 65537 (0x10001)
Enter pass phrase for /home/work/app/ssl.key:  //设置ssl.key的密码(huipeng)
Verifying - Enter pass phrase for /home/work/app/ssl.key:   //再次确认密码(huipeng)
[work@a3fbc53d18c9 hui-jz.github.io]$
```
-- 注 -- 
	key文件的密码。不推荐输入。因为以后要给nginx使用。每次reload nginx配置时候都要你验证这个PAM密码的。--
## 去除key文件的密码
```
[work@a3fbc53d18c9 app]$ mv ssl.key xxx.key  // 更改key文件名
[work@a3fbc53d18c9 app]$ openssl rsa -in xxx.key -out ssl.key  //使用 rsa 标准命令从私钥中提取公钥
Enter pass phrase for xxx.key:	//需要输入之前设置的密码(huipeng)
writing RSA key 	//这个提示说明成功取出了秘钥写入成功了
[work@a3fbc53d18c9 app]$
[work@a3fbc53d18c9 app]$rm xxx.key 		//xxx.key 没用了删除掉.
```
```
[work@a3fbc53d18c9 app]$ openssl req -new -key ssl.key -out ssl.csr
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [XX]:CN
Common Name : bigdata.hn.sgcc.com.cn
Common Name : sharebigdata.hn.sgcc.com.cn
Email Address : xmz_flx@hn.sgcc.com.cc
```





## 生成crt证书文件
-- 最后根据这2个文件生成crt证书文件
```
[work@a3fbc53d18c9 app]$ openssl x509 -req -days 3650 -in ssl.csr -signkey ssl.key -out ssl.crt
Signature ok
subject=/C=CN/L=Default City/O=Default Company Ltd/CN=advanced.hui/emailAddress=zhangkongchengshi@aliyun.com
Getting Private key
[work@a3fbc53d18c9 app]$

openssl x509 -req -days 3650 -in ssl.csr -signkey ssl.key -out ssl.crt
openssl x509 -req -days 3650 -in server.csr -CA ca.crt -CAkey server.key -CAcreateserial -out server.crt
```

	ca.crt ca.srl server.crt server.csr server.key

	ssl.crt
	ssl.csr
	ssl.key


./configure --prefix=/home/work/env/nginx --sbin-path=/home/work/env/nginx/sbin/nginx --conf-path=/home/work/env/nginx/conf/nginx.conf --error-log-path=/home/work/env/nginx/error-log --pid-path=/home/work/env/nginx/run/nginx.pid --lock-path=/home/work/env/nginx/lock/nginx.lock --user=work --group=work --with-http_ssl_module

make && make install


## genrsa 参数介绍:
	genrsa		//genrsa 标准命令生成私钥
	-des3		//-des/-des3/-idea：不同的加密算法
	-out 		//指定生成ssl.key的路径.
	1024 		//指定生成私钥的大小，默认是2048.
	-new 		//表示生成一个新证书签署请求
	-x509 		//专用于CA生成自签证书，如果不是自签证书则不需要此项
	-key 		//生成请求时用到的私钥文件
	-out 		//证书的保存路径
	-days 		//证书的有效期限，单位是day（天），默认是365天
	-signkey 	//指定签名钥匙










Proc-Type: 4,ENCRYPTED
DEK-Info: DES-EDE3-CBC,C16D0170689FB594

EuT7WOHzI6Ga5znXmx9SuYUL5fyy5oEb1Ef4cXKkjbRgz2n9v5W/en3NpB9eK+3t
0zEHykl8zYYWg88d9q67+TyengFS9Std+uCKYWPKBaSaEN/jcL4HwNEvNx96mhjP
5Tj1oiGHQORtXW4nHQBEbdPjc0B6+EhYuG48sVNNq6syY/NQu8hOG9jMKrWmiGGq
/7F0Jl9tlbbDyh0YgNb3HnuNnKYLlXx+HDpi2MohMQNx/jo2uZNVUI2/JJX0hklZ
P3sodCoh7PbfqY004Om/+C0IynzIxdCxNM8UynMYh1+XXj2UiFWzDhCtz3Myv7aD
l8sBtf0Tjq4JvYWMI1Y0JRmnzTKqtcozME8mflfeDzCrHcTnFM0u0ZUnH1bJadKG
Zk7rjCyzQAKI8VTsYD0J6deSl0h7Mx85Vr2fJaAJtxWrP6eUCVNqeTbqxifBiJqn
uUBxUAUCP1x+UwpH6N2FTibfH6Fan+WiaOr2aYjP4gS5YPiZ6/Y6s8cSK+JLKHt8
w1+aEfYmGjkNLAIElaenTsXj5zn/9JQi8lPJ3KbBtb9bHgeQZzqj8hLcdtANEb8V
/xp+krJHd84g8UBsJX7LZNyKyGvG8mLiz66SSeyNCWzOcJ/kPrVBXVvj7q5LryS1
eakoqpT23RtCyV+4uc9+EpqSh7aSjr3d5q0sTWxmk9E6Z0c8lGxlyh4jnHGbrVD9
qZc3feFTwnSbSXMAFQe5zUYZYt+d/Fdmo79ePhlDzQYF3MAz1jz9Rj6BOrbpOT+d
VbBJUIuMWrn+XZUJdrtBqwtSi7Fsx2OwlvMFdCLUUKI=
-----END RSA PRIVATE KEY-----
