---
title: 安装NodeJs 
date: 2018-04-09 22:45:48
tags: 
 - Node.js
categories: 
 - 前端
copyright: true
---

#### 下载Node.js

去官网官网：[https://nodejs.org/en/download/](https://nodejs.org/en/download/)

下载后 ftp上传或者命令下载压缩包
<!--more-->
``` 
wget https://nodejs.org/dist/v8.10.0/node-v8.10.0-linux-x64.tar.xz 
```


#### 解压
```
[root@localhost home]# tar -xJf node-v8.10.0-linux-x64.tar.xz
[root@localhost home]# cd /usr/local/data/node-v8.10
```

#### 配置
```
vi /etc/profile
```
在最后加上：
```
export NODE_HOME=/usr/local/data/node-v8.10
export PATH=$NODE_HOME/bin:$PATH
```
让环境变量生效
```
[root@localhost node-v8.10]# source /etc/profile
```
新版本都有自己npm 所以不需要再安装了

#### 测试
```
[root@localhost node-v8.10]# node -v
v8.10.0
[root@localhost node-v8.10]# npm -v
5.6.0
```
