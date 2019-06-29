---
title: 企业级容器 Docker
date: 2019-05-28 14:52:17
tags: 
- 容器
- Docker
- 企业级
categories:
- 微服务架构
---

<img style="height:75px" src="https://i.loli.net/2019/06/28/5d15d197811f938639.png" alt="Docker">
## 什么是 Docker

Docker 是一个开源的应用容器引擎，基于 Go 语言 并遵从Apache2.0协议开源。

Docker 可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现** 虚拟化**。

<!-- more -->

容器是完全使用沙箱机制，相互之间不会有任何接口（类似 iPhone 的 app）,更重要的是容器性能开销极低。

## 什么是虚拟化

虚拟化（英语：Virtualization）是一种资源管理技术，是将计算机的各种实体资源，如服务器、网络、内存及存储等，予以抽象、转换后呈现出来，打破实体结构间的不可切割的障碍，使用户可以比原本的组态更好的方式来应用这些资源。

<span class="text_red">虚拟化技术主要用来解决高性能的物理硬件产能过剩和老的旧的硬件产能过低的重组重用，透明化底层物理硬件，从而最大化的利用物理硬件对资源充分利用。</span>

## 为什么要用 Docker

- 上手快
- 职责的逻辑分类
- 快速高效的开发生命周期
- 鼓励使用面向服务的架构

## 容器与虚拟机比较

容器是在操作系统层面上实现虚拟化，直接复用本地主机的操作系统，而传统方式则是在硬件层面实现。

与传统的虚拟机相比，Docker 优势体现为启动速度快、占用体积小。

## Docker 组件



**Docker 服务器与客户端**

- Docker 是一个客户端-服务器（C/S）架构程序。
- Docker 客户端只需要向 Docker 服务器或者守护进程发出请求，服务器或者守护进程将完成所有工作并返回结果。
- Docker 提供了一个命令行工具 Docker 以及一整套 RESTful API。你可以在同一台宿主机上运行Docker守护进程和客户端，也可以从本地的Docker客户端连接到运行在另一台宿主机上的远程Docker守护进程。

**Docker 镜像与容器**

![Docker 镜像与容器](https://i.loli.net/2019/06/28/5d15de1a6d4a862021.png)




镜像（Image）就是一堆只读层（read-only layer）的统一视角，也许这个定义有些难以理解，下面的这张图能够帮助读者理解镜像的定义。 

![Docker 镜像](https://i.loli.net/2019/06/28/5d15de1a476e139291.png)

- 从左边我们看到了多个只读层，它们重叠在一起。除了最下面一层，其它层都会有一个指针指向下一层。这些层是 Docker 内部的实现细节，并且能够 在主机（译者注：运行 Docker 的机器）的文件系统上访问到。
- 统一文件系统（union file system）技术能够将不同的层整合成一个文件系统，为这些层提供了一个统一的视角，这样就隐藏了多层的存在，在用户的角度看来，只存在一个文件系统。我们可以在图片的右边看到这个视角的形式。 

你可以在你的主机文件系统上找到有关这些层的文件。需要注意的是，在一个运行中的容器内部，这些层是不可见的。在我的主机上，我发现它们存在于`/var/lib/docker/aufs`目录下。 

容器（container）的定义和镜像（image）几乎一模一样，也是一堆层的统一视角，唯一区别在于容器的最上面那一层是可读可写的。 

![Docker 容器](https://i.loli.net/2019/06/28/5d15de1a5e9ae82241.png)

细心的读者可能会发现，容器的定义并没有提及容器是否在运行，没错，这是故意的。正是这个发现帮助我理解了很多困惑。 

> 要点：容器 = 镜像 + 可读层。并且容器的定义并没有提及是否要运行容器。 

**总结**

- 镜像是构建 Docker 的基石。用户基于镜像来运行自己的容器。镜像也是 Docker 生命周期中的“构建”部分。
- 镜像是基于统一文件系统的一种层式结构，由一系列指令一步一步构建出来。
- 镜像当作容器的”源代码”。镜像体积很小，非常“便携”，易于分享、存储和更新。
- 容器（container）的定义和镜像（image）几乎一模一样，也是一堆层的统一视角，唯一区别在于容器的最上面那一层是可读可写的。

**Registry（注册中心）**

Docker 用 Registry 来保存用户构建的镜像。Registry 分为公共和私有两种。Docker 公司运营公共的 Registry 叫做 Docker Hub。用户可以在 Docker Hub 注册账号，分享并保存自己的镜像（说明：在 Docker Hub 下载镜像巨慢，可以自己构建私有的 Registry）。

https://hub.docker.com/

## 安装 Docker 

Docker 官方建议在 Ubuntu 中安装，因为 Docker 是基于Ubuntu发布的，而且一般Docker出现的问题Ubuntu是最先更新或者打补丁的。

> ​注意：这里建议安装在CentOS7.x以上的版本。

**安装**

```
# yum 包更新到最新
sudo yum update
# 安装需要的软件包， yum-util 提供 yum-config-manager 功能，另外两个是 devicemapper 驱动依赖的
sudo yum install -y yum-utils device-mapper-persistent-data lvm2

# 设置 yum 源为阿里云
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
# 安装 docker
sudo yum install docker-ce
# 安装后查看 docker 版本
docker -v
```
**设置ustc的镜像**

ustc 是老牌的 linux 镜像服务提供者了，还在遥远的 ubuntu 5.04 版本的时候就在用。ustc 的 docker 镜像加速器速度很快。ustc docker mirror 的优势之一就是不需要注册，是真正的公共服务。

[https://lug.ustc.edu.cn/wiki/mirrors/help/docker](https://lug.ustc.edu.cn/wiki/mirrors/help/docker)

编辑该文件：`vi /etc/docker/daemon.json`
```
{
    "registry-mirrors": ["https://docker.mirrors.ustc.edu.cn"]
}
```

**Docker的启动与停止**

**systemctl **命令是系统服务管理器指令
```
# 启动 docker
systemctl start docker
# 停止 docker
systemctl stop docker
# 重启 docker
systemctl restart docker
# 查看 docker 状态
systemctl status docker
# 开机启动
systemctl enable docker
# 查看 docker 概要信息
docker info
# 查看 docker 帮助文档
docker --help
```

## 常用命令

**镜像相关命令**


查看镜像 
```
docker images
```
搜索镜像 
```
docker search 镜像名称
```

拉取镜像 

```
docker pull 镜像名称
```

删除镜像 

```
# 按镜像ID删除镜像
docker rmi 镜像ID
# 删除所有镜像
docker rmi `docker images -q`
```

**容器相关命令**

查看正在运行的容器

```
docker ps
```

查看所有容器

```
docker ps –a
```

查看最后一次运行的容器

```
docker ps –l
```

查看停止的容器

```
docker ps -f status=exited
```

**创建与启动容器**

创建容器命令
```
docker run
```

停止容器

```
docker stop 容器名称（或者容器ID）
```

启动容器

```
docker start 容器名称（或者容器ID）
```

文件拷贝

如果我们需要将文件拷贝到容器内可以使用cp命令

```
docker cp 需要拷贝的文件或目录 容器名称:容器目录
```

也可以将文件从容器内拷贝出来

```
docker cp 容器名称:容器目录 需要拷贝的文件或目录
```

删除指定的容器

```
docker rm 容器名称（容器ID）
```


## 应用部署

**MySQL 部署**

```
# 拉取 mysql 镜像
docker pull centos/mysql-57-centos7
# 创建容器
docker run -di --name=tensquare_mysql -p 33306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql
```

-p 代表端口映射，格式为  宿主机映射端口:容器运行端口

-e 代表添加环境变量  MYSQL_ROOT_PASSWORD  是root用户的登陆密码

**Tomcat 部署**

```
# 拉取镜像
docker pull tomcat:7-jre7
# 创建容器  -p表示地址映射
docker run -di --name=mytomcat -p 9000:8080 -v /usr/local/webapps:/usr/local/tomcat/webapps tomcat:7-jre7
```

**Nginx 部署** 


```
# 拉取镜像 
docker pull nginx
# 创建 Nginx 容器
docker run -di --name=mynginx -p 80:80 nginx
```

**Redis 部署**

```
# 拉取镜像 
docker pull redis
# 创建容器
docker run -di --name=myredis -p 6379:6379 redis
```

## 迁移与备份

镜像备份

```
# 容器保存为镜像
docker commit mynginx mynginx_i

# 镜像备份
docker  save -o mynginx.tar mynginx_i
```

首先我们先删除掉 mynginx_img 镜像，然后执行此命令进行恢复

```
# 镜像恢复与迁移
docker load -i mynginx.tar
```

-i 输入的文件

执行后再次查看镜像，可以看到镜像已经恢复



> 更多教程可以参考官方文档，大家可以去[Docker 官方地址](https://www.docker.com/)

<span style="color: #f44336">持续更新中...</span>
<img src="/images/Come on/Come on9.gif">












