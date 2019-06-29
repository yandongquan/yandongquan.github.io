---
title: 分布式架构 - Nginx
date: 2018-04-15 07:46:13
tags:
- 分布式
- 架构
- Nginx
categories:
- 分布式
---
<img style="height:75px" src="http://nginx.org/nginx.png" alt="Nginx">
## 什么是 Nginx

Nginx 是俄罗斯人编写的一款<span style="color: #f44336">高性能的 HTTP 和反向代理服务器</span>。

在高连接并发的情况下，它能够支持高达 <span style="color: #f44336">50000 个并发</span>连接数的响应，但是内存、CPU 等系统资源消耗却很低，运行很稳定。

<!-- more -->

## Nginx 的优势

选择 Nginx 的理由也很简单：
- 第一，它可以支持5W高并发连接；
- 第二，内存消耗少；
- 第三，成本低，如果采用 F5、NetScaler 等硬件负载均衡设备的话，需要大几十万。而 Nginx 是开源的，可以免费使用并且能用于商业用途。

## 分布式架构中的作用

最常用的有三项：
- **路由功能**（与微服务对应）：域名/路径，进行路由选择后台服务器；
- **负载功能**（与高并发高可用对应）：对后台服务器集群进行负载；
- **静态服务器**（比 Tomcat 性能高很多）：在 mvvm 模式中，充当文件读取职责。

> 总结：实际使用中，这三项功用，会混合使用。比如先分离动静，再路由服务，再负载机器。

## 代理

**正向代理**：客户端自己请求出现困难。客户请了一个代理，来代自己做事，就叫代理。比如代理律师，代购，政府机关办事的代理人等等。
**反向代理**：服务端推出的一个代理招牌。

## Nginx 安装

### 源码编译方式

一般系统中已经装了了 make 和 g++，无须再装。
```shell
# 安装 make
yum -y install autoconf automake make
# 安装 g++
yum -y install gcc gcc-c++
```

安装nginx依赖的库
```shell
# 安装 pcre 
yum -y install pcre pcre-devel
# 安装 zlib 
yum -y install zlib zlib-devel
# 安装 openssl 
yum install -y openssl openssl-devel
```
[Nginx 官方下载地址](http://nginx.org/en/download.html)

安装 Nginx

```liunx
# 下载 Nginx
wget  http://nginx.org/download/nginx-1.16.0.tar.gz
# 解压 Nginx
tar -zxvf nginx-1.16.0.tar.gz
cd nginx-1.16.0
# 安装 HTTPS 模块
./configure   --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module
# 安装
make && make install
```
`--prefix` 指定安装目录
`--with-http_ssl_module` 安装 HTTPS 模块
`make` 编译
`make install` 安装

### yum 方式
Linux 系统下：
```
# yum扩展源
yum install epel-release -y
yum install nginx -y
```

### 目录结构
- Conf：配置文件
- Html：网页文件
- Logs：日志文件
- Sbin：二进制程序

### 启停命令

`./nginx -c nginx.conf`的文件。如果不指定，默认为 NGINX_HOME/conf/nginx.conf
`./nginx -s stop`  停止
`./nginx -s quit` 退出
`./nginx -s reload` 重新加载 nginx.conf

### 发送信号的方式
`kill -QUIT 进程号` 安全停止
`kill -TERM 进程号` 立即停止

## 配置文件

### Nginx 全局属性的配置

```config
#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}
```

**user**：主模块命令， 指定Nginx的worker进程运行用户以及用户组，默认由 nobody 账号运行。
**worker_processes**: 指定 Nginx 要开启的进程数。
**error_log**：用来定义全局错设日志文件的路径和日志名称。
日志输出级别有 debug，info，notice，warn，error，crit 可供选择，其中 debug 输出日志最为详细，而 crit（严重）输出日志最少。默认是 error。

**pid**: 用来指定进程 id 的存储文件位置。
**event**：设定 nginx 的工作模式及连接数上限，其中参数 use 用来指定 nginx 的工作模式（这里是 epoll，epoll 是[多路复用 IO(I/O Multiplexing)](https://www.baidu.com/s?ie=UTF-8&wd=%E5%A4%9A%E8%B7%AF%E5%A4%8D%E7%94%A8%20IO)中的一种方式）,
nginx 支持的工作模式有 select ,poll,kqueue,epoll,rtsig,/dev/poll。
其中 select 和 poll 都是标准的工作模式，kqueue 和 epoll 是高效的工作模式，对于 linux 系统，epoll 是首选。

**worker_connection**：是设置 nginx 每个进程最大的连接数，默认是1024，所以nginx最大的连接数 max_client=worker_processes * worker_connections。
进程最大连接数受到系统最大打开文件数的限制，需要设置 ulimit。

### http 服务器相关属性的配置

```
http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;
```

**include**：主模块命令，对配置文件所包含文件的设定，减少主配置文件的复杂度，相当于把部分设置放在别的地方，然后在包含进来，保持主配置文件的简洁。
**default_type**：默认文件类型，当文件类型未定义时候就使用这类设置的。
**log_format**：设定日志格式。
**sendfile**：开启高效文件传输模式（zero copy 方式），避免内核缓冲区数据和用户缓冲区数据之间的拷贝。
**tcp_nopush**：开启 TCP_NOPUSH 套接字（sendfile 开启时有用）
**keepalive_timeout**：客户端连接超时时间
**gzip**：设置是否开启 gzip 模块

### server 段虚拟主机的配置


```
server {
    listen       80;
    server_name  localhost;

    #charset koi8-r;

    #access_log  logs/host.access.log  main;

    location / {
        root   html;
        index  index.html index.htm;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    #location ~ \.php$ {
    #    root           html;
    #    fastcgi_pass   127.0.0.1:9000;
    #    fastcgi_index  index.php;
    #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    #    include        fastcgi_params;
    #}

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}
```

**listen**：虚拟主机的服务端口
**server_name**：用来指定ip或者域名，多个域名用逗号分开
**charset**：设置字符编码
**location /**：默认请求
    **root**：虚拟主机的网页根目录
    **index**：默认访问首页文件
**error_page**：定义错误提示页面
**location ~ .php$**：PHP 脚本请求全部转发到 FastCGI 处理.，使用FastCGI默认配置。
**location ~ /\.ht**：禁止访问 .htxxx 文件

## Nginx 日志描述

通过访问日志，你可以得到用户地域来源、跳转来源、使用终端、某个URL访问量等相关信息；
通过错误日志，你可以得到系统某个服务或server的性能瓶颈等。

因此，将日志好好利用，你可以得到很多有价值的信息。 

日志格式
打开nginx.conf配置文件：`vi /usr/local/nginx/conf/nginx.conf`
日志部分内容：`#access_log  logs/access.log  main;`

日志生成的到 Nginx 根目录 logs/access.log 文件，默认使用 `main` 日志格式，也可以自定义格式。

<p align="center" style="font-weight: bold;">参数明细表</p>

| 参数          | 说明          |
| --------------------- | ---------------------------------------------------- |
| $remote_addr          | 客户端的ip地址(代理服务器，显示代理服务ip)           |
| $remote_user          | 用于记录远程客户端的用户名称（一般为“-”）            |
| $time_local           | 用于记录访问时间和时区                               |
| $request              | 用于记录请求的url以及请求方法                        |
| $status               | 响应状态码，例如：200成功、404页面找不到等。         |
| $body_bytes_sent      | 给客户端发送的文件主体内容字节数                     |
| $http_user_agent      | 用户所使用的代理（一般为浏览器）                     |
| $http_x_forwarded_for | 可以记录客户端IP，通过代理服务器来记录客户端的ip地址 |
| $http_referer         | 可以记录用户是从哪个链接访问过来的                   |

查看日志命令 `tail -f /usr/local/nginx/logs/access.log`
<span style="color: #f44336">持续更新中...</span>

<img src="/images/Come on/Come on3.gif">
