---
title: 分布式系统 - 内存数据库 Redis
date: 2018-05-16 00:11:36
tags:
- 分布式
- 系统
- 数据库
- Redis
categories:
- 分布式
---

<img style="height:75px" src="https://redis.io/images/redis-small.png" alt="Redis">

## 什么是 NoSql
NoSQL，即 Not-Only SQL，泛指非关系型的数据库。它是为了解决<span style="color: #f44336">高并发、高可用、高可扩展、大数据存储问题</span>而产生的数据库解决方案。
<!-- more -->
NoSQL 可以作为关系型数据库的良好补充，但是不能替代关系型数据库。

## NoSql 数据库分类

**键值(Key-Value)存储数据库**
相关产品： Tokyo Cabinet/Tyrant、<span style="color: #f44336">Redis</span>、Voldemort、Berkeley DB
典型应用： 内容缓存，主要用于处理大量数据的高访问负载。
数据模型： 一系列键值对
优势： 快速查询
劣势： 存储的数据缺少结构化

**列存储数据库**
相关产品：Cassandra, <span style="color: #f44336">HBase</span>, Riak
典型应用：分布式的文件系统
数据模型：以列簇式存储，将同一列数据存在一起
优势：查找速度快，可扩展性强，更容易进行分布式扩展
劣势：功能相对局限

**文档型数据库**
相关产品：CouchDB、<span style="color: #f44336">MongoDB</span>
典型应用：Web 应用（与 Key-Value 类似，Value 是结构化的）
数据模型： 一系列键值对
优势：数据结构要求不严格
劣势：查询性能不高，而且缺乏统一的查询语法

**图形(Graph)数据库**
相关数据库：Neo4J、InfoGrid、Infinite Graph
典型应用：社交网络
数据模型：图结构
优势：利用图结构相关算法。
劣势：需要对整个图做计算才能得出结果，不容易做分布式的集群方案。



## Redis 是什么

Redis 是用 C 语言开发的一个开源的高性能键值对（key-value）数据库。它通过提供多种键值数据类型来适应不同场景下的存储需求，目前为止 Redis 支持的键值数据类型如下：
- 字符串类型
- 散列类型
- 列表类型
- 集合类型
- 有序集合类型

## Redis 的应用场景

- <span style="color: #f44336">缓存（数据查询、短连接、新闻内容、商品内容等等）。</span>
- 分布式集群架构中的 session 分离。
- 聊天室的在线好友列表。
- 任务队列。（秒杀、抢购、12306 等等）
- 应用排行榜。
- 网站访问统计。
- 数据过期处理（可以精确到毫秒）


<span style="color: #f44336">持续更新中...</span>

<img src="/images/Come on/Come on4.gif">
