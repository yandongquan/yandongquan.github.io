---
title: 微服务架构 - Spring Boot
date: 2018-06-14 17:31:18
tags:
- Spring Boot
- 微服务
- 架构
categories:
- 微服务架构
---

## 什么是SpringBoot

SpringBoot 是 Spring 项目中的一个子工程，与我们所熟知的 Spring-framework 同属于 spring 的产品。
<!-- more -->

我们可以看到下面的一段介绍：

> Takes an opinionated view of building production-ready Spring applications. Spring Boot favors convention over configuration and is designed to get you up and running as quickly as possible.

翻译一下：

> 用一些固定的方式来构建生产级别的spring应用。Spring Boot 推崇约定大于配置的方式以便于你能够尽可能快速的启动并运行程序。

其实人们把 Spring Boot 称为搭建程序的<span style="color: #CA0C16">脚手架</span>。其最主要作用就是帮我们快速的构建庞大的 spring 项目，并且尽可能的减少一切 xml 配置，做到开箱即用，迅速上手，让我们关注与业务而非配置。

## 为什么要学习SpringBoot

java 一直被人诟病的一点就是臃肿、麻烦。当我们还在辛苦的搭建项目时，可能 Python 程序员已经把功能写好了，究其原因注意是两点：

- 复杂的配置

项目各种配置其实是开发时的损耗， 因为在思考 Spring 特性配置和解决业务问题之间需要进行思维切换，所以写配置挤占了写应用程序逻辑的时间。

- 一个是混乱的依赖管理。

项目的依赖管理也是件吃力不讨好的事情。决定项目里要用哪些库就已经够让人头痛的了，你还要知道这些库的哪个版本和其他库不会有冲突，这难题实在太棘手。并且，依赖管理也是一种损耗，添加依赖不是写应用程序代码。一旦选错了依赖的版本，随之而来的不兼容问题毫无疑问会是生产力杀手。

我们可以使用 SpringBoot 创建 java 应用，并使用`java –jar `启动它，就能得到一个生产级别的 web 工程。

## SpringBoot的优点
Spring Boot 主要目标是：
- 为所有 Spring 的开发者提供一个非常快速的、广泛接受的入门体验。
- 开箱即用（启动器 starter-其实就是 SpringBoot 提供的一个 jar 包），但通过自己设置参数（.properties），即可快速摆脱这种方式。
- 提供了一些大型项目中常见的非功能性特性，如内嵌服务器、安全、指标，健康检测、外部化配置等。
- 绝对没有代码生成，也无需 XML 配置。

更多细节，大家可以到[Spring boot 官网](http://projects.spring.io/spring-boot/)查看。

## 快速入门

### 创建工程

创建 maven 名为 springboot-demo 的 Spring Boot 工程。

### 添加依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.github.demo</groupId>
    <artifactId>springboot-demo</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.0.RELEASE</version>
    </parent>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
</project>
```

### 编写启动类

```java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

### 编写 controller

```java
@RestController
public class HelloController {

    @GetMapping("hello")
    public String hello(){
        return "hello, spring boot!";
    }
}
```

## 更多技能

- Spring boot 整合 mybatis
- Spring boot 整合 jpa
- Spring boot 整合 redis
- Spring boot 整合 MQ
- Spring boot 整合 ...

更多细节，大家可以到[Spring boot 官网](http://projects.spring.io/spring-boot/)查看。

<img src="/images/Come on/Come on11.gif">
