---
title: 分布式架构 - 协调服务器 Zookeeper
date: 2019-04-03 10:04:20
tags:
- 分布式
- 架构
- 协调服务器
- Zookeeper
categories:
- 分布式
---
## 什么是 Zookeeper

Zookeeper 是一个分布式开源框架，提供了协调分布式应用的基本服务，它向外部应用暴露一组通用服务——分布式同步（Distributed Synchronization）、命名服务（Naming Service）、集群维护（Group Maintenance）等，简化分布式应用协调及其管理的难度，提供高性能的分布式服务。ZooKeeper本身可以以单机模式安装运行，不过它的长处在于通过分布式 ZooKeeper 集群（一个 Leader，多个 Follower），基于一定的策略来保证 ZooKeeper 集群的稳定性和可用性，从而实现分布式应用的可靠性。
<!-- more -->
- Zookeeper 是为别的分布式程序服务的
- Zookeeper 本身就是一个分布式程序（只要有半数以上节点存活，zk 就能正常服务）
- Zookeeper 所提供的服务涵盖：主从协调、服务器节点动态上下线、统一配置管理、分布式共享锁、统> 一名称服务等
- 虽然说可以提供各种服务，但是 zookeeper 在底层其实只提供了两个功能：管理(存储，读取)用户程序提交的数据（类似 namenode 中存放的 metadata）； 并为用户程序提供数据节点监听服务；

## Zookeeper 集群机制

Zookeeper 集群的角色： Leader 和 follower 

只要集群中有半数以上节点存活，集群就能提供服务

## Zookeeper 特性

- Zookeeper：一个 leader，多个 follower 组成的集群
- 全局数据一致：每个 server 保存一份相同的数据副本，client 无论连接到哪个 server，数据都是一致的
- 分布式读写，更新请求转发，由 leader 实施
- 更新请求顺序进行，来自同一个 client 的更新请求按其发送顺序依次执行
- 数据更新原子性，一次数据更新要么成功，要么失败
- 实时性，在一定时间范围内，client 能读到最新数据

## Zookeeper 数据结构


## Zookeeper 应用场景

数据发布与订阅（配置中心）
发布与订阅模型，即所谓的配置中心，顾名思义就是发布者将数据发布到ZK节点上，供订阅者动态获取数据，实现配置信息的集中式管理和动态更新。例如全局的配置信息，服务式服务框架的服务地址列表等就非常适合使用。

负载均衡
这里说的负载均衡是指软负载均衡。在分布式环境中，为了保证高可用性，通常同一个应用或同一个服务的提供方都会部署多份，达到对等服务。而消费者就须要在这些对等的服务器中选择一个来执行相关的业务逻辑，其中比较典型的是消息中间件中的生产者，消费者负载均衡。

命名服务(Naming Service)


分布式通知/协调


集群管理与Master选举

分布式锁

分布式事务

<span style="color:#CA0C16">持续更新中...</span>
<img src="/images/Come on/Come on6.gif">
